# 🧠 Wallet Risk Scoring - Compound Protocol

This project calculates a risk score (0–1000) for Ethereum wallets based on their transaction behavior, focusing on activity around the Compound protocol. It uses transaction data from the Covalent API, processes meaningful features, and computes a weighted risk score using a machine learning model.

---

## 📥 Data Collection

- Used [Covalent API](https://www.covalenthq.com/docs/api/) with Chain ID `1` (Ethereum Mainnet)
- Collected transaction data for 100 wallet addresses
- Extracted:
  - Transaction count
  - Failed transactions
  - Interactions with smart contracts (especially Compound protocol)
  - Gas metrics
  - Timestamps of first and last transaction

---

## 🛠️ Feature Selection

The following features were selected for evaluating wallet behavior:

| Feature | Description |
|--------|-------------|
| `num_txns` | Total number of transactions |
| `num_failed_txns` | Number of failed transactions |
| `unique_contracts` | Unique contracts interacted with |
| `num_compound_interactions` | Compound protocol-specific interactions |
| `total_gas_spent` | Total gas used |
| `avg_gas_price` | Average gas price paid |
| `wallet_age_days` | Days between first and last transaction |

---

## 🧮 Scoring Method

- **Step 1:** A simple proxy risk score was calculated:  
  `proxy_score = num_failed_txns - num_compound_interactions`

- **Step 2:** A `RandomForestRegressor` was trained using the 7 features to predict the proxy score.

- **Step 3:** Model feature importances were used as **weights** to calculate a weighted risk score.

- **Step 4:** The final score was normalized to a range of **0 to 1000**.

---

## ⚠️ Risk Justification

The scoring logic assumes:
- More failed txns indicate poor interaction or bot activity → **higher risk**
- Frequent Compound usage shows DeFi participation → **lower risk**
- Older wallets with more diverse contract activity → **lower risk**
- Inactive or low-gas wallets may be abandoned or inactive → **higher risk**

---

## 📁 Deliverables

- ✅ `data/wallet_features.csv`: Input features for each wallet  
- ✅ `data/wallet_risk_scores.csv`: Final wallet scores

| wallet_id | score |
|-----------|-------|
| 0xfaa0768bde629806739c3a4620656c5d26f44ef2 | 732 |
| ... | ... |

- ✅ `notebooks/risk_scoring_pipeline.ipynb`: Jupyter notebook with full code
- ✅ `requirements.txt`: List of required libraries

---

## 📦 Requirements

Install the required Python libraries:

```bash
pip install -r requirements.txt
```

---

## 🔗 License

This project is for educational and evaluation purposes only.
