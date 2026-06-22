#  Layoffs Exploratory Data Analysis

##  Overview
Explored a cleaned dataset of 1,995 global layoff events (March 2020 – 
March 2023) using SQL (MySQL) to identify which companies, industries, 
countries, and time periods were hit hardest during the COVID-19 era 
through the 2023 tech downturn.

This project builds directly on my 
[Layoffs Data Cleaning project](https://github.com/thuthu97/layoffs-data-cleaning) 
— using the same clean `layoffs_staging2` table as the foundation for analysis.

##  Objective
Move beyond data cleaning into actual insight generation: which companies 
laid off the most people, which industries and countries were hit hardest, 
how layoffs trended over time, and which companies led layoffs in each 
individual year.

##  Tools Used
- MySQL Workbench
- SQL (Window Functions, CTEs, DENSE_RANK, Running Totals)

##  Business Questions Answered
1. What was the single largest layoff event, and which companies shut down 
   entirely (100% staff laid off)?
2. Which companies had the highest total layoffs across the full dataset?
3. Which industries and countries were impacted most?
4. How did layoffs trend by year and by month?
5. What was the cumulative (rolling) total of layoffs over time?
6. Which companies led layoffs *within each individual year* (top 5/year)?

##  Key Findings

### Biggest Single Events
- The largest single layoff event was **12,000 employees** (Google)
- Several well-funded companies laid off **100% of staff** (complete 
  shutdowns) despite having raised significant funding — found by filtering 
  `percentage_laid_off = 1` and sorting by funds raised

### Top 5 Companies by Total Layoffs (entire dataset)
| Rank | Company | Total Laid Off |
|---|---|---|
| 1 | Amazon | 18,150 |
| 2 | Google | 12,000 |
| 3 | Meta | 11,000 |
| 4 | Salesforce | 10,090 |
| 5 | Philips | 10,000 |

### Industries & Countries Hit Hardest
| Top Industries | Total Laid Off | Top Countries | Total Laid Off |
|---|---|---|---|
| Consumer | 44,782 | United States | 254,874 |
| Retail | 43,613 | India | 35,993 |
| Other | 36,289 | Netherlands | 17,220 |

The United States accounted for over **7x more layoffs** than the next 
highest country (India), reflecting its outsized share of the global tech 
and consumer sectors represented in this dataset.

### Layoffs by Year
| Year | Total Laid Off |
|---|---|
| 2020 | 80,998 |
| 2021 | 15,823 |
| 2022 | **160,661** |
| 2023 | 125,677 |

**2022 was the peak year** — more than double 2020's initial COVID shock — 
likely reflecting the broader post-pandemic tech industry correction. 2021 
stands out as a brief recovery period with the lowest layoffs of the four 
years.

### Top Company Per Year (Top 5, Ranked)
Used a chained CTE with `DENSE_RANK() OVER (PARTITION BY year ORDER BY 
total_laid_off DESC)` to find the top 5 companies by layoffs *within each 
individual year* — surfacing which company "led" layoffs each year, rather 
than just overall.

##  Key Takeaways
- Layoffs were not evenly distributed — a small number of large tech 
  companies (Amazon, Google, Meta) account for a disproportionate share of 
  total layoffs
- 2022 marks the clear inflection point of the dataset, suggesting the 
  major correction came roughly 2 years after the initial pandemic-driven 
  disruption in 2020
- Combining `DENSE_RANK()` with `PARTITION BY` is essential for "top N per 
  group" questions — without it, a simple `ORDER BY ... LIMIT 5` would only 
  show overall leaders, hiding which companies led in earlier/quieter years

##  Files
- `Exploratory_Data_Analysis.sql` — full SQL analysis script
- Built on top of the cleaned dataset from 
  [layoffs-data-cleaning](https://github.com/thuthu97/layoffs-data-cleaning)

## 🔗 Related Project
[Layoffs Data Cleaning](https://github.com/thuthu97/layoffs-data-cleaning) — 
the data preparation project this analysis is built on.
