# Wallet Risk Scoring Report

This project analyzes 100 Ethereum wallets that interacted with the Compound V2 protocol. It generates a normalized risk score (0 to 1000) for each wallet based on supply, borrow, repay, and withdrawal activities.

---

## ✨ Deliverables

* **CSV Output File**: `wallet_risk_scores.csv`

  * Columns:

    * `wallet_id`: Ethereum wallet address
    * `risk_score`: Normalized risk score (0-1000 scale)
* **Sample Entry**:

  ```
  wallet_id,risk_score
  0x6e355417f7f56e7927d1cd971f0b5a1e6d538487,400
  ```

---

## 📊 Data Collection

* **Wallet Source**: [Google Sheet](https://docs.google.com/spreadsheets/d/1ZzaeMgNYnxvriYYpe8PE7uMEblTI0GV5GIVUnsP-sBs/edit?usp=sharing)
* **Alchemy API**: Used to pull transaction history for each wallet
* **Compound V2 cToken Contracts**: Queried using the `getAllMarkets()` method on the Comptroller contract
* **Transaction Filtering**: Only transactions involving Compound V2 contracts (e.g., `cUSDC`, `cDAI`, `cETH`) were retained

---

## 🔄 Feature Engineering

Each wallet's activity was aggregated into the following features:

* `total_supplied`
* `total_borrowed`
* `total_repaid`
* `total_withdrawn`
* `tx_count`
* `active_assets`

---

## 📊 Data Cleaning & Processing

* Dropped NFT-related fields (100% null): `erc721TokenId`, `erc1155Metadata`, `tokenId`
* Converted raw hexadecimal values to human-readable floats using known token decimals (e.g., 6 for USDC)
* Classified events: supply, borrow, repay, withdraw using heuristics based on sender, receiver, and token type

---

## 🔢 Scoring Methodology

* Applied PCA (Principal Component Analysis) to reduce all numeric wallet features to 1 dimension
* The resulting component represents the latent risk behavior
* Scores were normalized using MinMaxScaler to a range of 0-1000

---

## 💡 Interpretation of Risk Score

* **Score \~0** ➔ Low Risk (e.g., no borrows, minimal activity)
* **Score \~1000** ➔ High Risk (e.g., high borrow activity, little repayment)

---

## 🔍 Visualization Summary

* Histograms plotted for all wallet features
* Boxplots used to detect and optionally remove outliers

---

## 🚀 Final Output

The file `wallet_risk_scores.csv` contains:

* `wallet_id`
* `risk_score`

It can be used by DeFi analysts to rank wallets by riskiness or to support risk-based access control policies.

---

## 🚀 Technologies Used

* Python
* Pandas
* NumPy
* Web3.py
* Alchemy API
* Matplotlib
* Scikit-learn (PCA, MinMaxScaler)

---

## 📅 Author

Koppala Udayabhanu
