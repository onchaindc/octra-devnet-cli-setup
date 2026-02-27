# Octra Devnet Reference Client Setup Guide

This guide provides comprehensive instructions for setting up the Octra devnet reference client. Follow the steps based on your operating system: Windows (WSL2), Linux, or macOS.

## Prerequisites
- Ensure you have the latest version of Git installed.
- Node.js and npm must be installed on your system.
- Familiarity with command line operations.

## Setup Instructions

### For Windows (WSL2)
1. **Install WSL2**: Follow the official Microsoft documentation to install WSL2.
2. **Install Linux Distro**: Choose and install a Linux distribution from the Microsoft Store (e.g., Ubuntu).
3. **Open your Linux Terminal**: Access your Linux terminal from the start menu.
4. **Clone the Repository**:
   ```bash
   git clone https://github.com/onchaindc/octra-devnet-cli-setup.git
   cd octra-devnet-cli-setup
   ```
5. **Install Dependencies**:
   ```bash
   npm install
   ```
6. **Setup Wallet Configuration**:
   - Follow the wallet configuration guide provided in the repository.
7. **Run the Application**:
   ```bash
   npm start
   ```

### For Linux
1. **Open Terminal**.
2. **Clone the Repository**:
   ```bash
   git clone https://github.com/onchaindc/octra-devnet-cli-setup.git
   cd octra-devnet-cli-setup
   ```
3. **Install Dependencies**:
   ```bash
   npm install
   ```
4. **Setup Wallet Configuration**:
   - Refer to the wallet configuration guide provided in the repository.
5. **Run the Application**:
   ```bash
   npm start
   ```

### For macOS
1. **Open Terminal**.
2. **Clone the Repository**:
   ```bash
   git clone https://github.com/onchaindc/octra-devnet-cli-setup.git
   cd octra-devnet-cli-setup
   ```
3. **Install Dependencies**:
   ```bash
   npm install
   ```
4. **Setup Wallet Configuration**:
   - Follow the wallet configuration guide.
5. **Run the Application**:
   ```bash
   npm start
   ```

## Troubleshooting
- **Common Issues**:
  - Ensure Node.js and npm are correctly installed.
  - Check if your network connection is stable.
- **Logs**:
  - Review application logs for error messages.

## Tips
- **Windows Users**: Make sure WSL2 is properly configured for optimal performance.
- **Linux Users**: Ensure you have the necessary permissions to run scripts.
- **macOS Users**: Consider using Homebrew for package management.

---

For further assistance, please refer to the official documentation or raise an issue in the repository.