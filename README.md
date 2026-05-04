# Portfolio Optimization

Markowitz mean-variance portfolio optimization applied to 4 major indices, with efficient frontier and Capital Market Line visualization.

---

## Objective

Build and analyze the **tangent portfolio** from a universe of 4 market indices, maximize the Sharpe ratio, and visualize the efficient frontier versus the Capital Market Line.

---

## Assets

| Ticker | Index |
|--------|-------|
| `^RUT` | Russell 2000 |
| `^IXIC` | NASDAQ Composite |
| `^GSPC` | S&P 500 |
| `XWD.TO` | iShares MSCI World ETF |

**Period:** 2023-01-01 → 2024-01-01 · **Risk-free rate:** 1%

---

## Methodology

1. **Data extraction** — daily closing prices via `yfinance`
2. **Returns** — annualized expected returns and covariance matrix (×252)
3. **Tangent portfolio** — closed-form allocation maximizing Sharpe ratio
4. **Efficient frontier** — full mean-variance frontier plotted against the CML

---

## Key Results

| Asset / Portfolio | Ann. Return | Ann. Std Dev | Sharpe Ratio |
|-------------------|-------------|--------------|--------------|
| ^RUT | 16.8% | 20.1% | 0.79 |
| ^IXIC | 38.8% | 17.4% | 2.17 |
| ^GSPC | 23.2% | 13.1% | 1.70 |
| XWD.TO | 18.8% | 9.9% | 1.80 |
| **Tangent Portfolio** | **66.6%** | **26.1%** | **2.51** |

**Tangent portfolio weights:** `^RUT` −40%, `^IXIC` +286%, `^GSPC` −230%, `XWD.TO` +85%

---

## Stack

- Python · `yfinance` · `numpy` · `pandas` · `matplotlib`

---

## Run

```bash
pip install yfinance pandas numpy matplotlib
jupyter notebook optimisation_portfolio.ipynb
```
