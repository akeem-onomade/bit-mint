# **BitMint Protocol — Bitcoin-Backed Synthetic Liquidity on Stacks**

## **Overview**

**BitMint Protocol** is a decentralized lending framework designed to turn **idle Bitcoin** into **productive on-chain capital** without compromising custody or market exposure. By leveraging **Stacks' native Bitcoin finality**, the protocol enables trust-minimized borrowing, collateral management, and liquidation operations secured by the Bitcoin network.

Through **overcollateralized lending**, users can **lock BTC-backed assets**, **mint synthetic stablecoins**, and **retain full BTC upside**—unlocking liquidity for DeFi participation, trading, or any on-chain purpose.

---

## **System Overview**

At its core, BitMint operates as a **non-custodial smart contract** deployed on the **Stacks blockchain**, interfacing with **Bitcoin price oracles** for real-time valuation and collateral monitoring.

Users interact with the system through the following lifecycle:

1. **Collateralization** – Lock Bitcoin-backed collateral on-chain.
2. **Minting (Loan Request)** – Borrow synthetic stablecoins against the locked collateral.
3. **Repayment** – Repay loan plus interest to unlock collateral.
4. **Liquidation** – Automatic liquidation if collateral value falls below threshold.

Administrative actors (the contract owner) manage critical parameters such as:

* Collateral ratios and liquidation thresholds.
* Oracle price feeds.
* Platform initialization and upgrades.

The system maintains all state transitions on-chain, ensuring **verifiable accounting**, **transparent solvency**, and **auditability** of all active and historical loans.

---

## **Contract Architecture**

### **Core Contract Components**

| Component                       | Description                                                                        |
| ------------------------------- | ---------------------------------------------------------------------------------- |
| **Initialization & Governance** | Defines ownership, initialization flow, and admin-level controls.                  |
| **Collateral Management**       | Handles BTC-backed collateral deposits and accounting.                             |
| **Loan Engine**                 | Issues, tracks, and settles synthetic loan positions.                              |
| **Price Oracle Registry**       | Maintains validated BTC and supported asset price feeds.                           |
| **Liquidation Logic**           | Enforces collateral safety by monitoring ratios and triggering liquidation events. |

---

### **Key Data Structures**

| Type                         | Name                                                                               | Purpose                                   |
| ---------------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------- |
| **Data Vars**                | `minimum-collateral-ratio`, `liquidation-threshold`, `platform-fee-rate`           | Protocol-level configuration and metrics. |
| **Map: `loans`**             | Stores loan metadata: borrower, collateral, principal, rate, and lifecycle status. |                                           |
| **Map: `user-loans`**        | Indexes user-level active loan IDs for quick access.                               |                                           |
| **Map: `collateral-prices`** | Holds live oracle prices for supported assets (BTC, STX).                          |                                           |

---

### **Functional Modules**

#### **1. Administrative Controls**

* `initialize-platform` – Enables the protocol after deployment.
* `update-collateral-ratio`, `update-liquidation-threshold` – Adjusts risk parameters.
* `update-price-feed` – Updates oracle-fed asset pricing data.

#### **2. User Operations**

* `deposit-collateral` – Records new BTC deposits for use as loan backing.
* `request-loan` – Mints synthetic assets against locked BTC collateral.
* `repay-loan` – Settles outstanding debt and releases collateral.

#### **3. Internal Risk Functions**

* `calculate-collateral-ratio` – Computes dynamic health ratio of loans.
* `calculate-interest` – Accrues per-block loan interest.
* `check-liquidation` / `liquidate-position` – Ensures solvency and initiates liquidation when thresholds are breached.

---

## **Error Codes**

| Code        | Meaning                           |
| ----------- | --------------------------------- |
| `u100`      | Unauthorized access.              |
| `u101`      | Insufficient collateral.          |
| `u102`      | Below minimum requirement.        |
| `u103`      | Invalid amount.                   |
| `u104`      | Platform already initialized.     |
| `u105`      | Platform not initialized.         |
| `u106`      | Invalid liquidation attempt.      |
| `u107–u109` | Loan ID or status errors.         |
| `u110–u111` | Invalid price or asset reference. |

---

## **Data Flow**

1. **User Interaction Layer**
   → User submits collateral & loan requests through front-end or CLI tools.

2. **Smart Contract Execution**
   → Clarity contract validates input, updates maps, and enforces ratio constraints.

3. **Oracle Integration**
   → Admin periodically updates BTC/STX prices using trusted off-chain oracle feeds.

4. **State Transition & Settlement**
   → Loan creation, repayment, and liquidation are executed atomically on-chain.

5. **Transparency & Auditing**
   → All protocol data (loans, ratios, total collateral) retrievable via read-only functions.

---

## **Security Model**

* **Trustless Collateralization**: All positions are recorded on-chain with verifiable proof-of-collateralization.
* **Deterministic Liquidation**: Triggered solely by on-chain conditions, not external agents.
* **Immutable Pricing Source**: Controlled via governed oracle updates by the contract owner.
* **Non-custodial Design**: Users retain BTC exposure at all times; synthetic minting only leverages price-based collateralization.

---

## **Deployment & Configuration**

1. **Deploy the contract** to Stacks mainnet or testnet.
2. **Initialize** via `initialize-platform`.
3. **Set oracle price feeds** using `update-price-feed`.
4. **Tune parameters** (`update-collateral-ratio`, `update-liquidation-threshold`).
5. **Allow users to deposit** and interact with `deposit-collateral` and `request-loan`.

---

## **Future Extensions**

* Integration with **sBTC** and **native Bitcoin L2s** for real BTC collateralization.
* **Dynamic interest rate models** based on utilization.
* On-chain **liquidation auctions** and **insurance backstops**.
* Multi-asset collateral support beyond BTC/STX.

---

## **Summary**

The **BitMint Protocol** represents a secure, composable foundation for **Bitcoin-backed synthetic liquidity** within the Stacks DeFi ecosystem. By combining **on-chain collateral accounting**, **deterministic liquidation**, and **Bitcoin finality**, BitMint bridges the gap between passive BTC holdings and active capital efficiency.
