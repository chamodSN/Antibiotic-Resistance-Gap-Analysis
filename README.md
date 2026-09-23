# The Antibiotic Resistance Gap
**A global analysis of antimicrobial resistance trends and their relationship to antibiotic consumption, built entirely in Excel.**

## Business Question
Which pathogen–antibiotic combinations are becoming resistant fastest globally and does national antibiotic *consumption* predict *resistance* rates, or are other factors doing most of the work?

## Data Sources
- Resistance rate data: WHO GLASS derived antimicrobial resistance surveillance, via [Our World in Data - Antibiotics](https://ourworldindata.org/antibiotics)
  - E. coli bloodstream infections resistant to cephalosporins
  - S. aureus bloodstream infections resistant to methicillin (MRSA)
- Antibiotic consumption data: WHO GLASS-AMC-derived defined daily doses (DID) per 1,000 people/day, via the same Our World in Data hub
- Coverage: ~90 countries, 2016–2022 (resistance), 2016 - 2023 (consumption)

## Tools & Techniques
| Area | Technique |
|---|---|
| Data acquisition | Power Query - Web/CSV connectors, connection-only staging queries |
| Data cleaning | Custom columns (`Text.Length`) to reliably separate real countries from regional aggregates using ISO-3166 country codes, rather than fragile text-name matching |
| Combining data | Power Query Append (stacking same-shaped pathogen tables) and Merge (joining resistance to consumption on Code + Year) |
| Data modeling | Power Pivot data model with DAX measures: average resistance rate, year-over-year change, 3-year rolling average, consumption-to-resistance ratio |
| Exploration | PivotTables - fastest-rising pairs, country-level outliers, consumption-vs-resistance by pathogen |
| Statistical testing | Analysis ToolPak - Pearson correlation, run separately per pathogen |
| Dashboard | KPI cards, PivotChart, slicers, sparklines, single-page layout |

## Key Findings
1. **Global resistance is rising.** The blended average across both tracked pairs climbed from 28.6% (2016) to 40.6% (2022) - a 12.0 percentage-point, ~42% relative increase in six years.
2. **E. coli/Cephalosporins is the fastest-rising and highest-overall pair.** Average resistance 41.4% across the period, rising from 32.1% (2016) to 45.1% (2022), a +13.0pp move - edging out S. aureus/Methicillin (32.7% average, +11.3pp over the same period). "Fastest-rising" here is measured as net percentage-point change from 2016 to 2022.
3. **Country-level disparity is extreme.** 2022 resistance ranged from under 5% (Denmark, Austria) to over 85% (Bangladesh, Cameroon) - roughly an 18x spread, suggesting healthcare infrastructure and antimicrobial stewardship explain far more of the variation than any single national metric.
4. **Consumption is a contributing factor, not the dominant driver.** Correlation between antibiotic consumption and resistance is weak-to-moderate and pathogen-specific: r = 0.37 for E. coli (r² ≈ 13.8% of variance explained), r = 0.44 for S. aureus (r² ≈ 19.2%). Splitting the analysis by pathogen - rather than blending both together - was necessary to see this difference; the combined figure (r = 0.38) obscured it.
5. **Consumption reporting has real coverage gaps.** Far fewer countries report antibiotic consumption data than resistance data, which limits the consumption-vs-resistance analysis to a subset of countries. This is treated as a known limitation, not an error, and is called out explicitly rather than silently dropped.

## Limitations
- Only two pathogen–antibiotic pairs are covered (E. coli/Cephalosporins, S. aureus/Methicillin); broader pairs (e.g. K. pneumoniae, N. gonorrhoeae) would strengthen the analysis.
- Consumption data covers meaningfully fewer countries than resistance data - correlation results reflect only the overlapping subset, not the full global picture.
- Correlation measures a linear association only; it does not establish causation, and known confounders (healthcare access, infection-control practice, diagnostic capacity) are not modeled directly.

## How to Reproduce
1. Download the source CSVs from [ourworldindata.org/antibiotics](https://ourworldindata.org/antibiotics) and add those to the data folder.
2. Open `global-amr-resistance-analysis.xlsx`, go to **Data → Queries & Connections**, and click **Refresh All** - all cleaning, joins, measures, and the dashboard update automatically.

## Author
Built by Chamod as a data-analyst portfolio project, applying Power Query, Power Pivot/DAX, and statistical analysis techniques to real-world WHO surveillance data.
