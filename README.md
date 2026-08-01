[Uploading AAPL_Moving_Average_Strategy_README.md…]()
# AAPL Moving Average Crossover Strategy — Backtest & Analysis

## Overview

A systematic trading strategy built in Python that generates buy/sell signals from a 50-day and 200-day simple moving average (SMA) crossover on Apple Inc. (AAPL), backtested against a buy-and-hold benchmark over an 11-year period (Jan 2015 – Dec 2025).

The project was built from scratch to learn the core mechanics of quantitative strategy design: signal generation, lookahead-bias-free backtesting, risk-adjusted performance measurement, and benchmark comparison.

## Methodology

- **Data:** Daily AAPL closing prices pulled via the `yfinance` API, Jan 2015 – Dec 2025 (\~2,766 trading days).  
- **Signal:** A 50-day SMA ("short") and 200-day SMA ("long") are calculated on closing price. The strategy is "long" (in the trade) whenever the 50-day average is above the 200-day average (a "golden cross" regime), and flat/out of the market otherwise.  
- **Avoiding lookahead bias:** The signal is shifted forward by one day before being applied to returns, since a crossover can only be confirmed after a day's close — the strategy cannot act on same-day information it wouldn't yet have.  
- **Returns:** Daily AAPL returns are multiplied by the (lagged) position to isolate the strategy's actual daily P\&L, then compounded into a cumulative return series and compared against a buy-and-hold benchmark over the same period.  
- **Risk metrics:** Annualized Sharpe ratio (mean daily return ÷ daily volatility × √252) and maximum drawdown (largest peak-to-trough decline in cumulative wealth) were calculated for both the strategy and the benchmark.

## Tools

Python, pandas, NumPy, matplotlib, yfinance, Google Colab.

## Results (Jan 2015 – Dec 2025\)

| Metric | Strategy (50/200 SMA Crossover) | Buy & Hold |
| :---- | :---- | :---- |
| Total return | \+326% | \+1,022% |
| Annualized Sharpe ratio | 0.68 | 0.91 |
| Maximum drawdown | \-45.6% | \-38.5% |

## Key Insight

The moving-average crossover strategy underperformed buy-and-hold on every metric tested — total return, Sharpe ratio, and maximum drawdown — over this period. This is a common, realistic outcome for trend-following strategies during a strong, sustained bull market: because moving averages are lagging indicators, the strategy is structurally late both entering new uptrends (missing the sharpest early gains) and exiting downturns (a fast, sharp sell-off can do most of its damage before a 200-day average has time to be crossed). The strategy's worse drawdown versus buy-and-hold, despite being designed to reduce risk, illustrates this lag concretely: it does not protect against fast market moves, only against slow, sustained trend reversals.

## How to Run

1. Open the notebook in Google Colab.  
2. Run all cells in order (data download → signal construction → returns → performance metrics → chart).  
3. Adjust the `start`/`end` dates or the SMA window lengths (currently 50/200) to test other periods or parameter combinations.

