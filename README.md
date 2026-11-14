# MIS 311 – Assignment #1, Part 1: Exploratory Data Analysis of Most Visited Countries

**Course:** MIS 311 – Introduction to Business Analytics, Eastern International University
**Dataset:** `07_Most_Visited_Countries.xlsx` (provided by the course)
**Tools:** Python (pandas, matplotlib) and Excel

This project explores international tourist arrivals by country to see which destinations dominate and how arrivals are changing after 2022.

---

## 1. Data Overview

| Item | Detail |
|---|---|
| Source | Course-provided file combining yearly arrival figures (2022, 2023, 2024 estimate) with World Bank international tourism arrivals |
| Size (raw) | 206 rows × 6 columns |
| Unit of analysis | One row per country |
| Context | International tourist arrivals, used to compare how popular destinations are |

| Column | Meaning |
|---|---|
| `country` | Country name |
| `MostVisited_NumOfArrivals_Millions_2022` / `_2023` | Arrivals in millions (only for the most-visited countries) |
| `MostVisited_NumOfArrivalsPredictive_Millions_2024` | Predicted / estimated 2024 arrivals in millions |
| `MostVisited_NumOfArrivals_WorldBank` | World Bank arrivals, in persons |
| `MostVisited_DataYear_WorldBank` | Year of the World Bank figure (1998–2020, mostly 2019–2020) |

## 2. Data Cleaning

| Issue | Action | Why |
|---|---|---|
| 3 exact duplicate rows (Eritrea, Montenegro, Brunei) | Removed → **203 rows** | They would double-count in the statistics |
| Missing 2024 estimate in 153 of 203 rows (75.4%), 2023 in 154 (75.9%), 2022 in 183 (90.1%) | Left blank, **not imputed** | The data only tracks the ~50 most-visited countries each year, so the gaps are structural. Filling them with a mean or median would invent tourism numbers for small destinations |
| Missing World Bank value and year for Taiwan (1 row) | Left blank | No reliable substitute; Taiwan does have 2023 and 2024 figures |
| World Bank arrivals in persons, other columns in millions | Converted to millions | Puts the arrival columns on one unit |
| World Bank data years differ (1998–2020) | Kept and flagged | World Bank figures are used for distribution only, not for year-on-year comparison |
| Long column names | Renamed, ordered 2022 → 2024 | Readability |

Every statistic is calculated on the values that are available. The full log is in the Excel file (sheet *Cleaning Log*), and the untouched original is kept in *Raw Data*.

## 3. Descriptive Statistics

| Variable (millions) | Count | Missing | Mean | Median | Std dev | Min | Max |
|---|---|---|---|---|---|---|---|
| Arrivals 2022 | 20 | 183 | 30.62 | 24.43 | 20.05 | 11.66 | 79.40 |
| Arrivals 2023 | 49 | 154 | 19.23 | 11.03 | 20.79 | 3.73 | 100.00 |
| Arrivals 2024 (est.) | 50 | 153 | 24.10 | 16.80 | 20.92 | 5.90 | 89.40 |
| World Bank arrivals | 202 | 1 | 4.94 | 0.92 | 12.99 | 0.0009 | 117.11 |

Note: the 2022 and 2023 columns cover different sets of countries, so their means cannot be compared directly. Growth is therefore calculated only for countries that have both years.

### Insight 1 – Tourism is highly concentrated in a few countries

![Concentration of arrivals](images/insight1_concentration.png)

Among the 50 countries with a 2024 estimate, the top 5 (France, Spain, United States, China, Italy) receive 31.7% of all arrivals and the top 10 receive 49.6%. The World Bank data tells the same story: the mean (4.94M) is more than five times the median (0.92M), the skewness is 5.4, and the top 10 countries hold 51.9% of arrivals while 105 of 202 countries receive under 1 million visitors. This means the average is a misleading summary for tourism data, so medians and log scales are better. It also means that demand is driven by a handful of markets, while most destinations compete for a small share of visitors.

### Insight 2 – Most countries are still growing, but the biggest destinations are growing slowest

![Growth 2023 to 2024](images/insight2_growth.png)

For the 37 countries with both 2023 and 2024 figures, 25 grew and 12 declined, with a median change of +12.7% (range: −50.4% for Saudi Arabia to +83.4% for Taiwan). Larger destinations tend to grow more slowly (Spearman correlation between 2023 arrivals and growth = −0.38): France fell 10.6% and Spain 1.7%, while Taiwan, the Philippines, South Korea and Vietnam grew between 43% and 83%. For a business, this suggests that growth opportunities sit in emerging and rebounding destinations rather than the mature leaders. However, 2024 values are estimates, so large swings such as Saudi Arabia and the UAE should be checked against the original source.

## 4. Limitations

- 2024 values are predictions, not final counts.
- Coverage differs by year (50, 49 and 20 countries), so results describe the most-visited countries only.
- World Bank figures come from different years and countries may define "arrival" differently.

## 5. Repository Contents

| File | Description |
|---|---|
| `README.md` | This report |
| `MIS311_Most_Visited_Countries_Analysis.xlsx` | Raw data, cleaned data, cleaning log, descriptive statistics (with live formulas) |
| `analysis.py` | Python script that reproduces the cleaning, statistics and charts |
| `data/` | Original dataset and cleaned CSV |
| `images/` | Charts used in this report |

## References

Eastern International University. (2026). *07_Most_Visited_Countries* [Data set]. MIS 311, Moodle.

World Bank. (n.d.). *International tourism, number of arrivals* [Data set]. https://data.worldbank.org
