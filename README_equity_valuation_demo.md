# 📈 Equity Valuation Demo

This notebook demonstrates multiple equity valuation models in Python using `yfinance`.  
It supports automatic data fetching, model calculation, and export of results in CSV and HTML (with colour-coded valuation notes).

---

## 🧮 Implemented & Supported Models

| Model | Status | Data Source / Calculation | Formula |
|---|---|---|---|
| **DCF (Discounted Cash Flow)** | ✅ Implemented | Free cash flow from cashflow statement | See formula below |
| **DDM (Dividend Discount Model)** | ✅ Implemented | Dividend history or EPS × payout ratio | Two-stage growth |
| **Graham Number** | ✅ Implemented | EPS & Book Value per Share | \( \sqrt{22.5 \times EPS \times BVPS} \) |
| **Buffett Fair Value Formula** | ✅ Implemented | EPS & growth rate | \( EPS \times (8.5 + 2 \times g) \) |
| **Relative Valuation (P/E, P/B)** | ⚠️ Data pulled only | `trailingPE` & `priceToBook` from `yfinance` | Direct comparison |
| **Residual Income Model (ROE)** | ⚠️ Data pulled only | `returnOnEquity`, BVPS, cost of equity | \( BV + \sum \frac{RI_t}{(1+r)^t} \) |
| **EV/EBITDA Multiple** | ⚠️ Data pulled only | EV & EBITDA | \( \frac{EV}{EBITDA} \) |

✅ = fully calculated in code • ⚠️ = data retrieved; extend as needed

---

## ⚙️ Workflow
1. **Load configuration and tickers** – `config.json`, `tickers.txt`  
2. **Fetch financial data** – cash flow, EPS, BVPS, dividends, P/E, P/B, ROE, EV/EBITDA  
3. **Run valuation models** – DCF, DDM, Graham, Buffett  
4. **Aggregate** – median fair value, Upside %, valuation flag  
5. **Export** – `output/results.csv`, `output/report.html`

---

## 📐 Models & Formulae

### 1. Discounted Cash Flow (DCF)

Assumptions:
- Current free cash flow \( FCF_0 \)
- Annual growth rate \( g \)
- Discount rate \( r \)
- Terminal growth rate \( g_t \)
- Projection horizon \( N \)

# Corporate Valuation Formula

## Firm Value Calculation Formula

$$
\text{Firm Value} = \sum_{t=1}^{N} \frac{FCF_0 \cdot (1+g)^t}{(1+r)^t} + \frac{FCF_N \cdot (1+g_t)}{(r - g_t)} \cdot \frac{1}{(1+r)^N}
$$

where: `FCF_N = FCF_0 × (1+g)^N`

## Fair Value per Share

$$
\text{Fair Value per Share} = \frac{\text{Firm Value} + \text{Cash} - \text{Debt}}{\text{Shares Outstanding}}
$$

## Variables

- \( FCF_0 \): base free cash flow
- \( g \): growth rate
- \( r \): discount rate (WACC)
- \( N \): projection years
- \( g_t \): terminal growth rate

---

### 2. Dividend Discount Model (Two-Stage DDM)

Assumptions:
- Current dividend \( D_0 \)
- First-stage growth \( g_1 \) (for \( n \) years)
- Terminal (second-stage) growth \( g_2 \)
- Discount rate \( r \)

Formula:

$$\text{Value} = \sum_{t=1}^{n} \frac{D_0 \times (1+g_1)^t}{(1+r)^t} + \frac{D_n \times (1+g_2)}{(r - g_2)} \times \frac{1}{(1+r)^n}$$

where:

$$D_n = D_0 \times (1+g_1)^n$$

---

### 3. Graham Number

Benjamin Graham’s conservative fair-value formula:

$$
\text{Graham Number} = \sqrt{22.5 \cdot EPS \cdot BVPS}
$$

- **EPS**: earnings per share
- **BVPS**: book value per share

---

### 4. Buffett Approximation

Buffett Fair Value Formula:

$$
\text{Fair Price} = EPS \times (8.5 + 2 \times g)
$$

- \( g \): growth rate (%)

---

> Notes (implementation details in this notebook):
> - DCF requires your own cash flow forecasting; here we approximate using the latest FCF and a growth assumption.
> - DDM takes dividend history when available; otherwise uses EPS × payout ratio as a proxy for \( D_0 \).
> - Buffett method compares your fair price to current market price.
> - Relative valuation (P/E, P/B), ROE-based residual income, and EV/EBITDA are retrieved for comparison; extend formulas as needed.

---

## 📦 Files
- **Notebook (enhanced)**: `equity_valuation_demo_enhanced.ipynb`
- **Original**: `股票估值方法.ipynb`
- Generated on: 2025-08-10 17:45 UTC