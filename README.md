# Smart Contract Wallet + Policy Engine

A smart contract wallet with a risk-based policy engine that evaluates transactions and enforces tiered authorization on-chain through EIP-712 signed attestations.

> **[Read the full documentation](docs/index.md)**

Solidity / Foundry, a Python / FastAPI backend, and a Next.js frontend. The MVP simulator decodes calldata locally rather than running a fork-based simulation. Wallet execution requires a deployed contract and matching RPC, chain, and signing configuration; see the deployment and security guides for the implementation's limits.

## Getting Started

See the [Getting Started guide](docs/getting-started.md) for prerequisites and configuration.

```bash
# Clone with submodules (forge-std, openzeppelin)
git clone --recurse-submodules https://github.com/vaibhavkapur/smart-wallet-policy-engine.git
cd smart-wallet-policy-engine
cp .env.example .env

# Build & test smart contract
forge build
forge test -vvv

# Start backend
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload

# Start frontend (separate terminal, from the repository root)
cd frontend
npm install
npm run dev
```

## Quick Example

Use your deployed wallet and target addresses and the matching chain ID; `31337` is the local Anvil example.

```bash
# Evaluate a transaction against the policy engine
curl -X POST http://localhost:8000/decide \
  -H "Content-Type: application/json" \
  -d '{
    "wallet": "0x1234567890abcdef1234567890abcdef12345678",
    "chain_id": 31337,
    "target": "0xabcdefabcdefabcdefabcdefabcdefabcdefabcd",
    "value": "50000000000000000",
    "data": "0x"
  }'

# Response fields include decision, risk_score, reason_codes,
# tx_type, usd_value, and simulation. Values depend on policy and chain state.
```
