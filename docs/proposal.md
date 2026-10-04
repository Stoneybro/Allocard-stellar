# Allocard — Non-Custodial Delegated Spending Infrastructure for Stellar

**Website:** https://allocard.vercel.app/
**GitHub:** https://github.com/Stoneybro/Allocard-stellar
**Architecture:** https://github.com/Stoneybro/Allocard-stellar/blob/main/docs/TECHNICAL_ARCHITECTURE.md
**Contact:** zionlivingstone4@gmail.com
**Category:** End-User Application / Infrastructure
**Requested track:** SCF Build Award

---

## Products & Services

Allocard is non-custodial corporate expense card infrastructure. Instead of pre-loading funds onto a card or into an employee's wallet, a company issues **scoped, on-chain, revocable spending delegations**: a signed permission that lets an employee — or an AI agent acting for that employee — spend up to hard limits enforced by a smart contract, not a back-office policy. Employees can further redelegate a slice of their budget to task-specific AI agents (travel booking, procurement, reimbursement claims), each bound by tighter limits than the delegation they were given. Revoking a delegation cascades to everything delegated beneath it.

The submission includes the production deployment of the following components on Stellar/Soroban:

1. **Soroban Smart Account Suite.** A custom `__check_auth` smart account contract supporting multiple signer types (Ed25519, secp256r1/passkey, and policy contracts), plus a factory for deterministic account deployment.
2. **Policy-Signer Contract (the delegation engine).** A reusable Soroban contract — registrable as a smart account signer — that enforces spending caveats: lifetime cap, recurring/periodic allowance, per-transaction cap, recipient allowlist, and redemption-count limit. This is the direct equivalent of MetaMask's ERC-7710 delegation caveats, built natively for Soroban, and is designed to be reusable by any team that needs scoped on-chain spending authority, not just Allocard.
3. **Redelegation & Cascading Revocation.** Nested policy-signer contracts allowing an employee to redelegate a bounded subset of their authority to an agent's smart account, with revocation of a parent delegation automatically invalidating everything delegated beneath it.
4. **AI Policy & Compliance Layer.** An LLM-based decision layer (text + vision) that reads on-chain caveats, natural-language company policy, and per-delegation rules together, returning a pass/reject decision with reasoning for direct spend requests and reimbursement claims (including receipt OCR).
5. **Agent Execution Framework.** Travel and Procurement agents that hold their own Soroban smart accounts, receive scoped redelegations, research options against a budget, and execute approved transactions autonomously; a Reimbursement Agent that redeems a company-to-agent delegation and pays employees back automatically after a policy check.
6. **Delegation Canvas (reference web app).** A React Flow-based interface visualizing the live delegation tree for employers and employees, with drag-to-delegate and one-click revoke — the reference implementation showing how to integrate the contract suite.

All components are delivered as reusable, developer-first infrastructure: the policy-signer contract and redelegation pattern are usable by any team on Stellar needing scoped, revocable spending authority (payroll, treasury sub-accounts, marketplace escrow, agent wallets), with the expense-card app as the flagship reference implementation.

## Requested Budget

**$80,000**

## Success Criteria

**Technical Success:**
- Full delegation lifecycle — delegate, redelegate, spend, revoke — working end-to-end on Stellar Testnet
- Policy-signer contract enforcing all caveat types (lifetime cap, periodic allowance, per-tx cap, allowlist, redemption limit), independently reusable outside the Allocard app

**Ecosystem Adoption:**
- Policy-signer contract and SDK published openly, with at least one integration by a team outside Allocard
- A working reference app (Delegation Canvas) usable by early design-partner companies on testnet

**Community & Compliance:**
- Public technical documentation and threat model for the policy-signer contract
- Feedback from the Stellar developer community incorporated before mainnet deployment

## Go-To-Market Plan

Who we are targeting for early design partners and why:
- Small teams and DAOs that pay contributors or manage recurring vendor spend and need scoped, revocable authority rather than shared custody
- Startups experimenting with AI agents that need to spend on their behalf (travel, procurement, subscriptions) without handing over a funded wallet
- Stellar-native wallets, as passkey-based smart accounts are a natural feature to co-market

What we are offering: a reusable, open policy-signer contract and SDK for scoped spending delegation, with the expense-card app as the reference implementation.

Why this fits SDF's priorities: recurring business spend (payroll-adjacent, vendor payments, agent-driven purchases) is exactly the kind of daily, sticky payment activity that grows real transaction volume on Stellar, and a reusable delegation primitive lowers the barrier for other teams (treasury tooling, marketplace escrow, other agent-wallet projects) to build on Soroban without re-deriving the same access-control logic.

