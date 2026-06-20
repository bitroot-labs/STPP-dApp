# STPP DApp

**STPP (Secure Token Presale Protocol)** is a permissionless, decentralized presale platform built on Ethereum that solves critical problems in token launch mechanisms.

## What is STPP?

STPP combines three complementary mechanisms to create a secure and fair token launch framework:

1. **Commit-Reveal Dutch Auction** — Fair price discovery through cryptographic commitments and time-locked reveals
2. **Liquidity Bootstrapping Pool (LBP)** — Post-auction price formation with gradual weight adjustments and oracle-driven adaptive fees
3. **Token Vesting System** — Controlled distribution with configurable unlocking schedules

## Problems Solved

Traditional presale models suffer from several critical issues that STPP addresses:

- **Unfair Allocation** — Bots and well-connected participants often get preferential treatment
- **Price Manipulation** — Front-running and MEV (Maximal Extractable Value) exploitation distort prices
- **Immediate Dumping** — Token price crashes due to coordinated sell-offs after launch

## How STPP Works

### Security Mechanisms

- **Commit-Reveal Scheme** — Prevents front-running by hiding bid details until reveal phase
- **Merkle Tree Whitelisting** — Efficient access control for early participants
- **Early Participant Bonuses** — Rewards genuine early supporters
- **Oracle-Driven Adaptive Fees** — Automatically adjusts fees and pauses trading during volatility
- **Gradual Vesting** — Prevents coordinated sell-offs through time-locked distributions

### Architecture

STPP uses a **hybrid on-chain/off-chain architecture**:

- **On-Chain**: Critical data (Merkle roots, IPFS CIDs, cryptographic commitments) for transparency and verifiability
- **Off-Chain (IPFS)**: Large datasets (Merkle proofs, bonus allocations) to minimize gas costs
- **Cryptographic Integrity**: Participants can independently verify inclusion through Merkle proofs

This design minimizes transaction costs while maintaining full cryptographic security.

## Who is This For?

This project is **open-source** and designed for:

- **Researchers** studying Web3 presale mechanisms
- **Educators** teaching decentralized finance concepts
- **Developers** building custom token launch protocols
- **Projects** seeking fair and secure token distribution

The codebase is suitable for academic study, educational purposes, and production-grade experimentation.

## Quick Start (TL;DR)

For users familiar with Web3 development, here are the essential steps:

1. **Install dependencies**: `cd STPP-dapp && npm install`
2. **Start Hardhat node**: `cd contract && npx hardhat node` (keep running)
3. **Deploy contracts**: Open new terminal, `cd contract && npm run deploy:all`
4. **(Optional) Start IPFS**: `ipfs daemon` (for uploads only)
5. **Start frontend**: Open new terminal, `cd STPP-dapp && npm run dev`
6. **Connect wallet**: Add Hardhat Local network (RPC: `http://127.0.0.1:8545`, Chain ID: `31337`) and import test account from Hardhat node output
7. **Verify**: Open `http://localhost:3000`, connect wallet, verify connection to chainId 31337

For detailed instructions, see sections below:
- [System Requirements](#system-requirements)
- [How to Run the Project Locally](#how-to-run-the-project-locally)
- [Wallet Setup](#wallet-setup)

## 1️⃣ Architecture Overview

STPP DApp implements a decentralized presale platform with three interconnected layers:

### Smart Contracts

The Solidity contract layer provides the core on-chain functionality:

- **Dutch Auction Contract**: Implements commit-reveal auction mechanism for price discovery, with Merkle tree-based whitelisting and early participant bonus allocation
- **Liquidity Bootstrap Pool (LBP)**: Weighted automated market maker (AMM) for post-auction token trading, with time-based weight adjustments and oracle-driven adaptive fees
- **LBP Oracle**: Circuit breaker oracle that monitors price volatility and automatically pauses trading during rapid price movements, with adaptive fee management
- **Token Vesting Escrow**: Manages gradual token unlocking for participants according to configurable vesting schedules
- **Presale Manager & Factory**: Orchestrates presale lifecycle and enables permissionless presale creation through factory pattern

Contracts store only essential on-chain data (Merkle roots, IPFS Content Identifiers) while delegating large datasets to off-chain storage.

### Frontend

The React-based web application provides the user interface layer:

- Built with Create React App (CRA) and CRACO for build configuration
- Uses wagmi and RainbowKit for Ethereum wallet connectivity and transaction management
- Implements real-time data fetching from smart contracts and IPFS
- Provides interactive interfaces for presale creation, auction participation, LBP trading, and vesting claims

The frontend reads contract addresses and ABIs from deployment artifacts generated during contract deployment.

### IPFS Integration

IPFS serves as the off-chain storage layer:

- **Read Operations**: Frontend fetches whitelist Merkle proofs and bonus allocations from IPFS via public gateways
- **Write Operations**: Deployment and management scripts upload JSON files (whitelist trees, bonus allocations) to IPFS and store resulting CIDs on-chain
- **Hybrid Architecture**: On-chain CIDs enable decentralized data integrity verification while off-chain storage minimizes gas costs

This architecture follows the pattern of storing cryptographic commitments (Merkle roots, hashes) on-chain while storing full data sets off-chain, verified through cryptographic proofs.

## 2️⃣ System Requirements <a id="system-requirements"></a>

### Minimum Required Versions

The following versions represent the minimum requirements for the project to function:

- **Node.js** >= 18.x
- **npm** >= 9.x
- **IPFS (kubo/go-ipfs)** >= 0.20.x
- **Operating System**: macOS, Linux, or Windows (Windows Subsystem for Linux recommended for Windows)

### Tested Configuration

This project has been tested and verified with the following specific versions:

- **Node.js** 24.10.0
- **npm** 11.6.1
- **IPFS** 0.39.0
- **Hardhat** ^2.26.3 (installed as local dependency)

### Local Dependencies

- **Hardhat** ^2.26.3: Installed locally in `contract/` directory and executed via `npx`

> **Note**: Hardhat is installed as a local dependency rather than globally. This ensures version consistency across different environments, avoids conflicts with other projects, and guarantees reproducible builds. All Hardhat commands must be prefixed with `npx` (e.g., `npx hardhat node`).

### Browser Extensions

- **MetaMask** or **Rabby Wallet** browser extension for Web3 interactions


```

**`client/`** - Contains the React frontend built with Create React App (CRA) and CRACO. Handles user interface, wallet connections, and contract interactions.

**`contract/`** - Contains Hardhat workspace with Solidity smart contracts, deployment scripts, and test suite. All contract operations (compile, test, deploy) are executed from this directory.

