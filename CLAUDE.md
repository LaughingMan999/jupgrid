# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

JupGrid is a decentralized cryptocurrency grid trading bot for Solana that operates on the Jupiter Limit Order Book. It's a Node.js application that places buy and sell orders based on market price spreads to automate grid trading strategies.

## Commands

### Development Commands
- `npm run lint` - Run StandardJS linting
- `npm run fmt` - Auto-fix code formatting with StandardJS
- `node .` - Start the JupGrid application (main entry point: `src/jupgrid.js`)

### No Test Suite
The project does not currently have automated tests configured.

## Architecture Overview

### Core Files Structure
- `src/jupgrid.js` - Main application file containing the trading logic, order management, and user interface
- `src/settings.js` - Handles environment configuration, encryption/decryption of sensitive data, and user settings persistence
- `src/utils.js` - Utility functions for delays, token data fetching, encryption, and Solana account operations
- `src/jito_utils.js` - Jito blockchain transaction handling utilities
- `src/server.js` - Express server component (minimal)
- `src/tradelog.js` - Trade logging functionality
- `src/custom_onchain.js` - Custom on-chain operations

### Key Application Flow
1. **Initialization**: Environment setup with encrypted private key storage, token selection, and trading parameters configuration
2. **Grid Trading Loop**:
   - Cancel existing orders
   - Check for rebalancing needs
   - Calculate buy/sell prices based on spread percentage
   - Place limit orders through Jupiter API
   - Monitor order status and repeat

### Important Technical Details

#### Security & Configuration
- Private keys are encrypted using AES-192-CBC with user-provided passwords
- Settings are persisted in `userSettings.json` with version checking
- Environment variables stored in `.env` file (RPC_URL, PRIVATE_KEY, FLAG)

#### Trading Logic
- Uses "infinity mode" - targets a specific USD value for Token B
- Maintains two active orders: one buy, one sell
- Implements automatic rebalancing when token ratios drift >3% from target
- Features stop-loss protection and emergency shutdown

#### External Dependencies
- Jupiter Limit Order SDK for order placement
- Solana Web3.js for blockchain interactions
- Jito for transaction bundling and MEV protection
- StandardJS for code formatting (no semicolons, 2-space indentation)

### Development Notes
- The codebase follows StandardJS style guide (no semicolons)
- Uses ES6 modules (`import`/`export`)
- Extensive use of async/await for blockchain operations
- Token data is fetched from Jupiter's strict token list API
- Price data sourced from Jupiter's price API

### Configuration Files
- `package.json` - Contains Jupiter SDK dependencies and Solana Web3 libraries
- `.env` - Stores encrypted RPC URL and private key (created on first run)
- `userSettings.json` - Persists user trading preferences and token selections
- `tokens.txt` - Cached token list from Jupiter API