
# 📈 Valuation Demo 


---

## 🧭 Models at a Glance (7 total)

| Model | Status | Data Source / Calculation | Formula |
|---|:--:|---|---|
| **DCF (Discounted Cash Flow)** | ✅ Implemented | Free cash flow from cashflow statement; CAPM/`config.json` discount; terminal growth | See below (toy DCF) |
| **DDM (Dividend Discount Model)** | ✅ Implemented | Dividend history or `EPS × payout_ratio`; long‑run `g` from `config.json` or `growth_rates.csv` | Two‑stage Gordon |
| **Graham Number** | ✅ Implemented | EPS & BVPS from fundamentals | \\( \sqrt{22.5 \times EPS \times BVPS} \\) |
| **Buffett Fair Value (approx.)** | ✅ Implemented | EPS & growth (percent) from `growth_rates.csv` or info fallback | \\( EPS \times (8.5 + 2g\%) \\) |
| **Relative Valuation (P/E, P/B)** | ⚠️ Data pulled only | `trailingPE`, `priceToBook` (or `price/bvps`) from `yfinance` | Direct multiples |
| **Residual Income (ROE snapshot)** | ⚠️ Data pulled only | ROE, BVPS, cost of equity \\( k_e=r_f+\beta \cdot MRP \\) | \\( AE=(ROE-k_e)\times BV \\) *(snapshot)* |
| **EV/EBITDA Multiple** | ⚠️ Data pulled only | EV from market cap + debt − cash; EBITDA from info | \\( \tfrac{EV}{EBITDA} \\) |

> **Why “Data pulled only”?** For these three, the notebook outputs the raw/derived ratios for comparison. Full multi‑period RI valuation is shown by formula but not expanded in this demo (kept intentionally simple).

---

## 📁 Outputs (where each model writes)

- `output/results.csv` — **core valuations** (first 4 models):  
  Columns: `Ticker, Price, DCF_ps, DDM, Graham, BuffettApprox, ...`  
  Also writes a coloured **`output/report.html`** for quick sharing.

- `output/pe_pb.csv` — **Relative Valuation** (P/E, P/B).  
  Columns: `Ticker, price, trailingPE, priceToBook`.

- `output/residual_income.csv` — **RI snapshot**.  
  Columns: `Ticker, beta, ke, net_income, book_equity, roe, abnormal_earnings`.

- `output/ev_ebitda.csv` — **EV/EBITDA multiple**.  
  Columns: `Ticker, ev_to_ebitda`.

- `output/combined_models.csv` — **final consolidated** table merged on `Ticker`.

---

## 🔎 Column Glossary (quick meanings)

- **Ticker** — instrument code (e.g., `AAPL`, `2330.TW`, `GLD`)  
- **price** — latest price (recent close or a real‑time fallback)  
- **trailingPE** — \\( \mathrm{PE}=\tfrac{Price}{EPS_{TTM}} \\)  
- **priceToBook** — \\( \mathrm{P/B}=\tfrac{Price}{BVPS} \\) (falls back to `price/bookValue` if needed)  
- **beta (β)** — market sensitivity (≈1 market‑like; >1 more volatile; <1 defensive)  
- **ke** — cost of equity via CAPM: \\( k_e=r_f+\beta\cdot MRP \\)  
- **net_income** — latest annual net income (GAAP/IFRS)  
- **book_equity (BV)** — shareholders’ equity = assets − liabilities  
- **roe** — \\( \tfrac{Net\ Income}{Book\ Equity} \\)  
- **abnormal_earnings (AE)** — \\( (\mathrm{ROE}-k_e)\times BV \\) *(single‑period snapshot)*  
- **ev_to_ebitda** — \\( \tfrac{EV}{EBITDA} \\), with \\( EV=Market\ Cap+Debt-Cash \\)

---

## 🧮 Formulae (reference)

**Toy DCF (illustrative)**  
\\[
\text{Firm Value}=\sum_{t=1}^{N}\frac{FCF_0(1+g)^t}{(1+r)^t}+\frac{FCF_N(1+g)}{(r-g)}\cdot\frac{1}{(1+r)^N},\qquad FCF_N=FCF_0(1+g)^N
\\]

**DDM (two‑stage, simplified)**  
\\[
P_0\approx \sum_{i=1}^{n}\frac{D_0(1+g)^i}{(1+k_e)^i}+\frac{D_{n+1}}{k_e-g}\cdot\frac{1}{(1+k_e)^n}
\\]

**Graham Number**  
\\[
\sqrt{22.5\times EPS\times BVPS}
\\]

**Buffett Approx (demo only)**  
\\[
EPS\times (8.5+2g\%)
\\]

**Residual Income (full valuation form, for completeness)**  
\\[
\text{Intrinsic Value}\;=\;BV_0\;+\;\sum_{t=1}^{N}\frac{RI_t}{(1+k_e)^t},\qquad RI_t=(ROE_t-k_e)\times BV_{t-1}
\\]

**EV/EBITDA**  
\\[
\frac{EV}{EBITDA},\qquad EV=Market\ Cap+Debt-Cash
\\]

---

## 🖼️ Screenshot Tips

- Show first rows of: `pe_pb.csv`, `residual_income.csv`, `ev_ebitda.csv`  
- Include `report.html` (colour‑coded **Undervalued/Overvalued**)  
- Use `combined_models.csv` as the **overview** shot in your README

---

## ⚙️ Configuration

Typical `config.json`:
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
> The notebook also accepts `dcf_discount_rate` to mean `discount_rate`.

Optional `output/growth_rates.csv`:
```
Ticker, recommended_g_buffett_percent, recommended_g_ddm_decimal
AAPL,   8.0,                           0.03
MSFT,   7.0,                           0.025
...
```
- `recommended_g_buffett_percent` — percentage (e.g., `8.0` → 8%)  
- `recommended_g_ddm_decimal` — decimal (e.g., `0.03` → 3%)

---

## 🧰 Troubleshooting

- **“Missing file: results.csv”** — You didn’t run the core valuation cell. The three ratio CSVs still work.
- **EmptyDataError (no columns)** — A CSV exists but is empty. Ensure you always write headers with `to_csv(..., index=False)`; the notebook falls back to header‑only files when data is missing.
- **Ticker column mismatch** — The consolidator normalises `ticker/symbol` → `Ticker`. Standardise if you merge elsewhere.
- **Many NaNs** — Common for ETFs/financials/non‑US tickers (missing BV/EBITDA/FCF). For polished demos, start with large‑cap US equities.
- **`yfinance` throttling** — Retry or reduce ticker count; the notebook includes basic retries.

---

## 📝 Licence & Data Source

- Code: MIT Licence (adjust as needed)  
- Data: Yahoo Finance via `yfinance` — **educational/demo only; not real‑time; no guarantees**
