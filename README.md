# CDStressor

A Python-based Credit Default Swap (CDS) pricing and analysis tool, built to develop practical understanding of credit risk modelling. Developed as a portfolio project alongside study for the Yale Financial Markets course and Bloomberg Market Concepts certification.

## Overview

CDStressor implements a reduced-form CDS pricing model using hazard rates and survival probabilities. It prices the premium and protection legs of a CDS contract using continuous-time discounting, derives the breakeven hazard rate, and visualises how contract value changes as credit conditions shift.

The project also integrates real market data, including historical CDX index spreads and VIX levels, to contextualise model outputs against actual credit market conditions.

A stress testing module is currently in development, which will allow parallel credit spread shift scenarios, recovery rate stress, and historical scenario overlays (GFC 2008, EU sovereign debt crisis 2011, COVID March 2020).

## Project Structure

```
CDStressor/
├── CDStressor.ipynb        # Main notebook: CDS pricer and market data integration
├── practice.ipynb          # Supplementary notebook: portfolio analytics and risk concepts
├── CDX_index_hist.csv      # Historical CDX investment grade index data
├── requirements.txt        # Python dependencies
└── README.md
```

## Model

The pricing model follows the standard reduced-form (intensity-based) approach:

- **Hazard rate (λ):** implied from the market spread via λ ≈ spread / (1 − recovery rate)
- **Survival probability:** S(t) = e^(−λt)
- **Premium leg:** present value of spread payments, weighted by survival probability at each payment date
- **Protection leg:** present value of loss given default, weighted by the probability of defaulting in each period
- **NPV:** protection leg − premium leg

The breakeven hazard rate is the value of λ at which NPV = 0, found by linear interpolation.

## Features

- Interactive CDS pricer with adjustable notional, spread, maturity, recovery rate, and discount rate
- Valuation curve showing premium leg, protection leg, and NPV across a range of hazard rates
- Breakeven hazard rate annotation
- Profit/loss shading relative to entry NPV
- Historical CDX spread integration: implied hazard rates and NPV time series derived from real index data
- VIX index visualisation for broader market context

## Requirements

Python 3.x with the following packages:

```
numpy
pandas
plotly
ipywidgets
matplotlib
yfinance
```

Install all dependencies with:

```bash
pip install -r requirements.txt
```

## Data Sources

**CDX_index_hist.csv:** Historical closing prices for the CDX North America Investment Grade index. Source: Nasdaq Data Link. The implied hazard rate for each observation is derived using the recovery rate assumption of 40%.

**VIX:** Pulled from the [datasets/finance-vix](https://github.com/datasets/finance-vix) public repository via pandas.

**LQD:** iShares iBoxx Investment Grade Corporate Bond ETF price data, pulled via yfinance as a proxy for investment grade credit market conditions.

## Status

- [x] CDS pricing model (hazard rate / survival probability)
- [x] Interactive valuation tool
- [x] Historical market data integration (CDX, VIX, LQD)
- [ ] Parallel credit spread shift stress scenarios (CS01, P&L sensitivity)
- [ ] Recovery rate stress (LGD impact on fair value and expected loss)
- [ ] Historical scenario overlays (GFC 2008, EU sovereign debt crisis 2011, COVID 2020)
- [ ] Portfolio-level stress with concentration risk (stretch goal)

## Background

Built as a self-directed learning project to develop practical credit risk modelling skills alongside formal study. The stress testing extension is modelled on the kind of scenario analysis used in credit risk and market risk functions, where sensitivity to spread widening and recovery rate assumptions forms a core part of position monitoring and regulatory reporting.
