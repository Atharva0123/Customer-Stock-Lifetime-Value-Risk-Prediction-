# Customer-Stock-Lifetime-Value-Risk-Prediction-
Financial-services firms earn most of their revenue from a small group of long-term clients, yet they rarely know how long each client will stay or how much each will be worth
# 📈 Multi-Factor Risk, Bankruptcy & Customer Lifetime Value (CLV) Modeling

> Predict how much each client of a **hedge fund / M&A advisory / structured-products** business is worth over the next 12 months, and tie that value to **market risk** and **corporate distress** signals, using survival analysis, cohort analysis and machine learning.

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Platform](https://img.shields.io/badge/Run%20on-Google%20Colab-orange)
![Data](https://img.shields.io/badge/Data-Yahoo%20Finance-purple)
![Status](https://img.shields.io/badge/Status-Educational%20%2F%20Prototype-lightgrey)

---

## ✨ Features

| Module | What it does |
|---|---|
| **Multi-factor market risk** | Volatility, downside volatility, beta, max drawdown, VaR / CVaR, Sharpe, Sortino, momentum |
| **Market Stress Index** | Daily 0-1 score blending volatility and drawdown percentiles |
| **Bankruptcy screening** | Altman Z-Score for your ticker plus peers, with component breakdown |
| **Client data** | Simulated demographics and time-stamped transactions, or plug in your own CSVs |
| **Cohort analysis** | Quarterly retention heatmap and revenue-per-client curves |
| **Feature engineering** | RFM, inter-purchase gaps, 90-day trends, seasonality, behavior under market stress |
| **Survival modeling** | Kaplan-Meier curves, Cox proportional hazards, per-client survival curves |
| **CLV regression** | Ridge, Random Forest, Gradient Boosting, XGBoost, with optional hyper-parameter search |
| **Business dashboard** | Client tiers, Pareto curve, value-vs-flight-risk action map, CSV export |

Every chart is followed by a plain-English **"How to read this"** box for non-technical readers.

---

## 🚀 Quick Start

### Option A: Google Colab (recommended)
1. Open `CLV_Risk_Bankruptcy_Colab.ipynb` in [Colab](https://colab.research.google.com) (File ▸ Upload notebook).
2. Edit the **Control Panel** cell (see below).
3. Runtime ▸ **Run all**.

### Option B: Local
```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook CLV_Risk_Bankruptcy_Colab.ipynb
```
(Remove or comment out the `!pip install` line in the first code cell if you already installed the requirements.)

---

## 🎛️ Control Panel (customize everything in one cell)

```python
TICKER        = "RELIANCE.NS"   # any Yahoo ticker: "AAPL", "TCS.NS", "JPM", "BX"...
BENCHMARK     = "^NSEI"         # "^GSPC" (S&P 500), "^BSESN" (Sensex)...
PEER_TICKERS  = ["TCS.NS", "INFY.NS", "ITC.NS"]

MODEL_CHOICE  = "xgboost"       # "xgboost" | "gbr" | "rf" | "ridge"
LOG_TARGET    = False           # True = better ranking, False = better money accuracy
RUN_TUNING    = False           # True = automatic hyper-parameter search
MODEL_PARAMS  = { "xgboost": dict(n_estimators=400, learning_rate=0.05, max_depth=4, ...), ... }

CLV_HORIZON_DAYS      = 365     # prediction window
CHURN_INACTIVITY_DAYS = 240     # days without a transaction = churned
```

| I want to… | Change |
|---|---|
| Analyze another market/stock | `TICKER`, `BENCHMARK` |
| Try another algorithm | `MODEL_CHOICE` |
| Reduce over-fitting | lower `max_depth` / `learning_rate`, raise `min_child_weight` |
| Improve accuracy | raise `n_estimators` or set `RUN_TUNING = True` |
| Use real client data | `CUSTOMERS_CSV`, `TRANSACTIONS_CSV` |

### Using your own client data
| File | Required columns |
|---|---|
| Customers CSV | `customer_id`, `signup_date`, plus any demographic columns (age, region, segment…) |
| Transactions CSV | `customer_id`, `date`, `amount` |

---

## 🧠 How It Works

```
Yahoo Finance ──► Market risk factors ──► Market Stress Index ─┐
                                                               ▼
Client demographics + transactions ──► Cohort analysis     Feature engineering
                                                               │ (snapshot date: no look-ahead)
                        ┌──────────────────────────────────────┤
                        ▼                                      ▼
              Survival models (KM + Cox)            ML regression (12-month revenue)
                        │                                      │
                        └────────► Blended, survival-adjusted CLV ◄──┘
                                              │
                               Tiers · Pareto · Action map · CSV
```

**Leakage control:** features use only data *before* a snapshot date (`last date − horizon`); the target is revenue *after* it.

---

## 📊 Outputs

- Price, drawdown, rolling volatility/beta, VaR distribution, Market Stress chart
- Altman Z-Score comparison and driver chart
- Cohort retention heatmap and cohort value curves
- Kaplan-Meier curves (overall, by product, by risk group), Cox hazard-ratio plot
- Model comparison, actual-vs-predicted, error histogram, decile lift chart, feature importance
- Segment CLV, tier value share, Pareto curve, value-vs-flight-risk map
- `client_clv_scorecard.csv` with per-client CLV, tier and churn-risk score

---

## ⚠️ Limitations

- Client data is **simulated by default**. Results demonstrate the method and are not real business figures.
- Altman Z is **not suitable for banks/insurers**; Yahoo fundamentals can be incomplete.
- Churned clients are treated as lost (no reactivation model).
- The pipeline uses one snapshot for training; use rolling snapshots and time-based validation for production.
- **Educational use only. Not investment or financial advice.**

---

## 🗺️ Roadmap
- Rolling multi-snapshot training and time-series cross-validation
- BG/NBD + Gamma-Gamma benchmark
- SHAP explanations
- Merton distance-to-default model
- Streamlit dashboard

## 📁 Repository Structure
```
├── CLV_Risk_Bankruptcy_Colab.ipynb   # main notebook
├── requirements.txt
├── PROJECT_REPORT.md                 # detailed project report
└── README.md
```

## 🤝 Contributing
Pull requests are welcome. Open an issue first to discuss major changes.

## 📄 License
MIT (add a `LICENSE` file before publishing).
