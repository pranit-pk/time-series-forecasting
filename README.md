# NIFTY-50 Log-Return Forecasting  
_Classical Time-Series Models vs LSTM_

## Overview
This project evaluates the effectiveness of classical statistical models and deep
learning for **short-horizon forecasting of NIFTY-50 log returns** using historical
daily market data from 2015 to 2024.

Rather than attempting to build a trading strategy or optimize profit, the goal
is to **rigorously compare modeling approaches under realistic market conditions**
and assess whether increased model complexity provides meaningful predictive gains
in an efficient financial market.

---

## Dataset
- **Source:** NSE India (via Kaggle)
- **Frequency:** Daily
- **Period:** Nov 2015 – Jul 2024
- **Raw Features:**
  - Open
  - High
  - Low
  - Close
  - Shares Traded
  - Turnover (₹ Cr)

### Target Variable
The modeling target is the **daily log return** of the NIFTY-50 index:

\[
r_t = \log\left(\frac{Close_t}{Close_{t-1}}\right)
\]

Log returns were chosen instead of price levels to:
- Enforce stationarity
- Remove trend and scale effects
- Enable fair comparison across models

---

## Problem Statement
Given the previous *N* trading days of log returns, predict the **next-day log
return** of the NIFTY-50 index.

This formulation reflects the realistic difficulty of forecasting short-term
market movements in a highly liquid and information-efficient index.

---

## Evaluation Strategy
To avoid data leakage and preserve temporal structure, **time-aware splits**
were used:

- **Train:** 2015–2021  
- **Validation:** 2022–2023  
- **Test:** 2024  

Models were evaluated using:
- **RMSE** (Root Mean Squared Error)
- **MAE** (Mean Absolute Error)

All metrics are reported on **log returns**, ensuring direct comparability across
models.

---

## Baseline Model — Naive Zero-Return Predictor
As a minimum benchmark, a naive baseline was implemented that predicts the
next-day log return as **zero**.

This baseline corresponds to the assumption that expected returns are zero,
a common implication of the Efficient Market Hypothesis.

Despite its simplicity, this model provides a strong reference point that more
complex models must outperform to demonstrate genuine predictive value.

---

## Linear Regression (Lagged Returns)
A linear regression model was trained using lagged log returns as input features.

This model captures short-term linear dependencies while remaining interpretable
and computationally simple.

Results showed performance comparable to the naive baseline, indicating that
any linear structure present in daily index returns is weak and short-lived.

---

## ARIMA (Statistical Time-Series Model)
An ARIMA model was applied directly to the log-return series.

- Stationarity was confirmed using the Augmented Dickey–Fuller (ADF) test
- Model orders were selected using AIC-based grid search
- ARIMA(1,0,1) was retained as a parsimonious configuration

Although ARIMA is theoretically well-suited for stationary time series, its
performance was similar to simpler baselines, reinforcing the limited
predictability of daily index returns.

---

## LSTM (Sequence Model on Log Returns)
A Long Short-Term Memory (LSTM) network was implemented to test whether
non-linear sequence modeling could extract additional signal from return
history.

Key design choices:
- **Input:** Sequences of past log returns
- **Sequence length:** 30 trading days (≈ one trading month)
- **Architecture:** Single-layer LSTM with constrained capacity to limit
  overfitting
- **Training:** Early stopping based on validation loss

Unlike price-level prediction, modeling returns removes trend-based illusion
and provides a fair test of whether deep learning offers real advantages.

---

## Model Comparison (Test Set)

| Model                  | RMSE (log returns) | MAE (log returns) |
|------------------------|--------------------|-------------------|
| Naive (Zero Return)    | ~0.0095            | ~0.0059           |
| Linear Regression      | ~0.0094            | ~0.0058           |
| ARIMA (1,0,1)          | ~0.0094            | ~0.0059           |
| **LSTM (seq = 30)**    | **~0.0075**        | **~0.0048**       |

The LSTM achieved a **modest but consistent improvement** over classical
baselines, indicating that limited non-linear temporal structure may exist,
even in highly efficient markets.

Importantly, the gains are small, reflecting the intrinsic difficulty of
short-term return prediction rather than model shortcomings.

---

## Key Insights
- Daily NIFTY-50 returns exhibit near-random behavior with minimal exploitable
  structure
- Simple baselines remain highly competitive
- ARIMA and linear models struggle to outperform the naive benchmark
- LSTM provides **incremental improvement**, not dramatic gains
- Increased model complexity does **not** imply guaranteed performance gains in
  financial time series

---

## Conclusion
This project demonstrates the importance of **problem formulation and evaluation
discipline** in financial machine learning.

By reframing the task from price prediction to log-return forecasting, the analysis
avoids misleading conclusions and provides an honest assessment of model capability
under realistic market conditions.

The results highlight both the **limitations and appropriate use cases** of deep
learning in financial time series modeling.
