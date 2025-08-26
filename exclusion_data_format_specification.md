# Exclusion Data Format Specification v1.0

## Overview
This document defines the standardized format for exclusion data to ensure consistency across all data sources and enable efficient analysis and backtesting.

## File Format
- **Type**: CSV (UTF-8 encoding with BOM)
- **Delimiter**: Comma (,)
- **Text Qualifier**: Double quotes (") for fields containing commas
- **Line Endings**: Unix (LF) or Windows (CRLF) acceptable

## Required Fields (Core)

### 1. Company
- **Field Name**: `Company`
- **Type**: String
- **Required**: Yes
- **Description**: Full legal name of the company
- **Example**: `Meta Platforms, Inc.`
- **Notes**: Use official name from SEC filings when available

### 2. Symbol
- **Field Name**: `Symbol`
- **Type**: String (Uppercase)
- **Required**: Yes
- **Description**: Primary stock ticker symbol
- **Example**: `META`
- **Notes**: Use primary listing ticker; for dual-listed, use US ticker

### 3. ISIN
- **Field Name**: `ISIN`
- **Type**: String (12 characters)
- **Required**: Recommended
- **Description**: International Securities Identification Number
- **Example**: `US30303M1027`
- **Format**: 2-letter country code + 9-character alphanumeric + 1 check digit

### 4. Category
- **Field Name**: `Category`
- **Type**: String (Controlled Vocabulary)
- **Required**: Yes
- **Description**: Primary exclusion category
- **Allowed Values**:
  - `Environmental`
  - `Social`
  - `Governance`
  - `Product`
  - `Controversial Business`
  - `Regulatory`
  - `Ethics`
  - `Financial`
- **Example**: `Environmental`

### 5. Criteria
- **Field Name**: `Criteria`
- **Type**: String
- **Required**: Yes
- **Description**: Specific reason for exclusion
- **Example**: `Coal mining operations exceeding 30% of revenue`
- **Best Practices**:
  - Be specific and measurable when possible
  - Include thresholds if applicable
  - Use consistent terminology

### 6. Concerned Groups
- **Field Name**: `Concerned_Groups`
- **Type**: String (Semicolon-delimited list)
- **Required**: Yes
- **Description**: Stakeholder groups affected or raising concerns
- **Example**: `Environmental NGOs; Indigenous Communities; Investors`
- **Standard Groups**:
  - `Investors`
  - `Customers`
  - `Employees`
  - `Environmental NGOs`
  - `Human Rights Organizations`
  - `Indigenous Communities`
  - `Local Communities`
  - `Regulators`
  - `Media`
  - `Academic Institutions`
  - `Religious Organizations`
  - `Labor Unions`

### 7. Decision
- **Field Name**: `Decision`
- **Type**: String (Controlled Vocabulary)
- **Required**: Yes
- **Description**: Action taken
- **Allowed Values**:
  - `Exclude`
  - `Divest`
  - `Underweight`
  - `Watch`
  - `Engage`
  - `Conditional Exclude`
- **Default**: `Exclude`

### 8. Date
- **Field Name**: `Date`
- **Type**: Date (ISO 8601)
- **Required**: Yes
- **Description**: Date exclusion was added/decided
- **Format**: `YYYY-MM-DD`
- **Example**: `2024-01-15`

### 9. Notes
- **Field Name**: `Notes`
- **Type**: String
- **Required**: No
- **Description**: Additional context or details
- **Example**: `Pending review after Q1 2024 sustainability report`

## Extended Fields (Recommended)

### 10. Sub_Category
- **Field Name**: `Sub_Category`
- **Type**: String
- **Description**: More specific categorization
- **Examples**: `Carbon Emissions`, `Labor Rights`, `Data Privacy`

### 11. Theme
- **Field Name**: `Theme`
- **Type**: String (Controlled Vocabulary)
- **Description**: Classification of exclusion type
- **Allowed Values**:
  - `product_based` - What the company produces/sells
  - `conduct_based` - How the company operates
  - `mixed` - Both product and conduct issues
  - `regulatory` - Regulatory/compliance driven

### 12. Source
- **Field Name**: `Source`
- **Type**: String
- **Description**: Data provider or research source
- **Examples**: `MSCI ESG`, `Sustainalytics`, `Internal Research`, `ISS ESG`

### 13. Source_Date
- **Field Name**: `Source_Date`
- **Type**: Date (ISO 8601)
- **Description**: Date of source report/assessment
- **Format**: `YYYY-MM-DD`

### 14. Confidence_Score
- **Field Name**: `Confidence_Score`
- **Type**: Float (0.0 to 1.0)
- **Description**: Confidence level in the exclusion decision
- **Example**: `0.95`

### 15. Severity
- **Field Name**: `Severity`
- **Type**: String (Controlled Vocabulary)
- **Description**: Severity level of the issue
- **Allowed Values**: `Critical`, `High`, `Medium`, `Low`

### 16. Revenue_Exposure
- **Field Name**: `Revenue_Exposure`
- **Type**: Float (Percentage)
- **Description**: Percentage of revenue from problematic activity
- **Example**: `45.5` (for 45.5%)

### 17. Geographic_Exposure
- **Field Name**: `Geographic_Exposure`
- **Type**: String (Semicolon-delimited)
- **Description**: Countries/regions of concern
- **Example**: `Myanmar; Sudan; Russia`

### 18. Review_Date
- **Field Name**: `Review_Date`
- **Type**: Date (ISO 8601)
- **Description**: Next scheduled review date
- **Format**: `YYYY-MM-DD`

### 19. End_Date
- **Field Name**: `End_Date`
- **Type**: Date (ISO 8601) or NULL
- **Description**: When exclusion ends (NULL = permanent)
- **Format**: `YYYY-MM-DD`

### 20. Related_Companies
- **Field Name**: `Related_Companies`
- **Type**: String (Semicolon-delimited symbols)
- **Description**: Related entities also affected
- **Example**: `GOOGL; GOOG` (for Alphabet share classes)

## Additional Identifiers (Optional)

### 21. CUSIP
- **Field Name**: `CUSIP`
- **Type**: String (9 characters)
- **Description**: Committee on Uniform Securities Identification Procedures number

### 22. SEDOL
- **Field Name**: `SEDOL`
- **Type**: String (7 characters)
- **Description**: Stock Exchange Daily Official List number

### 23. LEI
- **Field Name**: `LEI`
- **Type**: String (20 characters)
- **Description**: Legal Entity Identifier

### 24. Exchange
- **Field Name**: `Exchange`
- **Type**: String
- **Description**: Primary exchange
- **Example**: `NASDAQ`, `NYSE`, `LSE`

## Metadata Fields

### 25. Record_ID
- **Field Name**: `Record_ID`
- **Type**: String (UUID recommended)
- **Description**: Unique identifier for this exclusion record
- **Example**: `550e8400-e29b-41d4-a716-446655440000`

### 26. Parent_Record_ID
- **Field Name**: `Parent_Record_ID`
- **Type**: String (UUID)
- **Description**: Links to previous version if this is an update

### 27. Created_Date
- **Field Name**: `Created_Date`
- **Type**: DateTime (ISO 8601)
- **Description**: When record was created in system
- **Format**: `YYYY-MM-DDTHH:MM:SSZ`

### 28. Modified_Date
- **Field Name**: `Modified_Date`
- **Type**: DateTime (ISO 8601)
- **Description**: Last modification timestamp
- **Format**: `YYYY-MM-DDTHH:MM:SSZ`

### 29. Created_By
- **Field Name**: `Created_By`
- **Type**: String
- **Description**: User/system that created the record

## Example CSV Header Row
```csv
Company,Symbol,ISIN,Category,Criteria,Concerned_Groups,Decision,Date,Notes,Sub_Category,Theme,Source,Source_Date,Confidence_Score,Severity,Revenue_Exposure,Geographic_Exposure,Review_Date,End_Date,Related_Companies
```

## Example Data Rows

### Product-Based Exclusion
```csv
"Philip Morris International Inc.",PM,US7185461040,Product,"Tobacco production exceeding 5% revenue threshold","Health Organizations; Investors; Religious Organizations",Exclude,2024-01-15,"Core tobacco business model",Tobacco,product_based,MSCI ESG,2024-01-10,1.0,Critical,95.0,,2024-07-15,,
```

### Conduct-Based Exclusion
```csv
"Wells Fargo & Company",WFC,US9497461015,Governance,"Systematic account fraud and customer deception","Customers; Regulators; Media",Exclude,2023-10-01,"Multiple regulatory settlements",Consumer Protection,conduct_based,Internal Research,2023-09-28,0.85,High,,"United States",2024-04-01,2025-01-01,
```

### Mixed Theme Exclusion
```csv
"ExxonMobil Corporation",XOM,US30231G1022,Environmental,"Climate lobbying and high carbon operations","Environmental NGOs; Investors; Indigenous Communities",Exclude,2024-02-01,"Fossil fuel operations and misleading climate communications",Carbon Emissions,mixed,"Sustainalytics; MSCI ESG",2024-01-25,0.92,Critical,75.0,"Global",2024-08-01,,XON
```

## Validation Rules

### Required Field Validation
1. `Company` OR `Symbol` must be present (preferably both)
2. `Category` must be from allowed values
3. `Date` must be valid ISO 8601 format
4. `Decision` must be from allowed values (default to "Exclude" if empty)

### Data Quality Checks
1. `Symbol` should be uppercase
2. `ISIN` should match regex: `^[A-Z]{2}[A-Z0-9]{9}[0-9]$`
3. `Confidence_Score` must be between 0.0 and 1.0
4. `Revenue_Exposure` must be between 0 and 100
5. Dates should be logical (End_Date > Date, Review_Date > Date)

### Consistency Rules
1. If `Theme` is "product_based", `Criteria` should mention products/services
2. If `Theme` is "conduct_based", `Criteria` should mention behaviors/actions
3. Related companies should have their own exclusion records if applicable

## Multi-Source Integration

When combining data from multiple sources:

### Duplicate Detection
- Primary key: `Symbol` + `Category` + `Source` + `Date`
- Near-duplicates: Same `Symbol` + `Category` within 30 days

### Merge Strategy
1. Keep earliest `Date` as primary exclusion date
2. Combine unique `Concerned_Groups` with semicolon delimiter
3. Concatenate different `Notes` with " | " separator
4. Average `Confidence_Score` if multiple sources
5. Take highest `Severity` level
6. Keep all `Source` attributions

### Consensus Tracking
- Flag records where 2+ sources agree (same Symbol + similar Criteria)
- Calculate consensus strength: (number of sources) / (total sources)
- Track source agreement patterns

## Best Practices

### Data Entry
1. Use consistent company names (legal names from SEC filings)
2. Verify ticker symbols against current market data
3. Include ISINs when available for international matching
4. Be specific in Criteria - include thresholds and measurements
5. Use standard Concerned Groups names for better aggregation

### Updates and Amendments
1. Don't modify existing records - create new ones with Parent_Record_ID
2. Use End_Date to terminate exclusions rather than deleting
3. Document changes in Notes field
4. Maintain audit trail with Created/Modified metadata

### Source Attribution
1. Always attribute the source of exclusion decisions
2. Include source date for temporal tracking
3. Note if multiple sources confirm the same exclusion
4. Track source reliability over time

## Output Format for Analysis

The processed output CSV should include these computed fields:

```csv
Symbol,Company,ISIN,Category,Theme,Theme_Binary_Product,Theme_Binary_Conduct,Criteria,Concerned_Groups_List,Concerned_Groups_Count,Decision,Date_Added,Year,Quarter,Month,Source_List,Source_Count,Has_Consensus,Confidence_Avg,Severity_Max,Revenue_Exposure,Review_Status,Days_Since_Added,Notes_Combined
```

## Change Log
- **v1.0** (2024-12-26): Initial specification
  - Core fields defined
  - Extended fields added
  - Validation rules established
  - Multi-source integration guidelines

## Implementation Notes

### For Manual Data Entry
- Use the provided Excel template with dropdown validations
- Follow the field descriptions carefully
- Run validation script before submission

### For Automated Integration
- Implement field mapping for your source
- Apply validation rules programmatically
- Log any data quality issues
- Generate standardized output

### For Analysis Systems
- Parse dates to pandas datetime
- Convert percentage fields to floats
- Create binary indicators for themes
- Calculate derived metrics (consensus, age, etc.)