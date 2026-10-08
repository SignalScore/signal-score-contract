# SignalScore

Decentralized Governance, Moderation, and Asset Management Infrastructure on Stellar

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](Cargo.toml)

## Project Overview

SignalScore is a programmable, on-chain community governance and asset management protocol built on the Stellar network using Soroban smart contracts. It enables communities and developers to secure digital assets in programmable vaults, manage decentralized content moderation and dispute resolution, and integrate off-chain price feeds via oracles, all while natively wrapping external assets for seamless cross-border utility.

The system serves platforms needing robust infrastructure for secure on-chain token management, external data integration, and decentralized governance. Its main capabilities include securely vaulting tokens, providing on-chain oracle price snapshots, wrapping native Stellar assets 1:1 into Soroban-native tokens, dispensing testnet tokens via a faucet, and managing community content disputes through an on-chain moderation and appeal system.

## Table of Contents

- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Built With / Technology Stack](#built-with--technology-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [Testing](#testing)
- [Smart Contracts](#smart-contracts)
- [License](#license)

## Key Features

- **Token Vault:** Secure token storage with configurable parameters, user balances, spender allowances, and emergency pause/unpause functionality.
- **Oracle:** On-chain price snapshot feeds featuring heartbeat freshness checks and a registry of authorized price-update signers.
- **Wrapped Token:** A mint/burn wrapper that pegs 1:1 with underlying Soroban/Stellar assets, allowing deposits (`wrap`) and redemptions (`unwrap`), alongside admin minting.
- **Content Moderation:** A decentralized flagging system where users can flag content, and designated moderators can review, approve, appeal, and resolve disputes.
- **Faucet:** A token dispenser enforcing strict per-user claim limits and time-based cooldown periods.
- **Web Frontend:** A Next.js-based web interface template ready for smart contract integration.

## Built With / Technology Stack

### Blockchain & Smart Contracts
- **Stellar Network:** The underlying blockchain for deploying and executing the contracts.
- **Soroban Smart Contracts:** The rust-based smart contract platform on Stellar.
- **Rust:** The programming language used for all smart contract logic.

### Frontend
- **Next.js:** The React framework used for the web application frontend.
- **React:** The UI library for building the web interfaces.
- **Tailwind CSS:** Utility-first CSS framework for frontend styling.

## Project Structure

```text
.
├── app/                  # Next.js frontend application pages and layouts
├── src/                  # Rust source code for the Soroban smart contracts
│   ├── errors.rs         # Contract error definitions
│   ├── lib.rs            # Main entry points and Faucet contract implementation
│   ├── moderation.rs     # Content moderation contract implementation
│   ├── oracle.rs         # Oracle price feed contract implementation
│   ├── storage.rs        # Token Vault storage and configurations
│   ├── tests.rs          # Rust unit tests for the contracts
│   ├── types.rs          # Custom data types for the contracts
│   └── wrapped_token.rs  # Wrapped Token contract implementation
├── Cargo.toml            # Rust workspace and dependency manifest
├── package.json          # Node.js frontend dependencies and scripts
└── next.config.mjs       # Next.js configuration
```

## Prerequisites

To build, test, and run this project, you will need the following tools installed:

- **Rust** and `cargo` (Latest stable version recommended)
- **Node.js** (v18 or higher) and **npm**
- **Stellar CLI / Soroban CLI** (For deploying and interacting with the contracts)

## Installation

1. **Install Node.js dependencies for the frontend:**
   ```bash
   npm install
   ```

2. **Build the Rust smart contracts:**
   ```bash
   cargo build --target wasm32-unknown-unknown --release
   ```

## Running the Project

### Frontend Development Server

To run the Next.js web application locally:

```bash
npm run dev
```

The application will be available at `http://localhost:3000`.

### Frontend Production Build

To build and start the Next.js application for production:

```bash
npm run build
npm start
```

## Testing

The smart contracts are unit-tested natively in Rust using the `soroban-sdk::testutils` framework.

To run the smart contract test suite:

```bash
cargo test
```

## Smart Contracts

The repository contains several distinct Soroban smart contracts located within the `src/` directory:

### Faucet Contract (`src/lib.rs`)
- **Purpose:** Dispenses test tokens to users.
- **Important Entry Points:**
  - `initialize(env, admin, token, claim_limit, cooldown)`
  - `claim(env, user, amount)`

### Wrapped Token Contract (`src/wrapped_token.rs`)
- **Purpose:** Wraps existing Stellar/Soroban assets into a Soroban-native token (1:1 peg).
- **Important Entry Points:**
  - `initialize(env, admin, underlying, name, symbol, decimals)`
  - `wrap(env, caller, amount)`
  - `unwrap(env, caller, amount)`
  - `mint(env, admin, recipient, amount)`
  - `burn(env, caller, amount)`

### Oracle Contract (`src/oracle.rs`)
- **Purpose:** Provides on-chain price snapshots with a heartbeat freshness window.
- **Important Entry Points:**
  - `init(env, admin, heartbeat)`
  - `add_signer(env, admin, signer)`
  - `set_price(env, signer, asset, price)`
  - `get_price(env, asset)`

### Token Vault Contract (`src/storage.rs`)
- **Purpose:** Securely vaults tokens and manages user allowances and balances.
- **Configuration:** Features an emergency pause/unpause mechanism controlled by the admin.

### Moderation Contract (`src/moderation.rs`)
- **Purpose:** On-chain dispute and content moderation tracking.
- **Important Entry Points:**
  - `initialize_moderation`
  - `add_moderator`
  - `flag_content`
  - `review_flag`
  - `appeal_flag`
  - `resolve_appeal`

## License

This project is licensed under the MIT License. See the [Cargo.toml](Cargo.toml) file for license details.
