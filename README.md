# 🟧 Satoshi Vault Protocol

> **Bitcoin-Secured Decentralized Lending Protocol on Stacks**

---

## **Overview**

**Satoshi Vault Protocol** is a decentralized lending and borrowing infrastructure built on the **Stacks blockchain**, leveraging **Bitcoin’s finality and security**. The protocol enables users to lend, borrow, and earn yield on digital assets through **collateralized loans**, **flash loans**, and **multi-asset liquidity pools** — all enforced by Clarity smart contracts.

By combining **DeFi composability**, **dynamic interest rate models**, and **cross-chain interoperability**, Satoshi Vault establishes a **trustless financial layer** secured by Bitcoin.

---

## **Core Features**

* 💰 **Over-Collateralized Lending:** Secure loans backed by crypto assets with real-time health monitoring.
* ⚖️ **Dynamic Interest Rates:** Algorithmic rate adjustments based on liquidity utilization.
* 🧩 **Multi-Asset Collateralization:** Combine multiple assets for improved capital efficiency.
* ⚡ **Flash Loans:** Single-transaction borrowing for arbitrage, liquidations, or refinancing.
* 🧠 **Credit Scoring System:** Rewards responsible borrowers with improved loan terms.
* 🛡️ **Insurance Fund:** Ensures solvency protection against liquidation losses.
* 🌉 **Cross-Chain Bridges:** Enables seamless movement of assets across ecosystems.
* 🗳️ **Decentralized Governance:** Controlled by holders of the protocol’s governance token.

---

## **System Architecture**

Satoshi Vault is designed as a **modular DeFi system** with tightly scoped smart contract components. Each subsystem (e.g., lending, oracle, governance) interacts through deterministic interfaces and shared data maps.

### **Architecture Diagram (Conceptual)**

```
                           ┌──────────────────────────────┐
                           │     Governance Layer         │
                           │ - Governance Proposals       │
                           │ - Voting, DAO Control        │
                           └──────────────┬───────────────┘
                                          │
┌──────────────────────────┐              │             ┌──────────────────────────┐
│    Lending Subsystem     │ <────────────┼────────────>│   Interest Rate Engine    │
│ - Loan Management        │                            │ - Dynamic Rate Curves     │
│ - Collateral Handling    │                            │ - Utilization Tracking    │
└─────────────┬────────────┘                            └──────────────────────────┘
              │
              │
┌─────────────▼────────────┐        ┌──────────────────────────┐
│     Oracle Subsystem     │        │    Liquidity Pools        │
│ - Price Feeds (Primary)  │        │ - Asset Pools             │
│ - Backup Oracles         │        │ - Utilization Metrics     │
└─────────────┬────────────┘        └─────────────┬────────────┘
              │                                    │
              ▼                                    ▼
     ┌──────────────────────┐             ┌──────────────────────┐
     │  Cross-Chain Bridge  │             │  Insurance Fund       │
     │ - Asset Transfers    │             │ - Reserve Protection  │
     │ - Fee Management     │             │ - Protocol Solvency   │
     └──────────────────────┘             └──────────────────────┘
```

---

## **Contract Architecture**

### **1. Core Parameters & Constants**

Defines the protocol owner, precision constants, and base configurations such as:

* **Loan-to-Value (LTV)** ratio
* **Liquidation penalties**
* **Max uint boundaries**
* **Error codes** for standardized revert logic

---

### **2. User & Credit Management**

**Maps:**

* `users` — tracks total deposits, borrows, and health factor
* `user-credit-score` — maintains credit history and reputation metrics

**Purpose:**
Provides on-chain credit intelligence to enable differentiated borrowing terms for users.

---

### **3. Loan & Collateral Management**

**Maps:**

* `loans` — stores loan metadata including borrower, collateral, interest rate, and status
* `multi-asset-collateral` — supports aggregated collateral positions across multiple assets

**Private Function:**

* `calculate-health-factor` — ensures continuous solvency through ratio checks

---

### **4. Liquidity Pools & Dynamic Interest**

**Maps:**

* `asset-pool` — maintains liquidity, utilization, and availability
* `dynamic-interest-rates` — parameterizes base rate and utilization slopes

**Private Function:**

* `calculate-dynamic-interest-rate` — adjusts borrow/lend rates dynamically based on utilization thresholds

---

### **5. Oracle & Price Feeds**

**Maps:**

* `asset-price-feeds` — current price and update timestamps
* `oracle-price-sources` — primary and backup oracle configurations

Provides redundancy and real-time market integrity for collateral valuations.

---

### **6. Governance Layer**

**Maps:**

* `governance-proposals` — decentralized proposal and voting mechanism
* `LENDING-GOVERNANCE-TOKEN` — fungible governance token

**Functions:**

* `mint-governance-token`
* `update-protocol-parameters`
* `toggle-contract-pause`

---

### **7. Staking & Rewards**

**Maps:**

* `staking-deposits` — staking amount and reward accumulation
* `reward-pool` — tracks pending user rewards

**Functions:**

* `stake-tokens`
* `claim-rewards`

Encourages long-term alignment and liquidity provisioning.

---

### **8. Flash Loans**

Enables **instant, uncollateralized loans** repayable within a single transaction, facilitating:

* Arbitrage
* Collateral swaps
* Liquidations

**Function:**
`flash-loan (asset amount callback-contract callback-function)`

Implements callback-based execution with validation on repayment within transaction context.

---

### **9. Cross-Chain Integration**

**Maps:**

* `cross-chain-bridges` — defines active bridges with configurable fees

**Function:**
`initiate-cross-chain-transfer`
Ensures controlled and verifiable transfers across ecosystems (e.g., Stacks ↔ Bitcoin ↔ other L2s).

---

## **Security Design**

* **Immutable Logic:** All financial computations are deterministic and auditable on-chain.
* **Oracle Redundancy:** Primary and backup oracles mitigate single-point oracle risk.
* **Health Factor Enforcement:** Automatic liquidation triggers maintain solvency.
* **Governance Safeguards:** Only authorized proposals can modify protocol-critical parameters.
* **Insurance Reserve:** Provides buffer for bad debt and systemic losses.

---

## **Data Flow (Simplified)**

```
[ User Deposit ] 
     │
     ▼
[ Liquidity Pool ] ←→ [ Interest Rate Engine ]
     │
     ▼
[ Loan Request ] → [ Oracle Price Feed ] → [ Collateral Valuation ]
     │
     ▼
[ Loan Approval ] → [ Health Factor Monitoring ]
     │
     ▼
[ Borrower Interaction ]
     │
     ▼
[ Repayment or Liquidation ]
```

---

## **Development & Deployment**

### **Tech Stack**

* **Smart Contract Language:** Clarity
* **Blockchain:** Stacks (secured by Bitcoin)
* **Token Standard:** SIP-010 (Fungible Token)

### **Setup**

1. Clone repository

   ```bash
   git clone https://github.com/satoshivault/protocol.git
   cd protocol
   ```

2. Install dependencies

   ```bash
   npm install -g clarinet
   ```

3. Run local tests

   ```bash
   clarinet test
   ```

4. Deploy to testnet

   ```bash
   clarinet deploy --network testnet
   ```

---

## **Future Roadmap**

* 🧩 Integrate decentralized identity for credit-based lending
* 🪙 Expand support for wrapped Bitcoin and Stacks-native stablecoins
* 🧱 Add modular vault strategies and leveraged yield farming
* 🔐 Implement multi-sig based protocol upgrade path

---

## **License**

Licensed under the **MIT License**.
See [`LICENSE`](./LICENSE) for more details.
