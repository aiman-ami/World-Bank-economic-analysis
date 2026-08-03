# World Bank Economic Indicators Analysis (1990 - Present)

**Tools:** Python · pandas · NumPy · matplotlib
**Data:** 217 countries · 3 indicators · 7,812 rows
**Skills shown:** Data cleaning · Wide-to-long reshaping · Feature engineering · EDA · Data visualization

## The One-Line Summary

Pakistan has consistently trailed its South Asian neighbours in GDP per capita for over two decades, while inflation spikes have been sharp, shock-driven, and recurring.

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
| unemployment_pct | Unemployment as % of total labour force |
| economic_stress_index | Engineered: mean of inflation % and unemployment %, computed with available values when one is missing |
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
- Engineered `economic_stress_index`, `gdp_category`, and `decade` columns
- Exported two outputs: full dataset (7,812 rows) and complete cases only (5,522 rows)

## Analysis and Visualizations

### Q1. How has Pakistan's GDP per capita trended vs the South Asia average?
![Pakistan GDP per Capita vs South Asia Average](Pakistan_GDP_per_Capita_vs_South_Asia_Average.png)

Pakistan tracked closely with the South Asia average through the late 1990s. After 2000 the gap widened steadily as India and Sri Lanka drove the regional average upward through sustained productivity gains. Pakistan's trajectory shows boom-bust cycles rather than structural growth, suggesting dependence on remittances and consumption over export-led transformation.

### Q2. When did inflation crises hit Pakistan?
![Pakistan Inflation Rate](Pakistan_Inflation_Rate.png)

Inflation exceeded 10% during two concentrated periods: the early-to-mid 1990s (1991-1997) and the 2008 to 2011 post-GFC spillover, with the worst spike in 2023 at nearly 31%. Notably, 2001-2003 was Pakistan's calmest inflation stretch in the entire series, under 3.3% each year. These are sharp shocks, not gradual drift, meaning Pakistan's inflation is largely shock-driven rather than chronic demand-pull. This matters for currency stability and monetary policy modeling.

### Q3. Where does Pakistan sit in South Asia today?
![South Asia GDP per Capita 2024](South_Asia_GDP_per_Capita.png)

As of 2024, Pakistan's GDP per capita stands at $1,479, second lowest in South Asia above only Nepal ($1,447). It sits far below India ($2,695), Bangladesh ($2,593), and Sri Lanka ($4,516). Despite being the second largest economy in South Asia by population, Pakistan ranks near the bottom by per capita income.

### Q4. Has global income convergence happened?
![Global Income Category Distribution by Decade](Global_Income_Category_Distribution_by_Decade.png)

The share of High Income and Upper-Middle Income countries has grown each decade since the 1990s, consistent with broad development gains across East Asia, Eastern Europe, and Latin America. The Low Income share fell sharply, from roughly 38% of country-year observations in the 1990s to about 11% by the 2020s, the largest shift of any tier. Convergence has been real and broad-based, though the remaining Low Income countries as of the 2020s likely represent the hardest structural cases left.

## Key Findings

1. **Pakistan's GDP growth has been volatile and structurally lagging.** Pakistan has trailed the South Asia average in GDP per capita since around 2000, with the gap widening each decade. While India and Sri Lanka drove the regional average upward through sustained productivity and export-led growth, Pakistan's trajectory has been marked by boom-bust cycles rather than structural transformation.
2. **Pakistan has experienced two distinct inflation crises.** Annual inflation exceeded 10% during two concentrated periods: the early-to-mid 1990s and the 2008-2011 post-GFC spillover. These were sharp spikes, not gradual drift, indicating Pakistan's inflation is largely shock-driven rather than chronic demand-pull.
3. **Pakistan ranks 5th out of 6 South Asian countries** in GDP per capita as of 2024.
4. **Global income convergence is real and broad-based.** The Low Income share of country-year observations fell from roughly 38% in the 1990s to 11% in the 2020s, the largest shift of any income tier, while High and Upper-Middle Income shares both grew steadily.
5. Inflation and unemployment both carry meaningful reporting gaps in this dataset, inflation is missing in 23.0 percent of rows and unemployment in 16.4 percent. The economic stress index should be interpreted with caution as a result, since a portion of its values are built from only one of the two underlying indicators rather than both.

## Outputs

| File | Description |
|---|---|
| world_bank_clean.csv | Full cleaned dataset, 7,812 rows, real countries only |
| world_bank_complete_cases.csv | Rows with all 3 indicators present, 5,522 rows |

## How to Run

1. Place `GDP.csv`, `inflation.csv`, and `unemployment.csv` in the same folder as `world_bank_data_cleaning.ipynb`.
2. Install requirements: `pip install pandas numpy matplotlib`
3. Open `world_bank_data_cleaning.ipynb` and run all cells from top to bottom (Restart Kernel and Run All is recommended to ensure the country filter and every downstream step apply cleanly).
4. Cleaned outputs (`world_bank_clean.csv`, `world_bank_complete_cases.csv`) and all four charts will be generated in the notebook's working directory.

## About Me

Aiman Ishaq
Data analytics student building real projects on real data.

[GitHub] · [LinkedIn]
