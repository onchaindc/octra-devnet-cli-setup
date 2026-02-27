# Octra Devnet Reference Client Setup Guide

## Introduction
Welcome to the Octra Devnet reference client setup guide! This document provides a comprehensive, step-by-step approach for beginners to set up the Octra devnet client.

## Environment Setup
Before you begin, ensure that you have the following software installed:
- Git: [Download Git](https://git-scm.com/downloads)
- Node.js: [Download Node.js](https://nodejs.org/en/download/)
- A Code Editor: (e.g., Visual Studio Code, Atom)

## Step-by-Step Instructions
1. **Clone the Repository**  
   Open your command line interface (CLI) and run:
   ```bash
   git clone https://github.com/onchaindc/octra-devnet-cli-setup.git
   ```

2. **Navigate to the Directory**  
   ```bash
   cd octra-devnet-cli-setup
   ```

3. **Install Dependencies**  
   ```bash
   npm install
   ```

4. **Configure Environment Variables**  
   Create a `.env` file in the root directory and add the following:
   ```bash
   OCTRA_API_KEY=<your_api_key>
   ```

## Wallet Configuration
1. **Create a Wallet**  
   You can create a new wallet using:
   ```bash
   node createWallet.js
   ```

2. **Load Your Wallet**  
   Use your existing wallet by loading it with:
   ```bash
   node loadWallet.js <path_to_your_wallet_file>
   ```

## Usage Examples
- To check your balance:
   ```bash
   node checkBalance.js
   ```

- To send a transaction:
   ```bash
   node sendTransaction.js
   ```

## Troubleshooting Section
- **Issue**: Cannot connect to the server.  
   **Solution**: Check if your API key is correct and the server is running.

- **Issue**: Wallet not found.  
   **Solution**: Ensure the path to your wallet file is correct.

## Tips for Windows WSL2 Users
- Make sure to enable the Windows Subsystem for Linux and install a Linux distribution from the Microsoft Store (e.g., Ubuntu).
- Use the Linux command line to follow the instructions above.

## Tips for Linux/macOS Users
- You should be able to run the commands directly in your Terminal with no modifications needed.

Happy coding!