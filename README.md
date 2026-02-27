# Octra-Devnet-CLI-Setup-Guide

A beginner-friendly, copy-paste-ready guide to set up the official Octra devnet reference client (octra_ref_client) from scratch — optimized for Windows + WSL2 (Ubuntu 22.04 / 24.04), but also works on native Linux (Ubuntu/Debian) and macOS.

This guide covers:
- full environment requirements
- exact step-by-step install (including libpvac build)
- wallet.json setup (use the devnet RPC IP)
- running the CLI and post-setup tasks (faucet, explorer)
- a troubleshooting section with concrete fixes
- a one-click install script (review before running)

🚨 Read carefully and copy-paste the commands into your terminal. Always review scripts before running them.

---

## Environment (required)
- Best: WSL2 on Windows 11/10 with **Ubuntu 22.04** or **24.04 LTS** ✅  
- Also works on: native Linux (Ubuntu/Debian) and macOS  
- Python: **Python 3.10+**  
- C++ compiler: **g++** supporting **C++17** (required to build libpvac — PVAC/HFHE C++ library)  
- Reason: libpvac is compiled from C++ in pvac/ and is required for "Encrypt Balance" to work

---

## Full step-by-step setup

> Note : just ignore step 1 if you have wsl/wsl2 already. 

1) Install WSL2 (Windows only)
- Open PowerShell as Administrator and run:
```powershell
wsl --install
```
- Restart PC, then open your installed Ubuntu terminal from the Start menu.

2) Update system and install build dependencies (CRITICAL for libpvac)
- Open your Ubuntu terminal (WSL or native) and run:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y g++ make build-essential python3 python3-venv python3-pip python3-dev
```

- for macOS
```bash
xcode-select --install
```

(Ensure g++ is installed and supports C++17. `g++ --version` should show a modern release.)

3) Clone the official Octra reference client
```bash
git clone https://github.com/octra-labs/octra_ref_client.git
cd octra_ref_client
```

4) Make the setup script executable and run it (this does EVERYTHING automatically)
```bash
chmod +x setup.sh
./setup.sh
```

What `setup.sh` does:
- Checks for `g++` and `python3`
- Compiles `libpvac` (PVAC-HFHE C++ library with SIMD support) inside the `pvac/` folder ← THIS IS WHY "Encrypt Balance" works for others but not you if libpvac didn't build
- Creates a Python virtual environment (`venv`)
- Installs all Python dependencies into the venv
- Prints the final run command at the end

5) Create / configure your `wallet.json`
- If `wallet.json` doesn't exist after setup, create it manually:
```bash
nano wallet.json
```
- Paste exactly this structure (replace the placeholders with your values):
```json
{
  "priv": "0xYOUR_PRIVATE_KEY_HERE",
  "addr": "octYOUR_ADDRESS_HERE",
  "rpc": "http://165.227.225.79:8080"
}
```

IMPORTANT: Use the raw IP `http://165.227.225.79:8080` for devnet (the `https://devnet.octra.com` hostname sometimes has issues). Use the above IP RPC in `wallet.json`.

How to get a wallet:
- Generate one with the separate wallet generator repo:
```bash
git clone https://github.com/octra-labs/wallet-gen.git
cd wallet-gen
./wallet-generator.sh
```
- Or import your existing Octra private key (mainnet keys work 100% on devnet)

6) Run the client
```bash
./venv/bin/python3 cli.py
```
You will get a TUI menu with options including:
- Refresh / Check Balance
- Encrypt Balance (Shield public OCT to private HFHE balance) ← this is usually the step that fails when libpvac was not built
- Stealth Transfer
- Decrypt, etc.

---

## Post-setup usage
- Claim faucet (devnet): https://faucet-devnet.octra.com — enter your `oct...` address (10 OCT every 24h)
- After claiming from faucet, go to CLI → **Encrypt Balance**
- Explorer: https://scan.octra.com

---

## Quick verification & helpful commands

- Confirm `g++`:
```bash
g++ --version
```

- Confirm Python:
```bash
python3 --version
```

- Confirm `pvac` build artifacts (after `./setup.sh`):
```bash
ls -la pvac/
# look for .so or shared library files that the Python client imports
```

- If venv exists, always run via venv:
```bash
./venv/bin/python3 cli.py
```
(Do NOT run `python3 cli.py` outside the venv unless you explicitly activate the venv.)

---

## Troubleshooting (common problems & solutions)

- "No such file or directory: requirements.txt"  
  → This repo does NOT use `requirements.txt` in the root. Use `setup.sh` instead!