Distribution:
- Open-source the policy-signer contract and SDK so other Soroban builders can adopt the delegation pattern independently of the Allocard app
- Approach 2-3 small companies or DAOs directly as pilot design partners once the testnet MVP is live
- Publish technical write-ups on the caveats engine and redelegation pattern to reach other Stellar builders

## Traction Evidence

Allocard's current architecture — non-custodial spending delegation with on-chain caveats and an AI policy layer — was built and demoed as a working product on Ethereum, using the MetaMask Smart Accounts Kit (ERC-4337 smart accounts, ERC-7710 delegation and redelegation) with Venice AI as the policy and receipt-OCR decision layer. It qualified end-to-end for Best A2A Coordination, Best Agent, and Best Use of AI tracks at the hackathon it was built for.

Live demo (Ethereum version): https://allocard.vercel.app/
Original Ethereum repo: https://github.com/Stoneybro/Allocard
Stellar rebuild repo: https://github.com/Stoneybro/Allocard-stellar

Team lead Zion Livingstone has over four years of experience as a Web3 developer, with a specialization in Fully Homomorphic Encryption (FHE), including a shipped FHE product built with Zama. He is a two-time winner of the Zcash privacy hackathon; that project has since been rebranded and substantially re-architected into what is now Allocard.

---

## Tranche 1 (Deliverable Roadmap) — MVP

**Estimated time:** 5 weeks
**Estimated Budget:** $25,000

- Custom Soroban smart account contract with pluggable signer types (Ed25519, secp256r1/passkey, policy contract), plus a deterministic factory for account deployment
- Policy-signer contract (the caveats engine): lifetime cap, periodic allowance, per-transaction cap, recipient allowlist, and redemption-count limit, composable on a single delegation
- Deploy the full smart account + policy contract suite to Stellar Testnet
- Technical documentation and a short public demo of the delegation flow

**How to verify completion?**
- Public repo with the smart account and policy-signer contracts, deployed and verifiable on Stellar Testnet
- Recorded on-chain flow: a valid delegated spend succeeds, an out-of-policy spend reverts
- Demo video walking through delegation, spend, and a caveat violation

---

## Tranche 2 (Deliverable Roadmap) — Testnet

**Estimated time:** 4 weeks
**Estimated Budget:** $26,000

- Redelegation: nested policy-signer contracts so an employee can redelegate a bounded subset of their authority to an agent's smart account, with child caveats capped at the parent's limits
- Cascading revocation: revoking a parent delegation immediately invalidates every delegation beneath it
- Passkey onboarding (secp256r1/WebAuthn) for seedless account creation, plus sponsored transactions so new users don't need XLM to start
- Testnet QA and an integration demo covering the full delegate → redelegate → revoke lifecycle

**How to verify completion?**
- Public testnet demo: company delegates, employee redelegates to an agent, employer revokes, agent access confirmed dead — all via passkey-signed transactions
- A user can create and control a smart account using only a device passkey, with a fee-sponsored first transaction

---

## Tranche 3 (Deliverable Roadmap) — Mainnet

**Estimated time:** 5 weeks
**Estimated Budget:** $29,000

- AI policy and compliance layer: an LLM-based decision layer (text + vision) checking direct-spend requests and reimbursement claims (including receipt OCR) against on-chain caveats and company policy
- Travel and Procurement agent execution: agent smart accounts that receive redelegated budgets, research options within budget, and execute approved transactions
- Delegation Canvas: a React Flow-based reference web app for employers and employees to delegate, configure caveats, and revoke without touching contract calls directly
- On-chain event indexing for an audit trail, and the policy-signer contract packaged as a standalone, documented SDK
- Mainnet deployment of the reviewed contract suite, settling in USDC (Stellar Asset Contract)

**How to verify completion?**
- End-to-end demo: an employee redelegates to a Travel or Procurement agent, the agent proposes and executes a transaction, confirmed on Stellar Testnet
- Delegation Canvas live and usable by both employer and employee roles against real on-chain state
- Contracts deployed and verifiable on Stellar Mainnet, with one small-value public demonstration transaction

**Grand total: $80,000** across roughly 14 weeks.

---

## Team

**Zion Livingstone, Founder & Lead Engineer**
Over four years of experience as a Web3 developer, specializing in Fully Homomorphic Encryption (FHE), including a shipped FHE product built with Zama. Two-time winner of the Zcash privacy hackathon; that project has since been rebranded and substantially re-architected into what is now Allocard. Will lead all smart contract, AI integration, and application engineering for this proposal.
**GitHub:** https://github.com/Stoneybro
