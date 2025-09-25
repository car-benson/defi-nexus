# 📘 DeFi Nexus Protocol - Bitcoin-Backed Lending on Stacks

## Overview

**DeFi Nexus** is an enterprise-grade decentralized finance (DeFi) protocol enabling Bitcoin holders to unlock liquidity without liquidating their BTC. Built on the **Stacks** blockchain using the **Clarity** smart contract language, the protocol facilitates over-collateralized stablecoin loans against Bitcoin, leveraging trust-minimized architecture, automated risk controls, and dynamic governance mechanisms.

This system emphasizes **capital efficiency**, **real-time liquidation safety**, and **scalable multi-asset support** while preserving protocol solvency and transparency through verifiable on-chain logic.

---

## ✅ Key Features

* 🟠 **Bitcoin-Backed Lending**: Users can deposit BTC as collateral and borrow stablecoins while retaining BTC upside.
* 🔁 **Dynamic Collateralization**: Enforces minimum collateral ratios and automatically adjusts risk parameters.
* 🧠 **Real-Time Price Oracle Integration**: Enables accurate and up-to-date valuation of BTC and other assets.
* 🔐 **Liquidation Protection**: Automatic liquidation triggers when collateral ratio drops below the threshold.
* 🧩 **Multi-Asset Ready**: Easily extendable to support other assets such as STX.
* 🗳️ **Governance Controls**: Secure contract owner permissions for platform configuration and upgrades.

---

## 🔩 System Architecture

```
+-----------------------------+
|       External Users       |
+------------+---------------+
             |
             ▼
+-----------------------------+
|     DeFi Nexus Protocol     |
|       (Clarity Smart       |
|        Contracts on Stacks)|
+------------+---------------+
             |
             ▼
+-----------------------------+
|     Data Maps / Storage     |
|  - Loans                    |
|  - User Loans              |
|  - Price Oracles           |
+------------+---------------+
             |
             ▼
+-----------------------------+
|    Governance Interface     |
|  - Admin Functions          |
|  - Threshold Updates        |
|  - Price Feed Updates       |
+-----------------------------+
```

---

## ⚙️ Contract Architecture

The contract follows a modular structure separating **core lending logic**, **governance**, and **liquidation mechanics**.

### 📁 Constants & Errors

* Error codes (`ERR-*`) for granular transaction validation.
* Platform configuration constants: collateral ratios, liquidation thresholds, fee rates.

### 🧾 Data Variables

* `platform-initialized`: Flag to prevent re-initialization.
* `minimum-collateral-ratio`, `liquidation-threshold`, `platform-fee-rate`: Core protocol risk parameters.
* `total-btc-locked`, `total-loans-issued`: Platform analytics.

### 🗃️ Data Maps

* `loans`: Stores individual loan metadata.
* `user-loans`: Tracks each user’s active loans.
* `collateral-prices`: Oracle-fed BTC/STX price feed mapping.

### 🔒 Private Functions

* `calculate-collateral-ratio`: Evaluates health of a loan.
* `check-liquidation`: Triggers liquidation if collateral ratio falls below threshold.
* `calculate-interest`: Computes interest owed based on elapsed blocks.
* Utility validations: asset checks, price range validation, loan ID verification.

### 🌐 Public Functions

#### Platform Lifecycle

* `initialize-platform`: Bootstraps the protocol (owner only).

#### Lending Operations

* `deposit-collateral`: Registers BTC collateral (off-chain transfer assumed).
* `request-loan`: Issues a loan if collateral meets minimum ratio.
* `repay-loan`: Settles the loan principal + interest, marking the loan as `repaid`.

#### Governance Functions

* `update-collateral-ratio`: Adjust platform's required collateralization ratio.
* `update-liquidation-threshold`: Change liquidation safety margin.
* `update-price-feed`: Admin-updated oracle for BTC/STX pricing.

#### Read-Only Functions

* `get-loan-details`, `get-user-loans`, `get-platform-stats`, `get-valid-assets`

---

## 🔁 Data Flow

1. **User Deposits BTC** *(off-chain tracked, `deposit-collateral` marks logic intent)*
2. **Price Oracle Update** *(via `update-price-feed`)*
3. **Loan Request**:

   * Validates platform state, price, and collateralization.
   * Creates new loan and tracks under user.
4. **Loan Monitoring**:

   * `check-liquidation` (internal call) compares current ratio against threshold.
   * If below threshold, `liquidate-position` is triggered.
5. **Loan Repayment**:

   * Calculates interest from last calculation.
   * Requires user to pay full owed amount before releasing collateral.

---

## 🛠️ Usage Notes

* **BTC Handling**: The contract assumes BTC is handled via an external bridge, custodian, or off-chain mechanism. Clarity itself cannot natively custody BTC.
* **Collateral Tokens**: Although initially supporting BTC, the system is extensible to other assets such as STX via the `VALID-ASSETS` constant.
* **Liquidation**: Automatic through protocol triggers. However, external actors (bots or keepers) can also monitor and call `check-liquidation`.

---

## 🛡️ Security Considerations

* All state-changing operations are gated by strict validations and error codes.
* `tx-sender`-based authorization for user-specific operations and governance.
* Only the contract owner can update price feeds or risk parameters.

---

## 🏁 Getting Started (for Developers)

1. **Deploy the Contract**
2. **Initialize Platform**: `initialize-platform` (owner only)
3. **Update Oracle Price**: `update-price-feed` with BTC/USD
4. **Deposit BTC Collateral**
5. **Request a Loan**
6. **Repay Loan or Trigger Liquidation**

---

## 🔮 Future Improvements

* Oracle decentralization via SIP-010 oracles
* Integration with Bitcoin-native vaults
* Stablecoin minting and integration with Stacks DeFi ecosystem
* Collateral auctions and partial liquidation

---

## 📄 License

This protocol is released under the MIT License. Use at your own risk. Audits and further review are recommended before deployment in production environments.
