# Valuation Models 

This comprehensive equity valuation toolkit implements seven fundamental valuation methodologies (DCF, DDM, Graham Number, Buffett Fair Value, P/E & P/B multiples, Residual Income, and EV/EBITDA) in a unified Python framework. The project automates financial data collection via yfinance APIs and generates comparative analysis across multiple stocks simultaneously, outputting both CSV datasets and color-coded HTML reports. Designed for academic research and practical investment analysis, it demonstrates how different valuation approaches can complement each other while handling missing data gracefully and supporting configurable parameters for sensitivity analysis. This framework represents a synthesis of financial modeling theory and modern data science techniques, providing a robust foundation for equity research and portfolio-level analysis.
---

## Models at a glance (7 total)

| Model | Status | Data Source / Calculation | Formula |
|---|:--:|---|---|
| **DCF (Discounted Cash Flow)** |  Implemented | Free cash flow from cashflow statement; discount & terminal growth from `config.json` | See formula below |
| **DDM (Dividend Discount Model)** |  Implemented | Dividend history or `EPS × payout_ratio`; long‑run `g` from `config.json` or `growth_rates.csv` | Two‑stage Gordon |
| **Graham Number** |  Implemented | EPS & BVPS from fundamentals | √(22.5 × EPS × BVPS) |
| **Buffett Fair Value (approx.)** |  Implemented | EPS & growth (percent) from `growth_rates.csv` or info fallback | `EPS × (8.5 + 2g%)` |
| **Relative Valuation (P/E, P/B)** |  Data pulled only | `trailingPE`, `priceToBook` (or `price / bookValue`) from `yfinance` | Direct multiples |
| **Residual Income (ROE snapshot)** |  Data pulled only | ROE, BVPS, cost of equity ke = rf + β × MRP | AE = (ROE - ke) × BV *(snapshot)* |
| **EV/EBITDA Multiple** |  Data pulled only | EV from market cap + debt − cash; EBITDA from info | EV / EBITDA |


---

##  Outputs 

```
output/
├─ pe_pb.csv               # P/E & P/B snapshot
├─ residual_income.csv     # ROE, k_e, abnormal earnings snapshot
├─ ev_ebitda.csv           # EV/EBITDA multiple
├─ results.csv             # Core valuations: DCF / DDM / Graham / Buffett‑approx
├─ report.html             # Coloured table (Undervalued / Overvalued)
├─ combined_models.csv     # Final consolidation on Ticker

```

---

## Column glossary (quick meanings)

- **Ticker** — instrument code (e.g., `AAPL`, `2330.TW`, `GLD`)  
- **price** — latest price (recent close or real‑time fallback)  
- **trailingPE** — PE = Price / EPS_TTM  
- **priceToBook** — P/B = Price / BVPS  
- **beta (β)** — market sensitivity (≈1 market‑like; >1 more volatile; <1 defensive)  
- **ke** — cost of equity via CAPM: ke = rf + β × MRP  
- **net_income** — latest annual net income (GAAP/IFRS)  
- **book_equity (BV)** — shareholders' equity = assets − liabilities  
- **roe** — Net Income / Book Equity  
- **abnormal_earnings (AE)** — (ROE - ke) × BV *(snapshot)*  
- **ev_to_ebitda** — EV / EBITDA, with EV = Market Cap + Debt - Cash

---

## Formulae (reference)

### DCF (Discounted Cash Flow)
```
Firm Value = Σ[FCF₀(1+g)ᵗ / (1+r)ᵗ] + [FCFₙ(1+g) / (r-g)] × [1/(1+r)ᴺ]
where FCFₙ = FCF₀(1+g)ᴺ

Assumptions:
- Current free cash flow FCF_0 
- Annual growth rate g 
- Discount rate  r 
- Terminal growth rate g_t
- Projection horizon N


```
![DCF — Fair Value & Margin of Safety](output/dcf.PNG)


### DDM (Dividend Discount Model - two‑stage, simplified)
```
P₀ ≈ Σ[D₀(1+g)ⁱ / (1+ke)ⁱ] + [D_{n+1} / (ke-g)] × [1/(1+ke)ⁿ]

where:
D_n = D_0 * (1+g_1)^n
```
![DDM — Two‑stage Dividend Discount](output/ddm.PNG)

### Graham Number
```
√(22.5 × EPS × BVPS)

- **EPS**: earnings per share
- **BVPS**: book value per share
```
![Graham Number table](output/graham.PNG)


### Residual Income (full valuation form)
```
Intrinsic Value = BV₀ + Σ[RIₜ / (1+ke)ᵗ]
where RIₜ = (ROEₜ - ke) × BV_{t-1}
```

### EV/EBITDA
```
EV / EBITDA
where EV = Market Cap + Debt - Cash
```
![EV/EBITDA panel](output/EV_ebita.PNG)


### Buffett fair price


```
Fair Price = EPS * (8.5 + 2 * g)
g: growth rate (%)
```
![Buffett fair price panel](output/buffett_dcf.PNG)

### P/E & P/B relative
![P/E & P/B relative screen](output/pepb.PNG)


### ROE ranking & grading
![ROE ranking & grading](output/ROE.PNG)

### Consolidated conclusion
![Conclusion / model votes](output/conclusion.PNG)

---



Optional config (`config.json`):
```json
{
  "discount_rate": 0.09,
  "terminal_growth": 0.025,
  "ddm_defaults": {
    "payout_ratio": 0.35,
    "long_run_growth": 0.03
  }
}
```
---

## 📎 Contact

**Kai Yeh**  
Email: KaiYeh820206@gmail.com  
GitHub: https://github.com/WayneKaiYeh

---

## 📄 License

This repository is shared under the **Creative Commons BY‑NC‑ND 4.0** licence.  
-  No commercial use
-  No derivatives or redistribution
-  Attribution required

All rights to any private research notebooks and datasets are reserved.