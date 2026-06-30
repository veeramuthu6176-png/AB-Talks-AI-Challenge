---
name: stock-fundamental-research
description: >
  Analyze Indian and global listed companies using fundamentals, financial statements, business quality,
  competitive advantages, valuation, risks, and growth prospects. Generate evidence-based research
  reports and investor-friendly summaries. Use this skill whenever the user asks about a stock,
  share, company analysis, fundamentals, valuation, P/E ratio, market cap, ROE, ROCE, promoter
  holding, FII/DII data, earnings, revenue growth, debt, dividends, moat, sector comparison,
  peer comparison, or requests a research report on any listed company (NSE/BSE or global).
  Trigger for phrases like "analyze X stock", "fundamentals of Y", "is Z overvalued", "compare
  A vs B stocks", "pros and cons of investing in X", "research report on X", "deep dive on X",
  "quick take on X", or any question implying stock research, even casually phrased. Never provide
  buy/sell/hold recommendations or target prices.
---

# Stock Fundamental Research Skill

Analyze listed stocks using fundamentals only. Provide evidence-based views. **Never give buy/sell/hold calls, target prices, or personalized investment advice.**

---

## Step 0 — Detect Mode

| Trigger | Mode |
|---|---|
| Only stock name / casual question | **Quick Take** |
| "full analysis" / "deep dive" / "detailed" | **Deep Dive** |
| Two stocks / "vs" / "compare" | **Compare** |
| "pros and cons" / "strengths and risks" | **Pros & Cons** |
| User shares portfolio + asks fit | **Portfolio Fit** |

---

## Step 1 — Fetch Live Data

**Source priority (try in order, cross-check key figures with ≥2 sources):**
1. Screener.in — `https://www.screener.in/company/<TICKER>/`
2. Tickertape — `https://www.tickertape.in/stocks/<name>`
3. Moneycontrol — search for the company
4. NSE India — `https://www.nseindia.com/`
5. BSE India — `https://www.bseindia.com/`
6. Annual reports & investor presentations
7. Earnings call transcripts

**If live retrieval fails:** State clearly — *"Live data couldn't be fetched; figures may be outdated."*  
**If a specific figure is unavailable:** Flag it — *"🚩 Data unavailable — verify at [source]."*  
**Always cite the source next to every key figure.**

---

## Step 2 — Research Checklist

Collect and verify these data points before writing any output:

**Price & Size**
- CMP (Current Market Price), Market Cap, Face Value, 52W High/Low

**Valuation**
- P/E, P/B, EV/EBITDA — compare vs sector median AND 5Y own average

**Growth (3Y & 5Y CAGRs)**
- Revenue CAGR, Profit CAGR, EPS CAGR
- EPS for last 8 quarters (trend: accelerating / steady / slowing / declining)

**Profitability**
- EBITDA Margin (5Y trend), Net Profit Margin (5Y trend)

**Cash Flow**
- Free Cash Flow (FCF) for 3–5 years

**Financial Health**
- Debt-to-Equity (D/E), Interest Coverage Ratio, Current Ratio

**Returns**
- ROE & ROCE — current, 3Y avg, 5Y avg

**Dividends**
- Dividend history (5Y), payout ratio trend

**Ownership**
- Promoter holding — current + last 8 quarters trend; flag if pledging > 10%
- FII & DII holding trends — last 8 quarters

**Qualitative**
- Moat type (cost, brand, switching costs, network effect, regulatory)
- Pricing power, market share
- Management quality & governance track record
- Sector tailwinds / headwinds
- Latest earnings commentary & guidance
- Top 3 recent news items

**Peers**
- 3 closest peers with: P/E, P/B, ROE, Revenue Growth (3Y), D/E

---

## Step 3 — Apply Interpretation Rules

| Metric | Interpretation |
|---|---|
| **Valuation** | Cheap = below sector & 5Y history; Fair = within ~10%; Expensive = above both |
| **D/E** | <1 Safe · 1–2 Moderate · >2 Leveraged |
| **Interest Coverage** | >3 Healthy · 1.5–3 Watch · <1.5 Risk |
| **Current Ratio** | >1.5 Comfortable · 1–1.5 Watch · <1 Risk |
| **FCF** | Positive & growing = Strong · Positive & stable = Stable · Negative = Concern |
| **ROE / ROCE** | >15 Good · 10–15 Average · <10 Weak |
| **Growth trend** | Accelerating · Steady · Slowing · Declining |

