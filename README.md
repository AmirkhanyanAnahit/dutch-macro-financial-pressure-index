# Dutch Macro-Financial Pressure Index for the Netherlands

## Overview
How can we measure **macro-financial stress in the Dutch economy** in a way that is both economically meaningful and forecastable?

This project builds a **monthly Dutch Macro-Financial Pressure Index** by combining key macroeconomic and financial indicators into a single transparent measure of economic pressure. The project then asks a second, practical question:

**Can this pressure index be forecast one month ahead, and which type of predictors works best?**

The notebook is structured in two steps:

1. **Construct** a macro-financial pressure index grounded in economic logic  
2. **Test** multiple forecasting approaches to see what actually predicts it best  

---
## Repository structure
- `dutch_macro_financial_pressure_index.ipynb` — main notebook with the full workflow
- `datasets/` — source datasets used in the analysis
- `requirements.txt` — Python dependencies

---

## Project Motivation
Economic stress does not appear in one variable alone. It shows up through a combination of:
- weakening industrial activity,
- falling confidence,
- labor market deterioration,
- inflation pressure,
- and financial market stress.

This project tries to capture that broader picture in one interpretable indicator for the Netherlands.

The central idea is simple:

> A useful pressure index should not only look economically plausible in hindsight, but also be informative enough to forecast.

---

## Main Questions
This notebook is built around four core questions:

1. **Can a transparent Dutch macro-financial pressure index be constructed from multiple monthly indicators?**
2. **Which forecasting model performs best for one-month-ahead prediction?**
3. **Do lagged macro variables contain more information than fast-moving market indicators?**
4. **Do predictor relationships change across high-pressure vs normal periods?**

---

## Index Construction
The Dutch pressure index is built from five monthly Dutch components:

- **Unemployment rate**
- **Inflation rate**
- **Consumer confidence**
- **Industrial production**
- **AEX volatility**

### Design Principle
Each component is transformed so that:

**higher values always mean more macro-financial pressure**

For example:
- **higher unemployment** = more pressure
- **higher inflation** = more pressure
- **lower consumer confidence** = more pressure
- **weaker industrial production** = more pressure
- **higher equity-market volatility** = more pressure

### Construction Steps
1. Align all series to monthly dates  
2. Transform variables so they move in the same “pressure” direction  
3. Standardize the components  
4. Take an **equal-weighted average** of the standardized series  

The result is a single monthly indicator that summarizes stress in the Dutch macro-financial environment.

---

## Economic Interpretation
A useful composite index should react to major periods of economic stress. In this notebook, the index shows strong peaks around well-known episodes such as:

- the **Global Financial Crisis**
- the **COVID shock**
- the **inflation / energy shock**

Among the strongest peak months identified in the analysis are:

- **2022-10**
- **2008-11**
- **2020-03**

This supports the idea that the index is not arbitrary — it tracks economically meaningful stress.

---

## Forecasting Setup
The main forecasting task is:

**Predict the Dutch pressure index one month ahead**

The notebook compares several forecasting approaches, ranging from simple benchmarks to richer multivariable models.

### Models Tested
- **Naive persistence**
- **AR(1)-style linear benchmark**
- **Contemporaneous multivariable slow-macro model**
- **1-month lagged slow-macro model**
- **3-month lagged slow-macro model**
- **6-month lagged slow-macro model**
- **Fast-indicator model**
- **Ridge regression**
- **Regime-aware forecasting extension**

### Evaluation Design
- The **last 24 monthly observations** are held out for testing
- Models are evaluated using:
  - **MAE** — Mean Absolute Error
  - **RMSE** — Root Mean Squared Error

---

## Predictor Sets

### Slow Macro Predictors
These are variables that may reflect delayed transmission of broader macro conditions:

- Current Dutch pressure index
- Oil price
- ECB deposit facility rate
- Euro-area Business Climate Indicator (BCI)
- German industrial production

### Fast Indicators
These are more immediate market-based or sentiment-based signals:

- VSTOXX
- EURO STOXX 50 return
- EUR/USD return
- Euro-area consumer confidence

---

## Main Results

### One-Month-Ahead Forecasting Performance

| Model | MAE | RMSE |
|---|---:|---:|
| 3-month lagged macro model | 0.1507 | 0.2202 |
| Ridge regression | 0.1525 | 0.2193 |
| 6-month lagged macro model | 0.1759 | 0.2245 |
| 1-month lagged macro model | 0.1857 | 0.2355 |
| Contemporaneous multivariable model | 0.2177 | 0.2734 |
| AR(1)-style linear model | 0.2241 | 0.2866 |
| Fast indicator model | 0.2383 | 0.2894 |
| Naive persistence | 0.2523 | 0.3375 |

### Best Performing Model
The strongest result comes from the:

## **3-month lagged slow macro model**

This is one of the most interesting findings in the project:  
the best predictor of macro-financial pressure is **not the fastest market information**, but rather **lagged macroeconomic transmission**.

---

## Three-Month-Ahead Check
The notebook also includes a separate **3-month-ahead forecasting exercise**.

### Multivariable slow-macro model
- **MAE:** 0.1902
- **RMSE:** 0.2593

### Naive 3-month benchmark
- **MAE:** 0.2470
- **RMSE:** 0.3274

Even at a longer horizon, the structured macro model performs materially better than a simple naive benchmark.

---

## Regime Analysis
The project also tests whether relationships change depending on the level of pressure in the economy.

Two regimes are defined using the **75th percentile** of the pressure index:

- **High-pressure regime**
- **Normal-pressure regime**

### Rolling / expanding evaluation results

| Model | MAE | RMSE |
|---|---:|---:|
| Global expanding model | 0.2938 | 0.4031 |
| Regime-aware expanding model | 0.3160 | 0.4272 |

### Interpretation
The regime-aware approach is an interesting extension, but in this notebook it does **not** outperform the simpler global model.

That is an important result in itself:  
more complexity does not automatically mean better forecasting performance.

---

## Key Findings
- A transparent **Dutch Macro-Financial Pressure Index** can be constructed from macroeconomic and financial indicators.
- The index behaves in an economically plausible way and spikes during major stress episodes.
- **Naive persistence is not enough** to forecast the index well.
- The best-performing one-month-ahead model is the **3-month lagged macro model**.
- **Lagged macro transmission** appears more informative than immediate fast indicators in this sample.
- Ridge regression offers a small stability check, but not a major performance jump.
- A regime-aware extension does **not** beat the simpler global forecasting setup.

---

## Why This Project Is Interesting
This project leads to a more thoughtful workflow:
- build a custom macro-financial measure,
- justify its economic meaning,
- compare multiple forecast approaches,
- and report both positive and negative results honestly.

This makes the notebook relevant for:
- **macro-financial research**
- **economic forecasting**
- **portfolio / analyst-style projects**
- **applied data analysis in economics and finance**

---

## Tools Used
- **Python**
- **pandas**
- **NumPy**
- **matplotlib**
- **scikit-learn**
- **yfinance**

---

## Data Notes
The notebook combines:
- locally stored macroeconomic datasets,
- Dutch and European macro indicators,
- and market data downloaded via `yfinance`.

Because some datasets are referenced through **local Windows file paths**, the notebook is **not fully plug-and-play** on GitHub without path adjustments.

---

## Suggested Repository Structure

```text
project/
├── Dutch_Financial_pressure_Index_portfolio_final.ipynb
├── README.md
├── data/
│   ├── raw/
│   └── processed/
└── figures/
