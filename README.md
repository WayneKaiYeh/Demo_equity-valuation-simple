Equity Valuation Demo
This project demonstrates how to fetch financial data with Python and yfinance, and compute several common equity valuation models.
Included models:

DCF (Discounted Cash Flow)

DDM (Dividend Discount Model)

Graham Number

Buffett Approximation (simplified fair-price formula)

It automatically outputs:

results.csv: Result

report.html: Colour-coded valuation report (green = undervalued, red = overvalu
---

## Structure
```
equity-valuation-demo/
├─ config.json # default parameters
├─ tickers.txt # stocks
├─ valuation.ipynb # 
├─ output/
│ ├─ results.csv # result
│ └─ report.html # HTML report
└─ README.md 
```



---

Workflow
Load configuration and tickers

config.json: discount rate, terminal growth rate, default payout ratio for DDM, etc.

tickers.txt: one ticker per line (e.g., AAPL, MSFT, TSM)

Fetch financial data

Use yfinance to obtain:

Cash Flow (CFO, CapEx)

Price, EPS, Book Value

Dividend history

Run valuation models

DCF: project from the latest free cash flow for N years and discount to present; add terminal value

DDM: two-stage dividend growth model

Graham Number: based on EPS and book value per share

Buffett Approximation: based on EPS and estimated growth rate

Aggregate results

Compute the median of the four model values as the Baseline Fair Value

Calculate Upside %

Flag Undervalued / Overvalued

Export

results.csv: raw numbers

report.html: colour-highlighted report with notes


---

## 📐 Models & Formulae

### 1. Discounted Cash Flow（DCF）
Assumptions:
- Current free cash flow \( FCF_0 \)
- Annual growth rate \( g \)
- Discount rate \( r \)
- Terminal growth rate \( g_t \)
- Projection horizon \( N \)


#  Corporate Valuation Formula

## Firm Value Calculation Formula

$$
\text{Firm Value} = \sum_{t=1}^{N} \frac{FCF_0 \cdot (1+g)^t}{(1+r)^t} + \frac{FCF_N \cdot (1+g_t)}{(r - g_t)} \cdot \frac{1}{(1+r)^N}
$$

其中：`FCF_N = FCF_0 × (1+g)^N`

## fair value per share

$$
\text{Fair Value per Share} = \frac{\text{Firm Value} + \text{Cash} - \text{Debt}}{\text{Shares Outstanding}}
$$

## varibles

- $FCF_0$：base free cash flow
- $g$：growth rate
- $r$：discount rate (WACC)
- $N$：projection years
- $g_t$： terminal growth rate

---

### 2. Dividend Discount Model (Two-Stage DDM)
assumptions：
- Current dividend $D_0$
- First-stage growth $g_1$（for $n$ years）
- Terminal (second-stage) growth $g_2$
- Discount rate $r$

Formula：

$$\text{Value} = \sum_{t=1}^{n} \frac{D_0 \times (1+g_1)^t}{(1+r)^t} + \frac{D_n \times (1+g_2)}{(r - g_2)} \times \frac{1}{(1+r)^n}$$

and：

$$D_n = D_0 \times (1+g_1)^n$$

---

### 3. Graham Number
Benjamin Graham’s conservative fair-value formula:

$$
\text{Graham Number} = \sqrt{22.5 \cdot EPS \cdot BVPS}
$$
- EPS： earnings per share
- BVPS：book value per share(每股淨值)

---

### 4. Buffett Approximation
Simplified Buffett fair-price formula:

$$
\text{Fair Price} = EPS \times (8.5 + 2 \times g)
$$
- $g$：growth rate (%)
---