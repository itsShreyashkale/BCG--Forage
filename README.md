# PowerCo SME Churn Analysis — BCG Data Science Project

A end-to-end data science project completed as part of the **BCG Data Science Job Simulation**. The goal was to investigate SME customer churn for PowerCo, a major European gas & electricity utility, and build a predictive model to enable targeted retention strategies.

---

## 📌 Project Overview

PowerCo hypothesised that **price sensitivity** was the primary driver of SME customer churn. BCG was engaged to:

1. Validate this hypothesis through exploratory data analysis
2. Engineer meaningful predictive features from raw client and pricing data
3. Build a churn prediction model using a Random Forest classifier
4. Deliver an executive summary with actionable business recommendations

**Key finding:** Price sensitivity is a real but secondary driver. **Net margin and 12-month electricity consumption** are the strongest predictors of churn.

---

---

## 🔍 Task Breakdown

### Task 1 — Business Understanding

Defined the problem: build a binary classifier to predict which SME customers will churn within the next 3 months. Identified the key data sources (client data + price history) and outlined the analytical approach.

### Task 2 — Exploratory Data Analysis (EDA)

- Analysed churn rate (~10% of SME base)
- Identified highly skewed distributions in consumption and forecast features
- Explored churn rates by sales channel, contract type, and gas subscription
- Found that channel `MISSING` had a 7.6% churn rate — a signal worth retaining

### Task 3 — Feature Engineering

Created the following predictive features from raw data:

| Feature | Description |

| `offpeak_diff_dec_january_energy` | Price difference between Dec and Jan off-peak periods |
| `var_6m_price_*` / `var_year_price_*` | Price variance across 6-month and yearly windows |
| `off_peak_peak_var_mean_diff` | Mean price differences across tariff periods |
| `tenure` | Number of months as a PowerCo client |
| `months_activ` | Months active until reference date |
| `months_to_end` | Months remaining on current contract |
| `months_modif_prod` | Months since last product modification |
| `has_gas` | Binary flag: client subscribes to gas (→ higher loyalty) |

Categorical features (`channel_sales`, `origin_up`) were one-hot encoded. Skewed numerical features were log-transformed.

### Task 4 — Predictive Modelling

- **Model:** Random Forest Classifier (`n_estimators=1000`, 75/25 train-test split)
- **Target:** Binary churn label (1 = churned within 3 months)

**Model Performance:**

| Metric | Score |
| Accuracy | 90.4% |
| Precision | 81.8% |
| Recall | 4.9% |
| True Negatives | 3,282 / 3,286 |
| True Positives | 18 / 366 |

**Top Feature Importances:**

1. Net margin (`net_margin`)
2. 12-month electricity consumption (`cons_12m`)
3. Margin on power subscription (`margin_gross_pow_ele`)
4. Months active (`months_activ`)
5. Client tenure
6. Off-peak Dec–Jan price difference (the originally hypothesised feature)

### Task 5 — Executive Summary

Synthesised all findings into a single BCG-style executive summary slide using the SCQA framework (Situation → Complication → Key Question → Recommended Solution), presented to the Head of the SME division.

---

## 💡 Key Insights

- **Price is not the #1 churn driver.** Net margin and consumption volume matter more — financially valuable, high-consuming customers are less likely to churn.
- **Tenure is a critical early indicator.** Customers with fewer than 4 months of tenure churn at significantly higher rates. Crossing the 4-month mark is a key retention milestone.
- **Multi-product customers are more loyal.** Clients with both gas and electricity subscriptions churn ~2% less, reflecting higher switching costs.
- **The model has high precision (81.8%)** meaning discount budgets won't be wasted on false positives — but low recall (4.9%) means most churners are missed, and retraining with SMOTE oversampling is recommended.

---

## 🚀 Recommendations

1. **Deploy a targeted discount campaign** — Score all SME clients monthly and offer a 20% discount to the top 500 highest-risk customers.
2. **Strengthen new-client onboarding** — Assign dedicated account managers for the first 4 months for all new SME clients.
3. **Upsell gas to electricity-only customers** — Bundling increases switching costs and reduces churn probability.
4. **Improve model recall** — Retrain with SMOTE or cost-sensitive learning to better catch at-risk customers.
5. **Build a price-change alerting system** — Flag customers who experience large off-peak price swings for proactive outreach.

---

## 🛠️ Tech Stack

- **Python 3**
- `pandas`, `numpy` — data manipulation
- `matplotlib`, `seaborn` — visualisation
- `scikit-learn` — Random Forest classifier, train/test split, evaluation metrics

---

## 📊 Data Dictionary

See [`docs/Data_Description (1).pdf`](docs/Data_Description (1).pdf) for full field definitions.

**client_data.csv** — 26 features including consumption, margins, contract dates, channel, and churn label.  
**price_data.csv** — 7 fields covering off-peak, peak, and mid-peak variable and fixed energy prices by month.

---
<img width="1598" height="895" alt="image" src="https://github.com/user-attachments/assets/920de78c-00c5-436f-b7fb-38cd78a6b8be" />

## 🏢 Context

This project was completed as part of the **BCG X Data Science Virtual Experience Programme** on Forage. It simulates real client engagement work, from business problem framing through to stakeholder communication.
