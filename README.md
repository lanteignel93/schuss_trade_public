# Schuss — One-DTE Volatility Strategy

> [!NOTE]
> **This is the public showcase repository.** To request access to the private, full-source repository, please email [laurent.lanteigne@gmail.com](mailto:laurent.lanteigne@gmail.com).

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A daily signed volatility trade on SPXW one-day-to-expiration options: a reversal signal
on the 1-DTE straddle's own recent short-side returns sizes the book long or short at
each close — long days buy an iron butterfly, short days sell a 15Δ strangle — and
positions ride one session to PM settlement. The production position rules and the live
loss rule (buy the short package back at 3× the credit collected, monitored intraday)
are all measured here.

## The card (2023-01 → 2026-08, all trading days, production rules on)

| | desk config (ironfly / strangle) | reference (straddle) |
|---|---|---|
| Sharpe (annualized) | **2.27** | 2.22 |
| rules off (same inputs) | 2.05 | 2.12 |
| hit rate | 51% | 54% |

- **Live reconciliation**: the strategy has traded live for about a year at a realized
  Sharpe of ~2.3–2.4 — on top of the backtest.
- **The live 3× loss rule** (5-minute intraday first-passage): at the structure level it
  transforms the tail (worst short day −197 → −118 points; Sharpe 1.65 → 2.97 on short
  days). At book sizing it is insurance in calm years and survival in the 2025-26
  regime (recent-regime Sharpe 1.64 → 2.31, worst day −45K → −18K), with the honest
  caveat that a gap through the trigger fills at the market, not at 3×.
- **Luck check**: bootstrap of the realized daily P&L puts the total at +574K inside a
  [+382K, +763K] 5th–95th band with 0% losing paths; a zero-edge null (demeaned
  resamples) never reaches the realized total in 10,000 draws — the P&L is not a draw
  from luck.

## What's here

- `strategy_results.ipynb` — the executed results notebook: the strategy in one table,
  performance with rules on/off, the intraday loss-rule study, and the bootstrap luck
  check. Every figure reproduces from the small derived series in `data/`.
- `data/backtest_daily.parquet` — the daily backtest panel (both configurations,
  rules on/off arms).
- `data/stop3x_daily.parquet` — the intraday loss-rule study panel.

## What stays private

The signal construction and parameter provenance, the management-rule research
(per-leg vs package stops, profit-takes, entry filters — pre-registered studies with a
spent holdout), execution and cost analysis, and the production signal infrastructure.

*Enough to evaluate the strategy; not enough to replicate it.*
