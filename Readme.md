# Allocard — Stellar

Non-custodial corporate expense card infrastructure on Stellar/Soroban. A company delegates scoped, revocable spending authority to an employee or an AI agent — never custody of funds — and every transaction is checked against on-chain caveats (spending caps, recipient allowlists, redemption limits) before it can execute. Employees can redelegate a bounded slice of their own authority to task-specific agents (travel, procurement, reimbursement), and revoking a delegation cascades to everything delegated beneath it.

This is the Stellar/Soroban rebuild of [Allocard on Ethereum](https://github.com/Stoneybro/Allocard), originally built on MetaMask's Smart Accounts Kit (ERC-4337 + ERC-7710 delegation).

**Live demo (Ethereum version):** https://allocard.vercel.app/

## Status

Pre-build — this repo is the target for an active [Stellar Community Fund](https://communityfund.stellar.org/) Build Award submission. `contracts/` will be filled in starting with Tranche 1 (Soroban smart account + policy contract suite).

## Documentation

- [Technical Architecture](docs/TECHNICAL_ARCHITECTURE.md) — full system design: smart accounts, policy composition on OpenZeppelin's `stellar-accounts` framework, redelegation and cascading revocation, the AI policy layer, and security architecture.

## Structure

```
Allocard-stellar/
├── README.md
├── contracts/                      # Soroban contracts (Rust) — added during Tranche 1
└── docs/
    └── TECHNICAL_ARCHITECTURE.md   # full architecture doc
```

## Stack

Stellar / Soroban, OpenZeppelin `stellar-accounts`, Rust (`soroban-sdk`), passkey (secp256r1/WebAuthn) smart accounts, Stellar Asset Contract (XLM/USDC), React + `@stellar/stellar-sdk`.
