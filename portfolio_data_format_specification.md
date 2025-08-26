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
| **TICK_Score** | Integer | Proprietary rating (0-60) | 36 | Range: 0-60 |
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

### Verticals (Investment Themes)
- Innovation
- Infrastructure
- Lending
- Real Estate

---

## 2. Growth Portfolio Data Format

### Purpose
Track current Growth strategy portfolio composition, rebalancing targets, and allocation analytics.

### File Naming Convention
`Port [Version] - Growth.csv` (e.g., "Port 5.0 - Growth.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **TICK** | Integer | Current TICK score | 36 | Range: 0-60 |
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
| **TICK** | Integer | Current TICK score | 36 | Range: 0-60 |
| **Name** | String | Security name | "MercadoLibre" | Required |
| **Old** | Percentage | Previous weight | "11.79%" | 0-100% |
| **Change** | Percentage | Weight change | "-0.71%" | Calculated |
| **Weight** | Percentage | Current weight | "11.08%" | 0-100%, sum to 100% |
| **Yield** | Percentage | Dividend yield | "0.17%" | Optional |
| **Vertical** | String | Investment vertical | "Innovation" | From defined list |
| **Sector** | String | GICS sector | "Consumer Discretionary" | Standard sectors |

---

## 4. Vertical History Format

### Purpose
Track historical allocation across investment verticals over time (monthly on new moon).

### File Naming Convention
`Port [Version] - Growth Vertical History.csv` (e.g., "Port 5.0 - Growth Vertical History.csv")

### Required Fields

| Field | Type | Description | Example | Validation Rules |
|-------|------|-------------|---------|------------------|
| **Vertical** | String | Investment vertical | "Infrastructure" | From defined list |
| **Min** | Percentage | Historical minimum | "18.19%" | Calculated |
| **Max** | Percentage | Historical maximum | "29.10%" | Calculated |
| **[Date Columns]** | Percentage | Allocation on date | "21.54%" | Date in M-D-YYYY format |

### Date Column Format
- Columns represent monthly rebalancing dates
- Format: M-D-YYYY (e.g., "9-2-2021")
- Aligned with new moon trading schedule

---

## 5. Performance Deck Format

### Purpose
Comprehensive performance metrics and analytics for portfolio positions.

### File Naming Convention
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
- 50-60: Exceptional (highest conviction)
- 40-49: Strong
- 30-39: Good
- 20-29: Acceptable
- 10-19: Marginal
- 0-9: Avoid

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