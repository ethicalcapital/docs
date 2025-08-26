# Portfolio Data Format Specification
## Version 1.0

---

## Overview

This specification defines the data formats for Ethical Capital Investment Collaborative's portfolio management and security screening systems. These formats enable systematic tracking of investment decisions, portfolio construction, and performance analysis while maintaining consistency with the firm's ethical screening framework.

---

## 1. Universe Data Format (Security Screening Database)

### Purpose
Master database containing all securities under consideration, their exclusion status, TICK scores, and portfolio inclusion decisions.

### File Naming Convention
`Port [Version] - Universe.csv` (e.g., "Port 5.0 - Universe.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Excluded** | String | Binary exclusion decision | "Yes", "No" | Must be "Yes" or "No" |
| **Workflow** | String | Current workflow status | "Under Review", "" | Optional |
| **Ticker** | String | Trading symbol | "MELI" | Required for equities |
| **ISIN** | String | International Securities ID | "US58733R1023" | 12-character format when provided |
| **TICK_Score** | Integer | Proprietary rating (-100 to 100) | 36 | Range: -100 to 100, minimum 10 for portfolio inclusion |
| **Security_Name** | String | Full company/fund name | "MercadoLibre" | Required |
| **Note** | String | Investment thesis/concerns | "Latin America's e-commerce marketplace" | Free text |
| **Last_TICK_Date** | Date | Date of last TICK review | "6/26/2025" | MM/DD/YYYY format |
| **Category** | String | Investment category | "Technology", "Funds" | From defined list |
| **Sector** | String | GICS sector classification | "Consumer Cyclical" | Standard sectors |
| **Benchmark** | String | Benchmark inclusion | "S&P 500" | Optional |
| **Fund** | String | Fund classification | "" | For fund vehicles only |
| **Growth_Portfolio** | String | Inclusion in Growth | "Yes", "" | Binary |
| **Income_Portfolio** | String | Inclusion in Income | "Yes", "" | Binary |
| **Diversification_Portfolio** | String | Inclusion in Diversification | "Yes", "" | Binary |
| **TICK_History** | String | Historical TICK scores | "30,33,36" | Comma-separated |
| **Updated_By** | String | Data source | "Database", "Manual" | Required |
| **Last_Updated** | DateTime | Last modification | "07/31/2025 12:34" | MM/DD/YYYY HH:MM |
| **Has_Price_Data** | String | Price data availability | "Yes", "No" | Binary |
| **Has_Fundamental_Data** | String | Fundamental data availability | "Yes", "No" | Binary |
| **Data_Status** | String | Data completeness | "Complete", "Partial" | Required |

### Categories
- Technology
- Consumer Cyclical
- Financials
- Industrials
- Healthcare
- Materials
- Energy
- Utilities
- Real Estate
- Communication Services
- Consumer Defensive
- Funds

### Verticals (Investment Themes - Growth Strategy Only)
- Innovation
- Infrastructure
- Lending
- Real Estate

Note: Verticals apply only to the Growth strategy. Income and Diversification strategies use different classification systems.

---

## 2. Growth Portfolio Data Format

### Purpose
Track current Growth strategy portfolio composition, rebalancing targets, and allocation analytics.

### File Naming Convention
`Port [Version] - Growth.csv` (e.g., "Port 5.0 - Growth.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **TICK** | Integer | Current TICK score | 36 | Range: -100 to 100, minimum 10 for inclusion |
| **Symbol** | String | Trading symbol | "MELI" | Required |
| **Name** | String | Security name | "MercadoLibre" | Required |
| **New** | Percentage | Target allocation | "11.08%" | 0-100% |
| **Old** | Percentage | Current allocation | "11.79%" | 0-100% |
| **Delta** | Percentage | Position change | "-0.71%" | Calculated field |
| **Div_Yield** | Percentage | Dividend yield | "0.17%" | Optional |
| **Div_Yield_Contribution** | Percentage | Yield contribution to portfolio | "0.02%" | Calculated |
| **FCF_Cap** | Percentage | Free cash flow to cap | "6.23%" | Optional |
| **FCF_Contribution** | Percentage | FCF contribution | "0.69%" | Calculated |
| **Vertical** | String | Investment vertical | "Innovation" | From defined list |
| **Country** | String | Domicile country | "United States" | Required |
| **United_States** | Percentage | US allocation | "6.15%" | Regional breakdown |
| **Asia** | Percentage | Asia allocation | "11.08%" | Regional breakdown |
| **Latin_America** | Percentage | LATAM allocation | "11.08%" | Regional breakdown |
| **Europe** | Percentage | Europe allocation | "" | Regional breakdown |
| **Market_Cap_USD_BLN** | Decimal | Market cap in billions | 122.066 | In USD billions |
| **Avg_Volume** | Integer | Average daily volume | 338283 | Shares |
| **Price** | Decimal | Current price | 2407.74 | In USD |
| **Dollar_Volume_MLN** | Decimal | Dollar volume in millions | 81.45 | Calculated |
| **Sector** | String | GICS sector | "Consumer Discretionary" | Standard sectors |

### Vertical Allocation Fields
For each vertical (Real Estate, Infrastructure, Innovation, Lending):
- **[Vertical]_Current** - Current allocation percentage
- **[Vertical]_After_Rebal** - Post-rebalancing allocation

---

## 3. Growth Statistics Format

### Purpose
Summary statistics for Growth portfolio positions including weights, changes, and classifications.

### File Naming Convention
`Port [Version] - Growth Stats.csv` (e.g., "Port 5.0 - Growth Stats.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Ticker** | String | Trading symbol | "MELI" | Required |
| **TICK** | Integer | Current TICK score | 36 | Range: -100 to 100, minimum 10 for inclusion |
| **Name** | String | Security name | "MercadoLibre" | Required |
| **Old** | Percentage | Previous weight | "11.79%" | 0-100% |
| **Change** | Percentage | Weight change | "-0.71%" | Calculated |
| **Weight** | Percentage | Current weight | "11.08%" | 0-100%, sum to 100% |
| **Yield** | Percentage | Dividend yield | "0.17%" | Optional |
| **Vertical** | String | Investment vertical | "Innovation" | From defined list |
| **Sector** | String | GICS sector | "Consumer Discretionary" | Standard sectors |

---

## 4. Portfolio Weights History Format

### Purpose
Track historical portfolio composition and weightings at each rebalance date (new moon).

### File Naming Convention
`Port [Version] - [Strategy] Weights History.csv` (e.g., "Port 5.0 - Growth Weights History.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Type** | String | Data type | "Security", "Vertical", "Sector", "Cash" | From defined list |
| **Name** | String | Security ticker or category | "MELI", "Infrastructure" | Required |
| **Strategy** | String | Strategy name | "Growth", "Income", "Diversification" | From defined list |
| **TICK_Score** | Integer | Current TICK (securities only) | 36 | -100 to 100 for securities |
| **Sector** | String | GICS sector (securities only) | "Consumer Discretionary" | For security rows |
| **[Date Columns]** | Percentage | Weight on rebalance date | "11.08%" | Date in M-D-YYYY format |

### Data Types
- **Security**: Individual position weightings with TICK scores
- **Vertical**: Growth strategy vertical allocations (Innovation, Infrastructure, Lending, Real Estate)
- **Sector**: Sector allocations by GICS classification
- **Cash**: Cash position percentage

### Date Column Format
- Columns represent rebalance dates (new moon)
- Format: M-D-YYYY (e.g., "1-9-2025", "2-8-2025")
- Empty cells indicate no rebalance (position unchanged from previous)
- All weights in a column sum to 100%

---

## 5. Performance History Format

### Purpose
Track monthly returns and portfolio metrics separately from weightings.

### File Naming Convention
`Port [Version] - [Strategy] Performance History.csv` (e.g., "Port 5.0 - Growth Performance History.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Metric** | String | Performance metric type | "Monthly_Return", "Holdings_Count" | From defined list |
| **Strategy** | String | Strategy name | "Growth", "Income", "Diversification" | From defined list |
| **[Date Columns]** | Number | Metric value at month-end | "2.50%", "25" | Date in MM-DD-YYYY format |

### Performance Metrics
- **Monthly_Return**: Month-on-month return percentage
- **Holdings_Count**: Number of positions at month-end
- **Turnover**: Portfolio turnover for the month
- **Cash_Balance**: Dollar value of cash position
- **AUM**: Assets under management at month-end

### Date Column Format
- Columns represent month-end dates
- Format: MM-DD-YYYY (e.g., "01-31-2025", "02-28-2025")
- Monthly returns are from previous month-end to current month-end
- All computed metrics (YTD, 1Y, Sharpe, etc.) derived from monthly returns

---

## 6. Performance Analytics Formats

### 6A. Portfolio Analytics Deck

#### Purpose
Comprehensive security-level performance metrics and analytics for portfolio positions.

#### File Naming Convention
`Deck - Sheet1.csv` or `Port [Version] - Deck.csv`

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Ticker** | String | Trading symbol | "MELI" | Required |
| **Name** | String | Full security name | "MercadoLibre Inc." | Required |
| **Weight** | Percentage | Portfolio weight | "11.22%" | 0-100% |
| **Div_Yld_Ind** | Percentage | Indicated dividend yield | "0.16%" | Optional |
| **Div_Cont** | Percentage | Dividend contribution | "0.02%" | Calculated |
| **Price_Book** | Decimal | Price to book ratio | 21.6 | Positive number |
| **PB_Cont** | Decimal | P/B contribution | 2.42 | Calculated |
| **FCF_Yld_LTM** | Percentage | FCF yield (last 12 months) | "5.99%" | Optional |
| **FCF_Cont** | Percentage | FCF contribution | "0.67%" | Calculated |
| **Net_Income_Margin_LTM** | Percentage | Net margin % | "8.52%" | Optional |
| **NIM_Cont** | Percentage | NIM contribution | "0.96%" | Calculated |
| **Gross_Profit_Margin_LTM** | Percentage | Gross margin % | "51.54%" | Optional |
| **GP_Cont** | String/Decimal | GP contribution | "$0.06" | Calculated |
| **Market_Cap** | String/Number | Market capitalization | "$123,226.05" | In millions |
| **Total_Return_YTD** | Decimal | Year-to-date return | 0.4294 | As decimal |
| **Short_Interest** | Decimal | Short interest percentage | 0.0135 | As decimal |
| **Average_Cost** | Decimal | Average purchase price | 1758.20 | In USD |
| **Last_Price** | Decimal | Current price | 2430.62 | In USD |
| **PL** | Decimal | Profit/Loss in dollars | 582.32 | Calculated |
| **PL_Percent** | Decimal | Profit/Loss percentage | 0.4104 | As decimal |
| **Total_Return_Percent** | Percentage | Total return | "41.04%" | As percentage |

---

## 6. Data Relationships

### Security Flow
```
Universe (Master) → TICK Scoring → Portfolio Inclusion → Performance Tracking
```

### Key Relationships
1. **Ticker** is the primary key across all files
2. **TICK_Score** determines portfolio eligibility (minimum thresholds per strategy)
3. **Vertical** classification drives allocation targets
4. **Excluded** status prevents portfolio inclusion regardless of TICK score

### TICK Score Ranges
- 60-100: Exceptional (highest conviction)
- 50-59: Excellent
- 40-49: Strong
- 30-39: Good
- 20-29: Acceptable
- 10-19: Marginal (minimum for portfolio inclusion)
- 0-9: Below threshold (not eligible for portfolio)
- -1 to -49: Negative factors present
- -50 to -100: Significant concerns (may trigger exclusion review)

### 6B. Website Performance Data

#### Purpose
Strategy-level performance data for website display and client reporting.

#### File Naming Convention
`Website_Performance.csv` or `Strategy_Performance.csv`

#### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Date** | Date | Data date | "1/27/2025" | MM/DD/YYYY format |
| **Strategy** | String | Strategy name | "Growth" | Growth, Income, or Diversification |
| **YTD_Return** | Percentage | Year-to-date return | "19.29%" | As percentage |
| **1Y_Return** | Percentage | One year return | "24.02%" | As percentage |
| **3Y_Return** | Percentage | Three year annualized | "16.15%" | As percentage or "N/A" |
| **5Y_Return** | Percentage | Five year annualized | "N/A" | As percentage or "N/A" |
| **Since_Inception** | Percentage | Since inception annualized | "4.67%" | As percentage |
| **YTD_vs_Benchmark** | Percentage | YTD relative performance | "8.95%" | Positive/negative percentage |
| **1Y_vs_Benchmark** | Percentage | 1Y relative performance | "5.90%" | Positive/negative percentage |
| **3Y_vs_Benchmark** | Percentage | 3Y relative performance | "-24.23%" | Positive/negative percentage |
| **5Y_vs_Benchmark** | Percentage | 5Y relative performance | "N/A" | Positive/negative or "N/A" |
| **SI_vs_Benchmark** | Percentage | SI relative performance | "-21.05%" | Positive/negative percentage |
| **Holdings_Count** | Integer | Number of positions | 25 | Positive integer |
| **Cash_Percent** | Percentage | Cash allocation | "0.93%" | 0-100% |
| **Top_Holding** | String | Largest position ticker | "MELI" | Valid ticker |
| **Top_Weight** | Percentage | Largest position weight | "11.08%" | 0-100% |
| **Benchmark** | String | Benchmark index | "ACWI" | Standard benchmark |
| **Inception_Date** | Date | Strategy inception | "10/1/2021" | MM/DD/YYYY format |
| **Last_Updated** | Date | Last update date | "1/27/2025" | MM/DD/YYYY format |

#### Benchmark Definitions
- **Growth**: ACWI (All Country World Index)
- **Income**: AGG (Aggregate Bond Index) or custom blend
- **Diversification**: 60/40 (60% equity, 40% fixed income)

#### Integration with Website
- Data feeds strategy pages at https://ethicic-railway-production.up.railway.app/strategies/
- Updated monthly after rebalancing
- Historical data maintained for trend analysis

---

## 7. Research Notes and Questions Format

### Purpose
Track research history, open questions, and investment thesis development for securities in the universe.

### File Naming Convention
`Research_Notes.csv` or `Port [Version] - Research.csv`

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Ticker** | String | Trading symbol | "MELI" | Required, links to Universe |
| **Date** | Date | Entry date | "1/15/2025" | MM/DD/YYYY format |
| **Type** | String | Note type | "Question", "Research", "Thesis", "Risk", "Update" | From defined list |
| **Priority** | String | Priority level | "High", "Medium", "Low" | From defined list |
| **Status** | String | Current status | "Open", "In Progress", "Resolved", "Archived" | Required |
| **Subject** | String | Brief subject | "ESG controversy investigation" | Max 100 characters |
| **Note** | String | Full note text | "Investigating reports of labor issues at supplier facilities..." | Free text |
| **Source** | String | Information source | "NGO Report", "Earnings Call", "News" | Optional |
| **Author** | String | Note author | "SO" | Initials or name |
| **Resolution** | String | How resolved | "Confirmed no material issues" | Required if Status=Resolved |
| **TICK_Impact** | Integer | Impact on TICK score | "-5", "+10", "0" | Optional, numeric |
| **Related_Tickers** | String | Related securities | "AMZN,BABA" | Comma-separated list |
| **Follow_Up_Date** | Date | Review date | "2/1/2025" | Optional |

### Note Types
- **Question**: Open questions requiring research
- **Research**: Research findings and analysis
- **Thesis**: Investment thesis updates
- **Risk**: Risk factors and concerns
- **Update**: Corporate updates and changes
- **Meeting**: Meeting notes and insights
- **Model**: Financial model assumptions

### Integration with Universe
- Links to Universe via Ticker field
- Open high-priority questions may impact TICK scores
- Resolution of risks can trigger TICK review
- Historical notes inform the Note field in Universe

---

## 8. TICK History and Workbench Activity Format

### 8A. TICK History Format

#### Purpose
Maintain complete audit trail of TICK score changes with links to supporting research, notes, and workbench activity.

#### File Naming Convention
`TICK_History.csv` or `TICK_Audit_Trail.csv`

#### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **History_ID** | String | Unique identifier | "TH_20250115_MELI_001" | Auto-generated |
| **Ticker** | String | Security symbol | "MELI" | Required, links to Universe |
| **Date** | DateTime | Change timestamp | "1/15/2025 14:32:00" | MM/DD/YYYY HH:MM:SS |
| **Previous_TICK** | Integer | Prior TICK score | 30 | -100 to 100 |
| **New_TICK** | Integer | Updated TICK score | 36 | -100 to 100 |
| **Change_Delta** | Integer | Score change | +6 | Calculated |
| **Change_Type** | String | Type of change | "Manual", "Rebalance", "Event" | From defined list |
| **Change_Reason** | String | Brief reason | "Q4 earnings beat, margin expansion" | Required |
| **Research_Note_IDs** | String | Linked research notes | "RN_20250115_001,RN_20250114_003" | Comma-separated IDs |
| **Workbench_Session_ID** | String | Workbench session | "WB_20250115_143000" | Links to activity log |
| **Chart_Config_Used** | String | Chart configurations | "Valuation_Trends,Peer_Comp" | Chart templates used |
| **Data_Sources** | String | Data reviewed | "Sharadar_Fundamentals,Price_Data" | Data accessed |
| **Author** | String | Who made change | "SO" | Required |
| **Approved_By** | String | Approval if needed | "SO" | For significant changes |
| **Portfolio_Impact** | String | Effect on portfolios | "Added to Growth" | Optional |

#### Change Types
- **Manual**: Direct manual adjustment
- **Rebalance**: Monthly rebalancing review
- **Event**: Triggered by corporate event
- **Research**: Result of research findings
- **Model**: Model-driven update
- **Exclusion**: Related to exclusion decision

### 8B. Workbench Activity Log

#### Purpose
Track all workbench interactions for reproducibility and audit.

#### File Naming Convention
`Workbench_Activity_Log.csv` or `WB_Activity_[YYYY-MM].csv`

#### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Session_ID** | String | Unique session ID | "WB_20250115_143000" | Auto-generated |
| **Ticker** | String | Security analyzed | "MELI" | Required |
| **Timestamp** | DateTime | Activity time | "1/15/2025 14:30:00" | MM/DD/YYYY HH:MM:SS |
| **Action_Type** | String | Type of action | "Chart_View", "Data_Query" | From defined list |
| **Action_Details** | JSON | Specific action | {"chart":"valuation","period":"5Y"} | JSON format |
| **Chart_Config** | JSON | Chart configuration | {"type":"candlestick","overlays":["tick"]} | Full config |
| **Data_Accessed** | String | Data tables used | "prices,fundamentals,peers" | Comma-separated |
| **Query_Parameters** | JSON | Query details | {"tickers":["MELI","AMZN"],"metric":"PE"} | Query specifics |
| **Result_Summary** | String | Key findings | "PE below peer average" | Optional |
| **TICK_Changed** | Boolean | Did TICK change? | True | Yes/No |
| **New_TICK** | Integer | New score if changed | 36 | -100 to 100 |
| **Notes_Created** | String | Research notes added | "RN_20250115_001" | Note IDs |
| **Duration_Seconds** | Integer | Session duration | 1800 | In seconds |
| **User** | String | User ID | "SO" | Required |

#### Action Types
- **Chart_View**: Viewing a chart
- **Data_Query**: Running data query
- **Comparison**: Peer comparison
- **Config_Save**: Saving chart config
- **TICK_Update**: Updating TICK score
- **Note_Add**: Adding research note
- **Export**: Exporting data

### 8C. Linkage Structure

#### Cross-References
```
TICK_History.History_ID <-> Workbench_Activity.Session_ID
TICK_History.Research_Note_IDs <-> Research_Notes.Note_ID
TICK_History.Ticker <-> Universe.Ticker
Workbench_Activity.Notes_Created <-> Research_Notes.Note_ID
```

#### Data Flow
1. Workbench session starts → Session_ID created
2. Charts viewed, data analyzed → Activity logged
3. Decision made → TICK updated
4. TICK change → History record created with Session_ID
5. Research notes → Linked via Note_IDs

---

## 9. Update Frequency

| Data Type | Update Frequency | Timing |
|-----------|------------------|--------|
| Universe | Continuous | As information becomes available |
| Portfolio Composition | Monthly | New moon |
| TICK Scores | Monthly | During rebalancing review |
| Vertical History | Monthly | Post-rebalancing |
| Performance Metrics | Daily | Market close |

---

## 9. Data Quality Rules

### Mandatory Fields
- All securities must have: Ticker OR ISIN, Security_Name, TICK_Score, Excluded status
- Portfolio positions must have: Weight, Vertical, Sector

### Validation Rules
1. Portfolio weights must sum to 100% (±0.5% for rounding)
2. No excluded securities in portfolio
3. TICK scores must be current (reviewed within 60 days)
4. Regional allocations must sum to position weight
5. Vertical allocations must sum to 100%

### Data Completeness
- **Complete**: All mandatory fields populated, price and fundamental data available
- **Partial**: Mandatory fields populated, some optional data missing
- **Incomplete**: Missing mandatory fields

---

## 11. Integration with Exclusion Data

Securities in the Universe file should cross-reference with exclusion data:
- If Company appears in exclusion_data with Decision="Exclude", then Excluded="Yes"
- Exclusion criteria and concerned groups inform Note field
- TICK scoring incorporates exclusion screening as first gate

---

## 12. Revision History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| [Current Date] | 1.0 | Initial specification | SO |

---

## Appendix A: Standard Value Lists

### Sectors (GICS)
- Communication Services
- Consumer Discretionary
- Consumer Staples
- Energy
- Financials
- Health Care
- Industrials
- Information Technology
- Materials
- Real Estate
- Utilities

### Regions
- United States
- Europe
- Asia
- Latin America
- Africa
- Middle East
- Oceania

### Investment Verticals
- Innovation (disruptive technology, new business models)
- Infrastructure (essential services, utilities, transport)
- Lending (financial services, credit providers)
- Real Estate (property, REITs, development)

### Data Sources
- Database (automated feeds)
- Manual (analyst input)
- Hybrid (automated with manual override)