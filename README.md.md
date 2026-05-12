World Bank Economic Data — Cleaning and Analysis

Author: Aiman Ishaq
Data Source: World Bank Open Data


What this project does

World Bank publishes country-level economic indicators in wide format, one column per year, with real countries and regional aggregates sitting in the same file. This notebook takes three separate indicator files, cleans and reshapes them into a single tidy dataset, and uses it to look at Pakistan's economic position against its South Asian peers across 30+ years.


Indicators used

GDP per capita (current USD)
Inflation, consumer prices (annual %)
Unemployment, total (% of total labour force)


Tools

Python, pandas, numpy, matplotlib


What the notebook covers

Loading raw World Bank CSVs — the files have 4 metadata rows at the top that need skipping before the actual data starts.

Dropping junk columns — each file comes with unnamed trailing columns and two redundant indicator columns that serve no purpose in analysis.

Filtering regional aggregates — World Bank files mix real countries with aggregates like WLD, ECA, SSA. The notebook uses ISO code pattern matching combined with an explicit exclusion list to keep only real countries.

Reshaping wide to long — year columns get unpivoted using pd.melt() so each row represents one country, one year, one value.

Merging all three indicators on Country Code and Year into a single dataset.

Filtering to 1990 onwards where coverage across countries is consistent enough to be useful.

Standardising country names — fixing World Bank naming conventions like "Korea, Rep." and "Egypt, Arab Rep." to readable names.

Feature engineering — a composite economic stress index (mean of inflation and unemployment), World Bank income category brackets, and decade labels for trend grouping.

Pakistan-focused analysis comparing GDP per capita, inflation history, and regional standing against South Asian peers.

Exporting two clean CSVs — one with all rows including NaNs, one with complete cases only.


Charts

Pakistan GDP per Capita vs South Asia Average (1990 to present)

Both lines start close together around $500 in 1990. From the mid-2000s they begin separating. By 2024 the South Asia average is approaching $5,000 while Pakistan sits flat around $1,500. The shaded gap between the two lines makes the divergence hard to miss — Pakistan has not kept pace with how the rest of the region has grown.

Pakistan Inflation Rate (1990 to present)

Three high-inflation periods stand out. The mid-1990s saw rates consistently above 10%. The 2008 spike hit just over 20%. Then a relatively stable window through the 2010s before inflation climbed again from 2020, peaking above 30% in 2023 — the worst reading in the entire 35-year dataset.

South Asia GDP per Capita — 2024

Pakistan at $1,479 sits near the bottom of the region, just above Nepal at $1,447. Bangladesh at $2,593 and India at $2,695 are nearly double. Sri Lanka at $4,516 and Maldives at $13,379 are in a different category entirely.

Global Income Category Distribution by Decade

The Low Income share dropped from around 40% of country-year observations in the 1990s to under 10% by the 2020s. High Income and Upper-Middle Income shares have grown each decade. The catch is that this shift has largely benefited the middle tiers — the very bottom has not moved much.


Key findings

Pakistan's GDP growth has been slow and largely flat in per capita terms while the South Asia average has accelerated, driven mainly by India and Sri Lanka. The gap in 2024 is not a recent development — it has been widening steadily since the mid-2000s.

Inflation in Pakistan is not a chronic background problem but a shock-driven one. The pattern across 35 years shows sharp spikes followed by recovery, not a steady upward drift. The 2023 spike at over 30% is the most severe in the dataset by a significant margin.

Global income convergence is real but uneven. Middle-income countries have graduated upward each decade, but the lowest-income tier has remained largely stuck, which suggests the gains from globalisation and development have had a ceiling for the poorest economies.

Unemployment data has the most coverage gaps of the three indicators. Any analysis using the economic stress index should account for this — rows where unemployment is missing still get a partial score from inflation alone, which changes what the index actually measures.


Files

world_bank_data_cleaning.ipynb — main notebook
data/GDP.csv — raw GDP per capita file from World Bank
data/inflation.csv — raw inflation file from World Bank
data/unemployment.csv — raw unemployment file from World Bank
world_bank_clean.csv — full cleaned and merged dataset, NaNs kept
world_bank_complete_cases.csv — rows where all three indicators are present
