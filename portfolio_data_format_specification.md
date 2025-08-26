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

## 4. Strategy History Format

### Purpose
Track historical portfolio composition, weightings, and performance metrics across all strategies over time (monthly on new moon).

### File Naming Convention
`Port [Version] - [Strategy] History.csv` (e.g., "Port 5.0 - Growth History.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Type** | String | Data type | "Security", "Vertical", "Performance" | From defined list |
| **Name** | String | Security ticker or metric | "MELI", "Total_Return" | Required |
| **Strategy** | String | Strategy name | "Growth", "Income", "Diversification" | From defined list |
| **TICK_Score** | Integer | Current TICK (securities only) | 36 | -100 to 100 for securities |
| **Sector** | String | GICS sector (securities only) | "Consumer Discretionary" | For security rows |
| **Min** | Number | Historical minimum | "5.00%" or "-15.20%" | Calculated |
| **Max** | Number | Historical maximum | "15.00%" or "45.30%" | Calculated |
| **Avg** | Number | Historical average | "10.25%" or "12.50%" | Calculated |
| **Current** | Number | Most recent value | "11.08%" | Latest column value |
| **[Date Columns]** | Number | Value on date | "11.08%" or "24.5%" | Date in M-D-YYYY format |

### Data Types
- **Security**: Individual position weightings with TICK scores
- **Vertical**: Growth strategy vertical allocations (Innovation, Infrastructure, Lending, Real Estate)
- **Sector**: Sector allocations
- **Performance**: Performance metrics (returns, volatility, sharpe ratio, etc.)
- **Cash**: Cash position

### Performance Metrics Tracked
- **Total_Return**: Cumulative total return
- **YTD_Return**: Year-to-date return
- **1Y_Return**: Rolling one-year return
- **3Y_Return**: Rolling three-year annualized return
- **Volatility**: Rolling volatility
- **Sharpe_Ratio**: Risk-adjusted return metric
- **Max_Drawdown**: Maximum peak-to-trough decline
- **Holdings_Count**: Number of positions
- **Turnover**: Portfolio turnover rate
- **Active_Share**: Deviation from benchmark

### Date Column Format
- Columns represent monthly rebalancing dates
- Format: M-D-YYYY (e.g., "9-2-2021")
- Aligned with new moon trading schedule
- Performance metrics calculated as of each date

---

## 5. Performance Analytics Formats

### 5A. Portfolio Analytics Deck

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

### 5B. Website Performance Data

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

## 6. Research Notes and Questions Format

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

## 7. Update Frequency

| Data Type | Update Frequency | Timing |
|-----------|------------------|--------|
| Universe | Continuous | As information becomes available |
| Portfolio Composition | Monthly | New moon |
| TICK Scores | Monthly | During rebalancing review |
| Vertical History | Monthly | Post-rebalancing |
| Performance Metrics | Daily | Market close |

---

## 8. Data Quality Rules

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

## 9. Integration with Exclusion Data

Securities in the Universe file should cross-reference with exclusion data:
- If Company appears in exclusion_data with Decision="Exclude", then Excluded="Yes"
- Exclusion criteria and concerned groups inform Note field
- TICK scoring incorporates exclusion screening as first gate

---

## 10. Revision History

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