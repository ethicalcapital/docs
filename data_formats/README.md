# Data Formats Directory

This directory contains all data format specifications and templates for Ethical Capital Investment Collaborative's portfolio management and investment decision systems.

## Structure

### Specifications
- `portfolio_data_format_specification.md` - Comprehensive guide for all portfolio data formats
- `exclusion_data_format_specification.md` - Format for exclusion screening data

### Templates

#### Core Investment Data
- `universe_data_template.csv` - Master security screening database
- `exclusion_data_template.csv` - Exclusion tracking template

#### Portfolio Management
- `growth_portfolio_template.csv` - Portfolio composition and rebalancing
- `growth_stats_template.csv` - Portfolio statistics summary
- `portfolio_weights_history_template.csv` - Historical position weightings (rebalance dates)
- `performance_history_template.csv` - Monthly performance metrics (month-end)

#### Investment Research
- `research_notes_template.csv` - Research notes and questions tracking
- `tick_history_template.csv` - TICK score change audit trail
- `workbench_activity_template.csv` - Workbench session activity log

#### Reporting
- `performance_deck_template.csv` - Comprehensive performance analytics
- `website_performance_template.csv` - Strategy-level performance for website

### Working Data Examples
- `Port 5.0 - Universe (2).csv` - Example universe data
- `Port 5.0 - Growth.csv` - Example portfolio composition
- `Port 5.0 - Growth Stats.csv` - Example portfolio statistics
- `Port 5.0 - Growth Vertical History.csv` - Example allocation history
- `Port 4.9 - Growth.csv` - Previous version example
- `Deck - Sheet1.csv` - Example performance deck

## Key Concepts

### TICK Score System
- Range: -100 to 100
- Minimum for portfolio inclusion: 10
- Tracked historically with full audit trail

### Timing Distinctions
- **Portfolio Weights**: Set at rebalance (new moon)
- **Performance**: Measured at month-end
- These are separate timelines and tracked in separate files

### Data Linkages
- All data linked via Ticker as primary key
- Research notes linked via Note_IDs
- Workbench sessions linked via Session_IDs
- Complete audit trail from analysis → decision → portfolio impact

## Usage

1. Use templates as starting points for new data files
2. Follow specifications for field definitions and validation rules
3. Maintain linkages between related data files
4. Update monthly according to rebalance schedule

## Integration

These formats are designed for:
- Clean integration with Colab notebooks
- Compatibility with Garden platform workbench
- Compliance and audit requirements
- Performance attribution analysis