---

## Step 4 — Generate Output

### QUICK TAKE (150–220 words)

Structure:
1. **Company overview** — what it does, sector, market position (1–2 sentences)
2. **CMP · Market Cap · P/E** + valuation verdict (cheap/fair/expensive vs sector & history)
3. **D/E · ROE · ROCE** with interpretation labels
4. **Growth trend** — revenue & profit CAGR direction
5. **3 Strengths** (evidence-backed, brief)
6. **2 Watch-points** (risks or concerns)
7. **Fundamental Quality** — Strong / Moderate / Weak + one-line rationale
8. **Price chart** — embed a TradingView widget or link to a price chart
9. End with: *"Want the full Deep Dive?"*

---

### DEEP DIVE

**Read the template first:** `assets/deep-dive-template.html`

Instructions:
- Replace ALL `{{PLACEHOLDER}}` tokens with real data
- Output ONLY the completed HTML starting with `<style>`
- Do not wrap in markdown fences
- Tabs to populate: Snapshot · Valuation · Growth · Health · Returns · Peers · Ownership · View
- The **View tab** must include:
  - Strengths (3–5, evidence-backed)
  - Watch-points (3–5, evidence-backed)
  - Key metric to track
  - Overall Fundamental Quality (Strong / Moderate / Weak) + rationale
  - Disclaimer
  - Data Confidence (High / Moderate / Low) + reasoning
- Embed a TradingView price chart widget in the Snapshot tab

---

### COMPARE

Side-by-side table for both stocks covering:
CMP · Market Cap · P/E · P/B · EV/EBITDA · Revenue CAGR · Profit CAGR · EBITDA Margin · ROE · ROCE · D/E · Promoter Holding · Pledging · Dividend Yield

Then:
- **Where A Leads** (3–4 points)
- **Where B Leads** (3–4 points)
- **Neutral investor-style summary** — no winner declared
- Include price chart links / TradingView widgets for both stocks

---

### PROS & CONS

- **Strengths** — 3 to 5, each with evidence (metric / source)
- **Risks** — 3 to 5, each with evidence
- **Balanced summary** — 2–3 sentences, no recommendation

---

### PORTFOLIO FIT

- Concentration analysis (sector weight after adding this stock)
- Sector overlap with existing holdings
- What this stock adds (e.g., growth, dividends, defensiveness)
- What it duplicates
- Compact fundamental snapshot (CMP, P/E, ROE, D/E, growth)
- Discuss fit without advising any action

---

## Step 5 — Closing Line (mandatory on every output)

> *"This is a view of the fundamentals for educational purposes only. It is not investment advice and not a buy/sell/hold recommendation. Verify all figures independently. The final decision is yours."*

---

## Jargon Guide (explain on first use)

| Term | Plain-English explanation |
|---|---|
| P/E | Price-to-Earnings: how much you pay per rupee of profit |
| P/B | Price-to-Book: price vs net assets on the books |
| EV/EBITDA | Enterprise value vs operating profit — useful for capital-heavy businesses |
| EBITDA Margin | Operating profit as % of revenue |
| FCF | Free Cash Flow — profit that actually lands as cash after capex |
| CAGR | Compound Annual Growth Rate — annualised growth over a period |
| D/E | Debt-to-Equity — how much debt vs shareholders' money |
| ROE | Return on Equity — profit earned per rupee of shareholder funds |
| ROCE | Return on Capital Employed — efficiency across all capital including debt |
| Moat | Sustainable competitive advantage that protects profits |
| Pledging | Promoters pledging shares as loan collateral — a governance risk signal |
| FII / DII | Foreign / Domestic Institutional Investors |

---

## Asset Reference

| File | Purpose |
|---|---|
| `assets/deep-dive-template.html` | Base HTML for Deep Dive output — read before generating Deep Dive |
