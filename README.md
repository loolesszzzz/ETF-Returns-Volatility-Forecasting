# ETF Returns & Volatility Forecasting

## Overview

This repository presents an end-to-end analysis of ETF return and volatility dynamics using classical time-series models.
The project focuses on **understanding predictability, benchmarking models, and interpreting results in a finance and risk-management context**, rather than attempting directional trading strategies.

Using SPY (S&P 500 ETF) daily data, the analysis evaluates:

* The limited predictability of daily returns
* The persistence and clustering of volatility
* The relative performance of simple benchmarks versus more structured models

The emphasis is on **model evaluation, robustness, and business interpretation**.

---

## Objectives

* Assess whether time-series models add value beyond naïve benchmarks
* Compare return forecasting versus volatility forecasting performance
* Demonstrate an end-to-end analytics workflow suitable for finance and risk teams
* Communicate results in business-relevant terms

---

## Data

* **Asset**: SPY (S&P 500 ETF)
* **Frequency**: Daily
* **Period**: 2010–2024
* **Source**: Public market data

Derived datasets include:

* Daily log returns
* 21-day realised volatility (rolling standard deviation)

---

## Methodology

### Return Modelling

* Baseline historical mean
* ARIMA(1,0,1)
* Expanding-window, one-step-ahead forecasting
* Out-of-sample evaluation (2022–2024)

### Volatility Modelling

* Naïve persistence benchmark
* Exponential Smoothing (ETS)
* GARCH(1,1) with conditional variance
* Comparison against realised volatility

### Evaluation Metrics

* Mean Absolute Error (MAE)
* Root Mean Squared Error (RMSE)

---

## Key Findings

### Returns

* Daily returns exhibit minimal predictability
* ARIMA does not materially outperform a simple historical mean
* Results are consistent with the Efficient Market Hypothesis

**Implication**: Return forecasts should not be used for directional trading decisions.

### Volatility

* Volatility displays strong persistence and clustering
* Simple benchmarks perform competitively at short horizons due to volatility's random-walk-like behaviour
* GARCH captures conditional heteroskedasticity and responds rapidly to shocks, making it valuable for risk monitoring despite higher point forecast error

**Implication**: Volatility forecasts are useful for risk monitoring, stress testing, and capital allocation decisions.

---

## Model Performance Comparison

### Returns Forecasting

| Model | MAE | RMSE | vs Baseline |
|-------|-----|------|-------------|
| Baseline Mean | 0.00810 | 0.01103 | — |
| ARIMA(1,0,1) | 0.00826 | 0.01116 | **-1.9%** ↓ |

**Key Takeaway**: ARIMA provides no improvement. Daily return forecasts collapse toward zero, highlighting the dominance of noise over signal. Not suitable for directional trading.

### Volatility Forecasting

| Model | MAE | RMSE | vs Baseline |
|-------|-----|------|-------------|
| Naïve Persistence | 0.000351 | 0.000592 | — |
| ETS | 0.000351 | 0.000592 | **0%** |
| GARCH(1,1) | 0.001493 | 0.002026 | **-325%** ↓ |

**Key Takeaway**: Simple persistence benchmarks excel at one-day horizons. GARCH shows higher forecast error due to comparing conditional variance forecasts against smoothed realised volatility (metric mismatch). However, GARCH's responsiveness to shocks makes it valuable for **risk scenario analysis and tail event monitoring**.

---

## Visual Summary

### Return Forecast Comparison
![Return Forecasts](outputs/figures/return_forecast_comparison.png)
**What this shows**: Forecasts collapse toward zero. Out-of-sample predictability of daily returns is negligible. This confirms that daily equity returns are dominated by noise rather than predictable patterns.

### Volatility Forecast Comparison
![Volatility Forecasts](outputs/figures/volatility_forecast_comparison.png)
**What this shows**:
* ETS behaves like a smoother — excellent at tracking medium-term regimes, low noise, high stability. Lags during sudden jumps.
* GARCH behaves like a shock detector — responds immediately to large return shocks, captures clustering and conditional heteroskedasticity. Noisier in RMSE terms because it forecasts risk, not smooth averages.
* Both models capture volatility structure far better than return models capture direction.

---

## Repository Structure

```
ETF-Returns-Volatility-Forecasting/
│
├── data/
│   ├── raw/                 # Raw market data
│   └── processed/           # Cleaned returns and realised volatility
│
├── notebooks/
│   ├── 01_data_ingestion_and_cleaning.ipynb
│   ├── 02_exploratory_analysis_and_transforms.ipynb
│   ├── 03_return_models_arima.ipynb
│   ├── 04_volatility_models_ets.ipynb
│   ├── 05_volatility_models_garch.ipynb
│   ├── 06_model_evaluation_and_comparison.ipynb
│   └── 07_business_interpretation_and_forecast.ipynb
│
├── src/
│   ├── data_utils.py        # Reusable data-loading utilities
│   ├── diagnostics.py       # Diagnostic and statistical helpers
│   ├── plotting.py          # Reusable plotting functions
│   └── evaluation.py        # Shared evaluation logic
│
├── outputs/
│   ├── figures/             # Final visualisations
│   ├── tables/              # Model comparison tables
│   └── forecasts/           # Saved forecast outputs
│
├── requirements.txt
└── README.md
```

---

## Notes on Design Choices

* **Notebooks are the authoritative analysis** and remain self-contained to preserve reproducibility.
* The `src/` directory contains reusable utilities intended for:

  * Future notebooks
  * Automation or scheduled runs
  * Extension to additional assets or horizons
* Outputs are explicitly saved to separate modelling from consumption and reporting.

---

## Limitations and Extensions

**Limitations**

* Single asset
* One-step forecasting horizon
* Normal error assumption in GARCH
* Realised volatility is backward-looking

**Possible Extensions**

* Multi-asset analysis
* Alternative volatility measures (e.g. Parkinson, Garman–Klass)
* Heavy-tailed GARCH distributions
* Integration with portfolio risk metrics (VaR, ES)

---

## Disclaimer

This project is for educational and analytical purposes only and does not constitute financial advice.
