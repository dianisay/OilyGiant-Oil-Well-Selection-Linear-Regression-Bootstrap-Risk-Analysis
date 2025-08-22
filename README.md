# 🛢️ OilyGiant Oil Well Selection — Linear Regression & Bootstrap Risk Analysis

## 📌 Introduction
Select the best oil wells and region for development by predicting **reservoir volume** and quantifying **profit & risk**.  
We train **Linear Regression (only)** per region, pick the **top 200 wells** out of 500 by predicted reserves, and use **bootstrapping (1,000 samples)** to estimate expected profit, 95% CI, and loss risk. The final choice must keep **loss risk < 2.5%** and maximize average profit.

---

## 🎯 Business objective & constraints
- **Objective:** Choose the region that maximizes expected profit with controlled downside risk.
- **Constraints:**
  - **Model:** Linear Regression only.
  - **Exploration:** 500 candidate wells per region → select **top 200**.
  - **Budget:** 100M USD to develop 200 wells.
  - **Price:** 4.5 USD per barrel; dataset unit is **thousand barrels**, so revenue per unit = **4,500 USD**.
- **Risk rule:** Keep only regions with **loss probability < 2.5%**; pick the highest average profit among them.

---

## 📂 Data
- **Files:** `geo_data_0.csv`, `geo_data_1.csv`, `geo_data_2.csv`
- **Schema:**
  - `id` — well identifier
  - `f0`, `f1`, `f2` — features (important but anonymized)
  - `product` — reserves volume (thousand barrels)

---

## 🧭 Methodology

### 1) Data preparation
- **Checks:** dtypes, missing values, duplicates; keep only `f0`, `f1`, `f2` as features and `product` as target.
- **Split:** Train/Validation = **75/25** per region with fixed seed.
- **Scaling:** Optional for LR stability; no leakage (fit scaler on train only).

### 2) Modeling per region
- **Train:** Linear Regression on training set.
- **Validate:** Predict on validation; store `y_pred` and `y_true`.
- **Report:** Mean predicted reserves and **RMSE** per region; brief error analysis.

### 3) Profit calculation setup
- **Selection:** From validation predictions, pick **top 200** wells per region by `y_pred`.
- **Revenue:** 
  - 

\[
    \text{revenue} = 4500 \cdot \sum_{i \in S_{200}} \hat{y}_i
    \]


- **Profit:** 
  - 

\[
    \text{profit} = \text{revenue} - 100{,}000{,}000
    \]


- **Feasibility check:** Compare average reserves vs the **break-even** requirement (≈111.1 units across 200 wells on average).

### 4) Risk analysis (bootstrapping)
- **Bootstrap (1,000 runs):** Sample with replacement from validation predictions, select top-200 each run, compute profit.
- **Metrics:** Mean profit, **95% confidence interval**, **loss risk** (P[profit < 0]).
- **Decision:** Keep regions with risk < 2.5%; choose the one with highest mean profit.

### 5) Recommendations & sensitivity
- **Pick region:** Based on mean profit and risk threshold.
- **Sensitivity:** Discuss impact of oil price and cost assumptions; note LR-only constraint implications.

---

## 📊 Key analyses & visuals
- **Model quality:** RMSE per region; histogram of residuals.
- **Selection quality:** Distribution of top-200 predicted reserves per region.
- **Profit risk:** Bootstrap profit distribution with 95% CI and loss-risk annotation.

---

## ✅ Results summary
- **Best region:** <Region_k> with **mean profit = $<value>M**, **95% CI [<L>, <U>]**, **loss risk = <p>%**.
- **Model quality:** RMSE by region: 0: <r0>, 1: <r1>, 2: <r2>.
- **Business note:** Development recommended in <Region_k> given controlled downside and superior expected returns.

---

## 🧰 Tech stack
- **Python:** pandas, numpy, scikit-learn (LinearRegression), matplotlib, seaborn
- **Stats:** numpy.random for bootstrapping; scipy (optional) for CIs

---

## 🚀 Repro steps
1. **Install:** `pip install -r requirements.txt`
2. **Data:** Place `geo_data_0.csv`, `geo_data_1.csv`, `geo_data_2.csv` in `datasets/`.
3. **Run:** Execute `oilygiant_linear_bootstrap.ipynb` end-to-end.
4. **Outputs:** Figures → `figures/`, metrics → `reports/`, serialized artifacts (optional) → `models/`.

---

## 📈 Sample figures to include
- **RMSE per region (bar)**
- **Top-200 reserves distribution (violin/box)**
- **Bootstrap profit distribution with 95% CI**

---

## 🤝 Contact
Created by **Diana <Last Name>**  
🔗 [LinkedIn](https://linkedin.com/in/tuusuario) · 🌐 [Portfolio](https://tuportfolio.com)
