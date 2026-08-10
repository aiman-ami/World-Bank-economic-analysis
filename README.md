# World Bank Economic Indicators Analysis (1990 - Present)

**Tools:** Python · pandas · NumPy · matplotlib · scipy
**Data:** 217 countries · 3 primary indicators · 7,812 rows
**Skills shown:** Data cleaning · Wide-to-long reshaping · Feature engineering · EDA · Statistical testing · Data visualization

## The One-Line Summary

Pakistan has consistently trailed its South Asian neighbours in GDP per capita for over two decades with significantly higher growth volatility, while inflation spikes have been sharp, shock-driven, and recurring.

## The Problem

The World Bank publishes country-level economic data in wide format, one column per year, with regional and income-group aggregates (like WLD, EUU, SSF) mixed into the same file as real countries. Three separate indicator files needed to be cleaned, reshaped, filtered, and merged before any analysis was possible.

This project answers four questions:

1. How has Pakistan's GDP per capita compared to the South Asia average since 1990?
2. When did inflation crises hit and how severe were they?
3. Where does Pakistan sit relative to its South Asian neighbours today?
4. Has global income convergence actually happened across decades?

## The Dataset

**Source:** World Bank Open Data (public)
**Files:** GDP.csv · inflation.csv · unemployment.csv
**Raw coverage:** 266 country/aggregate entities · 1960 to present · 3 indicators
**After filtering to real countries only:** 217 countries

| Column | What It Means |
|---|---|
| gdp_per_capita | GDP per capita in current USD |
| inflation_pct | Annual consumer price inflation % |
| unemployment_pct | Unemployment as % of total labour force (excluded from final analysis due to data quality concerns) |
| economic_stress_index | Engineered: mean of inflation % and unemployment %, calculated only when both values are present |
| stress_index_partial | Boolean flag: True when only one of the two stress indicators is available |
| gdp_category | Income tier: Low / Lower-Middle / Upper-Middle / High |
| decade | Decade label for grouped analysis |

## What I Built

### Step 1: Clean and Reshape
**File:** `world_bank_data_cleaning.ipynb`

Three problems with the raw files:
- Wide format: 71 columns per file, one per year
- Regional and income-group aggregates (WLD, EUU, SSF, and 46 others) mixed with real countries
- No shared structure across the three indicator files

Steps taken:
- Skipped 4 World Bank metadata rows on load
- Dropped unnamed trailing columns and redundant indicator columns
- Filtered to real countries using a country-code exclusion list covering all World Bank regional and income-group aggregates (49 codes excluded, verified against the full set rather than a partial list), leaving 217 real countries
- Melted all three files from wide to long format
- Merged all three indicators into one tidy dataset on Country Code and Year
- Engineered `economic_stress_index`, `stress_index_partial`, `gdp_category`, and `decade` columns
- Exported two outputs: full dataset (7,812 rows) and complete cases only (5,522 rows)

## Analysis and Visualizations

### Q1. How has Pakistan's GDP per capita trended vs the South Asia average?
![Pakistan GDP per Capita vs South Asia Average](Pakistan_GDP_per_Capita_vs_South_Asia_Average.png)

Pakistan was already below the South Asia average in 1990, but the gap widened sharply after 2000 as India and Sri Lanka drove the regional average upward. Pakistan's trajectory shows boom-bust cycles rather than steady convergence. A Levene's test confirms that Pakistan's year-over-year GDP growth volatility is significantly higher than the South Asia average (p &lt; 0.05). This pattern is consistent with economies reliant on volatile capital inflows, though testing that hypothesis would require additional data on remittances, exports, and sectoral composition.

### Q2. When did inflation crises hit Pakistan?
![Pakistan Inflation Rate](Pakistan_Inflation_Rate.png)

Inflation exceeded 10% during three concentrated periods: the early-to-mid 1990s (1991-1997), the 2008-2011 post-GFC spillover, and 2019-2024 (driven by COVID-19 supply shocks, currency depreciation, and the 2022 floods). The worst single-year spike was 2023 at 30.8%. Notably, 2001-2003 was Pakistan's calmest inflation stretch in the entire series, under 3.3% each year. These are sharp shocks, not gradual drift, meaning Pakistan's inflation is largely shock-driven rather than chronic demand-pull.

### Q3. Where does Pakistan sit in South Asia today?
![South Asia GDP per Capita 2024](South_Asia_GDP_per_Capita.png)

As of 2024, Pakistan's GDP per capita stands at $1,479, 5th out of 6 South Asian countries with available data (Afghanistan and Bhutan lack reported 2024 GDP per capita). It sits far below India ($2,695), Bangladesh ($2,593), and Sri Lanka ($4,516).

### Q4. Has global income convergence happened?
![Global Income Category Distribution by Decade](Global_Income_Category_Distribution_by_Decade.png)

The share of High Income and Upper-Middle Income countries has grown each decade since the 1990s, consistent with broad development gains across East Asia, Eastern Europe, and Latin America. The Low Income share fell sharply, from roughly 38% of country-year observations in the 1990s to about 11% by the 2020s. Limitation: These shares use period-specific World Bank thresholds; even so, some portion of the shift reflects nominal USD appreciation rather than purely real growth.

## Key Findings

1. **Pakistan's GDP growth has been volatile and structurally lagging.** Pakistan has trailed the South Asia average in GDP per capita since 1990, with the gap widening sharply after 2000. A Levene's test confirms Pakistan's year-over-year GDP growth volatility is significantly higher than the South Asia average (p &lt; 0.05), while India and Sri Lanka drove the regional average upward through steadier convergence.

2. **Pakistan has experienced three distinct inflation crises.** Annual inflation exceeded 10% during three concentrated periods: the early-to-mid 1990s, 2008-2011, and 2019-2024. These were sharp spikes, not gradual drift, indicating Pakistan's inflation is largely shock-driven rather than chronic demand-pull. The worst single-year spike was 2023 at 30.8%.

3. **Pakistan ranks 5th out of 6 South Asian countries** in GDP per capita as of 2024 (Afghanistan and Bhutan lack 2024 data), standing at $1,479.

4. **Global income convergence is real but incomplete.** The Low Income share of country-year observations fell from roughly 38% in the 1990s to 11% in the 2020s. This analysis uses period-specific income thresholds, though nominal USD drift means the true real-growth component is smaller than the nominal shift suggests.

5. **Data gaps are material and explicitly flagged.** Inflation is missing in 23.0% of post-1990 rows. The economic stress index is calculated only when both inflation and unemployment are present; partial calculations are flagged separately and excluded from index-based analysis.

## Outputs

| File | Description |
|---|---|
| world_bank_clean.csv | Full cleaned dataset, 7,812 rows, real countries only |
| world_bank_complete_cases.csv | Rows with all 3 indicators present, 5,522 rows |

## How to Run

1. Place `GDP.csv`, `inflation.csv`, and `unemployment.csv` in the same folder as `world_bank_data_cleaning.ipynb`.
2. Install requirements: `pip install pandas numpy matplotlib scipy`
3. Open `world_bank_data_cleaning.ipynb` and run all cells from top to bottom (Restart Kernel and Run All is recommended to ensure the country filter and every downstream step applies cleanly).
4. Cleaned outputs (`world_bank_clean.csv`, `world_bank_complete_cases.csv`) and all four charts will be generated in the notebook's working directory.

## About Me

Aiman Ishaq
Data analytics student building real projects on real data.
