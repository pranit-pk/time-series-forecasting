# NIFTY-50 Time Series Forecasting

## Overview
This project focuses on forecasting the next-day closing value of the NIFTY-50
index using historical daily market data from 2015 to 2024.

The goal is not to build a trading strategy, but to evaluate and compare different
time-series modeling approaches on real financial data.

## Dataset
- Source: NSE India (via Kaggle)
- Frequency: Daily
- Period: Nov 2015 – Jul 2024
- Features:
  - Open
  - High
  - Low
  - Close
  - Shares Traded
  - Turnover (₹ Cr)

## Problem Statement
Given the previous 60 trading days of market data, predict the next-day closing
price of the NIFTY-50 index.

## Evaluation Strategy
Models are evaluated using time-aware splits:
- Train: 2015–2021
- Validation: 2022–2023
- Test: 2024

Performance is measured using RMSE and MAE.

## Baseline Model: Naive Persistence

As a minimum performance benchmark, a naive persistence model was used
where the next-day closing price is assumed to be equal to the current
day’s close.

This baseline reflects the strong random-walk characteristics commonly
observed in liquid financial markets and establishes a lower bound that
all subsequent models must outperform.

Despite its simplicity, this baseline achieved competitive performance,
highlighting the difficulty of short-horizon price forecasting.

## Classical Machine Learning Model: Linear Regression

A linear regression model was trained using lagged closing prices as
features to capture short-term linear dependencies in the time series.

The model used the previous five trading days’ closing prices as input
features to predict the next-day close. This approach provides a simple
and interpretable extension of the naive baseline.

The results showed a small but consistent improvement over the naive
model, indicating that limited linear structure exists in the data.
However, the dominance of the most recent lag reinforced the
near-random-walk nature of the index.

## Classical Time-Series Model (ARIMA)

An ARIMA(1,1,1) model was implemented to evaluate whether classical
time-series assumptions could improve next-day price forecasting.

Stationarity was tested using the Augmented Dickey–Fuller (ADF) test.
The closing price series was found to be non-stationary, while first
differencing achieved stationarity, justifying the use of d = 1.

Although ARIMA is theoretically well-suited for random-walk-like
financial series, it underperformed compared to simpler baselines in
this setup. This was primarily due to error accumulation during
recursive multi-step forecasting over the test horizon.

This result highlights the limitations of classical ARIMA models for
long-horizon price level forecasting on highly efficient markets.

## Model Comparison (Test Set)

| Model                     | RMSE (approx) | MAE (approx) |
|---------------------------|---------------|--------------|
| Naive Persistence         | ~212          | ~136         |
| Linear Regression (lags)  | ~210          | ~132         |
| ARIMA(1,1,1)              | ~5177         | ~5118        |

The naive persistence model established a strong baseline, reflecting
the near-random-walk nature of the NIFTY-50 index. Linear regression with
lagged features provided marginal improvement by exploiting short-term
linear dependencies. ARIMA underperformed due to recursive forecasting
error accumulation, reinforcing the difficulty of long-horizon price
level prediction in efficient markets.
