# Octra Devnet CLI - Complete Setup Guide

## Prerequisites

Before you begin, ensure you have the following installed on your system:

### System Requirements
- **Python**: Version 3.8 or higher
- **Git**: For cloning the repository
- **pip**: Python package manager (usually comes with Python)
- **Virtual Environment**: Python's venv module (built-in with Python 3.3+)
- **Operating System**: Linux, macOS, or Windows (with WSL2 recommended for Windows)
- **RAM**: Minimum 2GB (4GB recommended)
- **Disk Space**: At least 500MB free space

### Check Your Prerequisites

Before starting, verify your installations:

```bash
# Check Python version
python --version
# or
python3 --version

# Check Git version
git --version

# Check pip version
pip --version
# or
pip3 --version
```

You should see versions like:
- Python 3.8+ (e.g., Python 3.10.0)
- Git version 2.25+
- pip 20.0+

---

## Step 1: Clone the Repository

Clone the Octra Pre-Client repository:

```bash
git clone https://github.com/octra-labs/octra_pre_client.git
cd octra_pre_client
```

If cloning from a fork or custom setup:

```bash
git clone https://github.com/onchaindc/octra-devnet-cli-setup.git
cd octra-devnet-cli-setup
```

---

## Step 2: Set Up Virtual Environment

Creating a virtual environment isolates project dependencies and prevents conflicts with system packages.

### On Linux/macOS:

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# You should see (venv) at the beginning of your terminal prompt
```

### On Windows (Command Prompt):

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
venv\Scripts\activate

# You should see (venv) at the beginning of your terminal prompt
```

### On Windows (PowerShell):

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
venv\Scripts\Activate.ps1

# If you get an execution policy error, run:
# Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## Step 3: Upgrade pip and Install Dependencies

Once your virtual environment is activated:

```bash
# Upgrade pip to the latest version
pip install --upgrade pip

# Install required dependencies
pip install -r requirements.txt
```

If `requirements.txt` doesn't exist, install the core dependencies manually:

```bash
pip install pydantic
pip install rich
pip install typer
pip install httpx
pip install cryptography
pip install ed25519
```

---

## Step 4: Configure Your Wallet

The Octra CLI requires wallet configuration with your private key and address.

### Create wallet.json

Create a file named `wallet.json` in the project root directory:

```bash
# On Linux/macOS
touch wallet.json

# On Windows (Command Prompt)
echo. > wallet.json

# On Windows (PowerShell)
New-Item -Path wallet.json -ItemType File
```

### Configure wallet.json Content

Open `wallet.json` and add your wallet configuration:

```json
{
  "private_key": "your_private_key_here",
  "address": "your_wallet_address_here"
}
```

**Important Security Notes:**
- Keep your private key confidential
- Never commit `wallet.json` to version control
- Add `wallet.json` to your `.gitignore`:

```bash
echo "wallet.json" >> .gitignore
```

### Get Devnet Tokens

To get testnet tokens for the Devnet:

1. Visit the Octra Faucet: **https://faucet-devnet.octra.com**
2. Enter your wallet address
3. Claim your devnet OCT tokens
4. Wait for confirmation (typically a few seconds to a minute)

---

## Step 5: Run the Octra Devnet CLI

### Start the TUI Client

```bash
# Run the main CLI application
python -m octra.cli

# Or if using a specific entry point:
python octra_cli.py
```

### Expected Output

When successful, you should see:
```
╔════════════════════════════════════════════════╗
║     Welcome to Octra Devnet Terminal Client    ║
║         FHE-Powered Blockchain CLI             ║
╚════════════════════════════════════════════════╝

Connected to Devnet
Wallet: your_wallet_address
Balance: X.XX OCT
```

---

## Step 6: Basic CLI Commands

### Check Your Balance

```
> balance
Your balance: 100.00 OCT
```

### Send OCT Tokens

```
> send <recipient_address> <amount>
Example: send octra1abc123def456 50.00
```

### Multi-Send (Send to Multiple Addresses)

```
> multisend
Enter recipient 1: octra1abc123def456
Enter amount 1: 25.00
Enter recipient 2: octra1xyz789uvw123
Enter amount 2: 25.00
Confirm multi-send? (y/n): y
```

### View Transaction History

```
> history
```

### Check Gas Fees

```
> gas-estimate <amount>
```

### Exit the CLI

```
> exit
# or
> quit
# or press Ctrl+C
```

---

## Step 7: FHE Basics - Encrypt/Decrypt Operations

Octra uses Fully Homomorphic Encryption (FHE) for privacy-preserving transactions.

### Encrypt Balance for Privacy

```
> encrypt-balance
Your encrypted balance: [ENCRYPTED_VALUE]
```

### Decrypt Balance

```
> decrypt-balance
Your decrypted balance: 100.00 OCT
```

### FHE Computation Example

```
> compute <operation>
Example: compute add 50 25
Result: 75
```

---

## Troubleshooting

### Issue: Command not found (venv)

**Solution:** Make sure virtual environment is activated:

```bash
# Linux/macOS
source venv/bin/activate

# Windows Command Prompt
venv\Scripts\activate

# Windows PowerShell
venv\Scripts\Activate.ps1
```

### Issue: pip command not found

**Solution:** Use python -m pip:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Issue: Module not found (ModuleNotFoundError)

**Solution:** Reinstall dependencies:

```bash
pip install --upgrade -r requirements.txt
```

### Issue: Connection refused (Cannot connect to Devnet)

**Solution:**
1. Check internet connection
2. Verify Devnet is online: https://devnet.octrascan.io
3. Try connecting with explicit RPC endpoint:

```bash
python -m octra.cli --rpc https://devnet-rpc.octra.com
```

### Issue: Invalid private key in wallet.json

**Solution:**
1. Verify private key format (should be hex string or base64)
2. Ensure private key is correct length (32 bytes / 64 hex chars)
3. Re-check wallet.json syntax (valid JSON)

### Issue: Permission denied on venv activation (macOS/Linux)

**Solution:**

```bash
chmod +x venv/bin/activate
source venv/bin/activate
```

---

## Additional Resources

### Official Links
- **Octra Website**: https://octra.io
- **Devnet Explorer**: https://devnet.octrascan.io
- **Devnet Faucet**: https://faucet-devnet.octra.com
- **GitHub Repository**: https://github.com/octra-labs/octra_pre_client
- **Documentation**: https://docs.octra.io

### Useful Commands Reference

| Command | Description |
|---------|-------------|
| `balance` | View your current balance |
| `send` | Send OCT to another address |
| `multisend` | Send to multiple addresses |
| `history` | View transaction history |
| `encrypt-balance` | Encrypt your balance (FHE) |
| `decrypt-balance` | Decrypt your balance |
| `gas-estimate` | Estimate gas fees |
| `help` | Show all available commands |
| `exit` | Exit the CLI |

---

## Next Steps

1. **Explore the CLI**: Get familiar with all commands
2. **Test Transactions**: Send small amounts to learn the system
3. **Participate in Testnet**: Join community challenges
4. **Read the Docs**: Visit https://docs.octra.io for advanced topics

---

## Support

If you encounter issues:
1. Check this guide's troubleshooting section
2. Review the official documentation
3. Open an issue on GitHub: https://github.com/octra-labs/octra_pre_client/issues
4. Join the Octra community Discord for help

---

**Last Updated:** February 27, 2026
**Guide Version:** 1.0