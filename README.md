# 📈 Equity Valuation Demo

本專案展示如何以 Python 與 `yfinance` 抓取財務數據，並計算多種常見的股票估值模型。  
模型包含：
- **DCF**（Discounted Cash Flow，折現現金流）
- **DDM**（Dividend Discount Model，股利折現模型）
- **Graham Number**（葛拉漢合理價）
- **Buffett Approximation**（巴菲特簡化版合理價公式）

並自動輸出：
- `results.csv`：數值結果
- `report.html`：帶顏色標註的估值報表（綠色 = 被低估，紅色 = 被高估）

---

## 📂 Structure
```
equity-valuation-demo/
├─ config.json # 模型假設參數
├─ tickers.txt # 要分析的股票代碼
├─ valuation.ipynb # 主程式（Notebook 版本）
├─ output/
│ ├─ results.csv # 計算結果
│ └─ report.html # 帶顏色的 HTML 報告
└─ README.md # 專案說明
```



---

## ⚙️ 流程

1. **讀取設定檔與股票清單**
   - `config.json`：包含折現率、永續成長率、DDM 預設派息比率等
   - `tickers.txt`：每行一個股票代碼（例如 AAPL、MSFT、TSM）

2. **抓取財務數據**
   - 使用 `yfinance` 取得：
     - 現金流量表（CFO、CapEx）
     - 股價、EPS、Book Value
     - 股利記錄

3. **計算各模型估值**
   - **DCF**：以最近一期自由現金流為基礎，投影未來 N 年並折現，加上終值
   - **DDM**：兩階段股利成長模型
   - **Graham Number**：基於 EPS 與每股淨值
   - **Buffett Approximation**：基於 EPS 與預估成長率

4. **整合結果**
   - 計算四種模型的中位數作為「基準合理價」
   - 計算上漲空間（Upside %）
   - 標記 **Undervalued** / **Overvalued**

5. **輸出**
   - `results.csv`：純數據
   - `report.html`：Note 欄位綠/紅底色報表

---

## 📐 模型與數學公式

### 1. 折現現金流（DCF）
假設：
- 當前自由現金流 \( FCF_0 \)
- 年成長率 \( g \)
- 折現率 \( r \)
- 永續成長率 \( g_t \)
- 預估年數 \( N \)


公式：
$$
\text{Firm Value} =
\sum_{t=1}^{N} \frac{FCF_0 \cdot (1+g)^t}{(1+r)^t}
+
\frac{FCF_N \cdot (1+g_t)}{(r - g_t)} \cdot \frac{1}{(1+r)^N}
$$

其中：
$$
FCF_N = FCF_0 \cdot (1+g)^N
$$

每股合理價：
$$
\text{Fair Value per Share} = \frac{\text{Firm Value} + \text{Cash} - \text{Debt}}{\text{Shares Outstanding}}
$$









---

### 2. 股利折現模型（兩階段 DDM）
假設：
- 當前股利 \( D_0 \)
- 第一階段年成長率 \( g_1 \)（持續 \( n \) 年）
- 第二階段永續成長率 \( g_2 \)
- 折現率 \( r \)

公式：
\[
\text{Value} =
\sum_{t=1}^{n} \frac{D_0 \cdot (1+g_1)^t}{(1+r)^t}
+
\frac{D_n \cdot (1+g_2)}{(r - g_2)} \cdot \frac{1}{(1+r)^n}
\]

其中：
\[
D_n = D_0 \cdot (1+g_1)^n
\]

---

### 3. Graham Number
Benjamin Graham 提出的保守合理價公式：
\[
\text{Graham Number} = \sqrt{22.5 \cdot EPS \cdot BVPS}
\]
- EPS：每股盈餘
- BVPS：每股淨值

---

### 4. Buffett Approximation
簡化版的巴菲特合理價公式（教學用）：
\[
\text{Fair Price} = EPS \times (8.5 + 2 \times g)
\]
- \( g \)：成長率（%）

---