- `libpvac` build fails / "encrypt balance" errors  
  → Make sure you ran:
  ```bash
  sudo apt install -y g++ make build-essential
  ```
  then re-run:
  ```bash
  ./setup.sh
  ```

- Permission denied when running `setup.sh`  
  → Ensure the script is executable:
  ```bash
  chmod +x setup.sh
  ./setup.sh
  ```

- venv issues (broken venv / dependency errors)  
  → Remove the `venv` folder and re-run `./setup.sh`:
  ```bash
  rm -rf venv
  ./setup.sh
  ```

- Still can't encrypt (proof generation fails)  
  → Verify `pvac/` contains compiled libraries:
  ```bash
  ls -la pvac/
  # you should see .so or similar built objects
  ```
  If missing, the C++ build failed — reinstall g++/build-essential and re-run `./setup.sh`.

- RPC connection error / client cannot talk to devnet  
  → Edit `wallet.json` and ensure `rpc` is set to:
  ```text
  "rpc": "http://165.227.225.79:8080"
  ```
  Hostnames like `https://devnet.octra.com` can sometimes fail — use the raw IP.

---

## Extra tips & notes
- Always run the client with:
```bash
./venv/bin/python3 cli.py
```
(Do not rely on the system Python unless venv is activated.)

- Backup `wallet.json` securely in multiple safe places (do not share your `priv` key).

- This client is devnet-only right now (mainnet merge expected ~1 week from Feb 2026 — verify upstream status before using mainnet).

- Proof generation is slow/unoptimized on devnet — expected durations: **30–500 seconds** on first encrypt (this is normal).

---

## One-click install script (WSL2 / Ubuntu 22.04 / 24.04 — REVIEW BEFORE RUNNING)

This script automates the steps above in an Ubuntu environment. Always read and understand scripts before running them.

Save as `octra-devnet-one-click.sh`, review, then:
```bash
chmod +x octra-devnet-one-click.sh
./octra-devnet-one-click.sh
```

Script contents:
```bash
#!/usr/bin/env bash
set -euo pipefail
echo "=== Octra Devnet Reference Client One-Click Installer ==="
echo "Review this script before running. This is intended for Ubuntu (WSL2/native)."

# Prompt
read -p "Continue? (y/N): " yn
if [[ "${yn,,}" != "y" ]]; then
  echo "Aborted."
  exit 1
fi

# Update + install build deps
echo "Updating and installing build dependencies..."
sudo apt update && sudo apt upgrade -y
sudo apt install -y git g++ make build-essential python3 python3-venv python3-pip python3-dev

# Clone official client
if [ -d "octra_ref_client" ]; then
  echo "octra_ref_client already exists. Skipping clone."
else
  git clone https://github.com/octra-labs/octra_ref_client.git
fi
cd octra_ref_client

# Run setup.sh
if [ -f setup.sh ]; then
  chmod +x setup.sh
  echo "Running setup.sh (this will build libpvac, create venv, and install Python deps)..."
  ./setup.sh
else
  echo "ERROR: setup.sh not found in octra_ref_client. Exiting."
  exit 1
fi

echo ""
echo "=== One-click script finished ==="
echo "If wallet.json does not exist, create it with:"
echo '{
  "priv": "0xYOUR_PRIVATE_KEY_HERE",
  "addr": "octYOUR_ADDRESS_HERE",
  "rpc": "http://165.227.225.79:8080"
}'
echo "Run the client with: ./venv/bin/python3 cli.py"
``` 

---

## Helpful reference table

| Item | Value / Command |
|------|-----------------|
| Official repo to clone | `https://github.com/octra-labs/octra_ref_client.git` |
| Devnet RPC (use this IP) | `http://165.227.225.79:8080` |
| Run client | `./venv/bin/python3 cli.py` |
| Faucet | `https://faucet-devnet.octra.com` |
| Explorer | `https://scan.octra.com` |

---

## Final checklist (before running encrypt)
- [ ] Running on recommended environment (WSL2 Ubuntu 22.04/24.04, or native Linux/macOS)  
- [ ] Python 3.10+ available  
- [ ] g++ / build-essential installed (C++17 support)  
- [ ] `./setup.sh` finished successfully (check `pvac/` for built libs)  
- [ ] `wallet.json` created with `rpc` set to `http://165.227.225.79:8080`  

---

If you want, I can:
- produce a ready-to-commit `README.md` file with this content, or
- create the `octra-devnet-one-click.sh` script file in the repo for you to review and commit.

Which would you like me to do next?
