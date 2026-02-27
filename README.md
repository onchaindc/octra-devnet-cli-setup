# Octra Web Client - Super Simple Setup (WSL / Ubuntu)

**Last updated:** February 27, 2026  
**For Windows users with WSL/Ubuntu** (the fastest & cleanest way)

This is the **official web wallet** for Octra Devnet — runs locally in your browser at `http://127.0.0.1:8420`.

Official repo: https://github.com/octra-labs/webcli

---

## Quick Setup (under 5 minutes)

### 1. Open WSL / Ubuntu Terminal
- Press `Windows key` → type **Ubuntu** (or **WSL**) → open it  
  (or open PowerShell and type `wsl` then Enter)

### 2. Install required packages (one-time)
Copy and paste this single line:

```bash
sudo apt update && sudo apt install -y g++ libssl-dev make
```

Enter your password when asked.

### 3. Clone & Setup the wallet
Now run these commands one by one:
```bash
git clone https://github.com/octra-labs/webcli.git
cd webcli
chmod +x setup.sh
./setup.sh
```

→ This builds everything (libpvac + C++ server). Takes 2–5 minutes.

### 4. Start the web client
After setup finishes, run:
```bash
./octra_wallet
```

Leave this terminal window open (the server must keep running).

### 5. Open in browser
Open Chrome/Edge and go to:

http://127.0.0.1:8420

Done! 🎉

---

## First Time Use
1. Create New Wallet or Import Private Key (Devnet only!)
2. Set a 6-digit PIN (encrypts your wallet locally)
3. Claim test tokens → https://faucet-devnet.octra.com
4. Refresh page → balance shows up

Useful links:
- Explorer: https://scan.octra.com
- Devnet RPC: auto-configured

---

## Troubleshooting

- ./setup.sh fails on missing packages?  
  Run again: sudo apt install -y g++ libssl-dev make

- Want different port?  
  ./octra_wallet 9000 then open http://127.0.0.1:9000

- Server not starting?  
  Close terminal → reopen WSL → cd webcli → ./octra_wallet

- Slow build? Just let it finish — first time is normal.
