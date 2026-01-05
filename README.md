# Mean-Reversion-Strategy-Simulator-with-Robust-Validation
Research-oriented mean-reversion backtesting framework for single-asset time series. Models deviations from an SMA equilibrium, applies ADF and Hurst diagnostics, and evaluates a threshold-based strategy with transaction costs and train/validation/test splits.

# Overview

This project implements a mean-reversion trading research framework for single-asset time series, with a focus on statistical validity, proper backtesting practice, and interpretability rather than raw performance.

The system allows users to:

- Test mean-reversion hypotheses using statistical diagnostics

- Simulate a simple threshold-based mean-reversion strategy

- Optimise strategy parameters using train / validation / test splits

- Incorporate transaction costs, trade activity constraints, and exposure penalties

- Interactively explore results via a GUI

# Core Idea

Mean reversion is defined as reversion of deviations from an equilibrium, not of price itself.

In this project:

- The equilibrium is defined by a Simple Moving Average (SMA)

- The deviation is defined as the spread:

       Spread = Price − SMA

- All statistical tests and trading signals are applied to this spread, ensuring internal consistency between theory, diagnostics, and trading logic.

# Methodology

1. Data Preparation

Daily price data is downloaded via yfinance

Indicators computed:

- SMA

- Log returns

- Spread (price deviation from SMA)

2. Mean Reversion Diagnostics

Before any optimisation, the strategy performs a statistical assessment of mean reversion:

- Augmented Dickey–Fuller (ADF) Test 

- Null hypothesis: unit root (no mean reversion)

- Hurst Exponent

Measures long-term memory of the spread

Interpretation:

H < 0.45 → mean-reverting

H ≈ 0.5 → random walk

H > 0.55 → trending

These diagnostics are informational, not performance-driven.

3. Trading Strategy

A simple threshold-based mean-reversion strategy is implemented:

- Enter long when the spread falls below a lower percentile

- Enter short when the spread rises above an upper percentile

- Positions are binary (long / short)

Transaction costs are applied per position change in basis points.

4. Performance Metrics

The following metrics are reported:

- Cumulative strategy returns (net of costs)

- Sharpe ratio

- Maximum drawdown

- Number of trades

- Average exposure

A composite score is used during optimisation:

      Score = Sharpe + 0.5 × MaxDrawdown − ActivityPenalty − TradePenalty


This discourages:

- Overfitting

- Trivial “no-trade” solutions

- Unrealistic low-exposure strategies

5. Optimisation Framework

When the optimiser is enabled, parameters are selected using a three-way split:

- Training set: estimate thresholds

- Validation set: select parameters

- Test set: report final performance

Parameters optimised:

- SMA window length

- Spread percentile threshold

Selection is based only on validation performance.
Test results are strictly out-of-sample.

# Graphical User Interface (GUI)

The Tkinter-based GUI allows users to:

- Select ticker and date range

- Adjust transaction costs (decimal basis points)

- Switch between single-parameter backtest and full optimiser with validation

- Interactively explore strategy behaviour

# Limitations

- Single-asset framework only

- Simple SMA-based equilibrium

- No volatility targeting or position sizing

- No intraday data

- No live execution or slippage modelling

# Future Extensions

Possible next steps:

- Pairs trading / cointegration-based spreads

- SMA half-life modelling

- Cross-asset testing

- Regime detection and filtering

- Volatility-normalised position sizing
