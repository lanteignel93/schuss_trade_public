# Schuss — 1-DTE Options Strategy

> [!NOTE]
> **This is the public showcase repository.** To request access to the private, full-source repository, please email [laurent.lanteigne@gmail.com](mailto:laurent.lanteigne@gmail.com).

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A quantitative trading engine designed for **1-DTE (One Day to Expiration)** options strategies on major indices (SPX, NDX). This system handles signal generation, risk management (Gamma/Theta exposure), and backtesting for short-duration volatility trades.

## Architecture

The project follows a production-grade `src` layout:

```text
schuss_trade_public/
├── .git/
├── .github/
│   └── workflows/
├── notebooks/                   # Jupyter notebooks
├── src/
│   └── one_dte_trade/           # <--- Main Package Directory
│       ├── cached_data/
│       ├── configs/             # Configuration files folder
│       ├── utils/               # Utility scripts folder
│       ├── __init__.py
│       ├── analysis.py
│       ├── backtester.py
│       ├── config.py
│       ├── data.py
│       ├── datagen.py
│       ├── features.py
│       ├── straddle_data.py
│       ├── strategy_signal.py
│       └── vol_indicies.py
├── tests/
│   └── test_smoke.py
├── .gitignore
├── .pre-commit-config.yaml
├── pyproject.toml
├── README.md
└── uv.lock
```


## Getting Started
We use `uv` for fast, reliable dependency management.


```bash
# 1. Sync Environment
uv sync

# 2. Activate
source .venv/bin/activate

# 3. Install Pre-commit Hooks
pre-commit install
```


## Strategy Logic

* Instrument: Index Options (SPX, SPY, QQQ).
* Timeframe: Intraday to Overnight (1 Day duration).
* Core Concept: Exploiting the accelerated theta decay and mean-reverting volatility premium at the very end of the curve.
* Risk Management: Strict delta limits, Fractional Kelly-Criterion, Max Loss as a multiple of premium left.
