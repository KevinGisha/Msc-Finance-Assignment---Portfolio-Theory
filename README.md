# 📈 Multi-Asset Portfolio Analysis & Optimization

A quantitative portfolio analysis project built in Python. It constructs a diversified **12-asset portfolio** (equities, bonds, forex, commodities), studies the statistical behaviour of returns, optimizes allocations under constraints, evaluates performance across market regimes, benchmarks against the S&P 500, and extends the optimizer with **ESG preferences**.

**Author:** Sahaya Princelin Gisha Kevin Micheal
**Notebook:** `SAHAYA_PRINCELIN_GISHA_KEVIN_MICHEAL_Portfolio.ipynb`

---

## 🗂️ Portfolio Universe

| Asset Class | Asset | Ticker |
|---|---|---|
| Equity | Microsoft | `MSFT` |
| Equity | Tesla | `TSLA` |
| Equity | Apple | `AAPL` |
| Equity | Nestlé | `NESN.SW` |
| Equity | JPMorgan Chase | `JPM` |
| Bond ETF | iShares 10-20 Year Treasury | `TLH` |
| Bond ETF | iShares 10+ Year Investment Grade Corporate | `ILTB` |
| Bond ETF | iShares iBoxx $ Investment Grade Corporate | `LQD` |
| Forex | USD/CHF | `USDCHF=X` |
| Forex | GBP/USD | `GBPUSD=X` |
| Commodity | Gold futures | `GC=F` |
| Commodity | Silver futures | `SI=F` |

- **Data source:** Yahoo Finance via `yfinance`
- **Period:** January 2005 – April 2025 (covers the 2008 GFC, COVID-19, and the recent rate-hike cycle)
- **Frequency:** Daily

---

## 🔍 What the Project Covers

### 1. Data & Return Statistics
- Downloads prices and computes daily returns
- Summary statistics, **annualized mean / median / standard deviation**
- **Skewness and kurtosis** for every asset
- Return distribution histograms with KDE
- **Shapiro-Wilk normality tests** (normality rejected for all assets, so returns have fat tails)
- Cumulative return plots and a **correlation heatmap**

### 2. Constrained Portfolio Optimization (SciPy `SLSQP`)
Both portfolios are subject to:

| Constraint | Limit |
|---|---|
| Fully invested | Weights sum to 100% |
| Long-only | 0% ≤ weight ≤ 100% |
| Annualized volatility | ≤ 15% |
| Expected annual return | ≥ 10% |
| Commodities (Gold + Silver) | ≤ 20% |
| Forex (USDCHF + GBPUSD) | ≤ 30% |

**Resulting allocations:**

| Asset | Min-Variance | Max-Sharpe |
|---|---:|---:|
| Microsoft | 5.64% | 13.00% |
| Tesla | 4.45% | 12.98% |
| Apple | 6.40% | 16.29% |
| Nestlé | 8.17% | 6.87% |
| JPMorgan | 7.46% | 9.53% |
| TLH | 29.03% | 24.63% |
| ILTB | 0.00% | 0.00% |
| LQD | 0.00% | 0.00% |
| USD/CHF | 12.38% | 0.00% |
| GBP/USD | 15.51% | 0.00% |
| Gold | 10.97% | 16.68% |
| Silver | 0.00% | 0.00% |

### 3. Regime Analysis & Value-at-Risk
Three market regimes were defined and both portfolios were evaluated in each, with **parametric VaR at 90%, 95% and 99%**:

| Regime | Period | Min-Var Return / Vol | Max-Sharpe Return / Vol |
|---|---|---|---|
| Crisis | Mar 2020 – Dec 2020 | 29.9% / 11.6% | 83.6% / 25.5% |
| Recovery / Normal | 2021 – 2022 | -4.1% / 8.5% | -2.9% / 16.7% |
| Inflation / Rate-hike | 2023 – 2024 | 12.2% / 7.0% | 29.2% / 12.9% |

The Max-Sharpe portfolio earns more but carries substantially higher tail risk. The Min-Variance portfolio is more defensive in every regime.

### 4. Market Benchmarking (CAPM-style Regression)
OLS regression of the Max-Sharpe portfolio's daily returns against **SPY**:

- **Beta:** 0.62
- **Adjusted R²:** 57.2%
- **Portfolio annual return:** 20.08% vs **SPY:** 15.84%
- **Annual excess premium:** ≈ 4.23%

### 5. ESG-Aware Optimization
The objective is extended to `maximize: μᵀw − ½γ·wᵀΣw + λ·sᵀw`, where `s` is a vector of ESG scores (0–1), `γ = 3` is risk aversion and `λ = 0.5` is ESG preference. Extra constraints:

- Minimum portfolio ESG score of **0.80**
- Cap of 20% on the lowest-ESG assets (FX)
- Volatility cap of 20%

The ESG-aware optimizer shifts toward high-ESG holdings: **Microsoft 35.4%, Apple 22.3%, Nestlé 20.7%, Tesla 15.8%, TLH 5.8%**, with zero allocation to JPMorgan, FX and commodities.

> ⚠️ ESG scores used here are illustrative assumptions, not data from an ESG rating provider.

---

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `SciPy` · `statsmodels` · `yfinance` · `matplotlib` · `seaborn` · `tabulate`

---

## 🚀 Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Install dependencies
pip install yfinance pandas numpy scipy statsmodels matplotlib seaborn tabulate jupyter

# 3. Launch the notebook
jupyter notebook SAHAYA_PRINCELIN_GISHA_KEVIN_MICHEAL_Portfolio.ipynb
```

You can also open the notebook directly in **Google Colab**. An internet connection is required to download price data from Yahoo Finance.

---

## 📁 Repository Structure

```
├── SAHAYA_PRINCELIN_GISHA_KEVIN_MICHEAL_Portfolio.ipynb   # Full analysis
└── README.md
```

---

## ⚠️ Disclaimer

This project was created for **educational purposes** as part of a university assignment. It is not financial advice. Results depend on historical data and modelling assumptions, and past performance does not guarantee future results.
