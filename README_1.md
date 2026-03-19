# Michigan Water & Population Trends Report
**Course Assignment:** Group 11 — Assignment 4  
**Team:** Birva Chudasama · Maria Namitha Nelson · Milina Jasvin Cordeiro  
**Tool:** Microsoft SQL Server Reporting Services (SSRS) | Report Builder 15.0  
**File:** `assignment.rdl`

---

## Overview

An interactive SSRS paginated report that analyzes **water usage trends across Michigan industries (2013–2022)** and **county-level population estimates across the U.S. (2010–2019)**. The report uses choropleth maps and charts to surface geographic and industry-level patterns, supporting data-driven decision-making in areas like resource management, policy-making, and economic planning.

---

## Datasets

### 1. Michigan Water Use Data (2013–2022)
**Source:** [Kaggle — Michigan Water Use Data](https://www.kaggle.com/datasets/oleksiimartusiuk/michigan-water-use-data-2013-to-2022) (compiled from Michigan.gov public sources)

Tracks water withdrawal by industry across three source types:

| Column | Description |
|---|---|
| `Industry` | Industry sector (e.g., Commercial-Institutional, Industrial-Manufacturing, Irrigation, Electric Power Generation, etc.) |
| `Year` | Year of recorded water usage (2013–2022) |
| `Great Lakes (Gallons)` | Water withdrawn from the Great Lakes |
| `Groundwater (Gallons)` | Water withdrawn from groundwater sources |
| `Inland Surface Water (Gallons)` | Water withdrawn from rivers and streams |
| `Total Gallons` | Sum of all three water sources |

### 2. County Population Totals: 2010–2019
**Source:** [U.S. Census Bureau — Population Estimates Program](https://www.census.gov/data/datasets/time-series/demo/popest/2010s-counties-total.html)

Annual population estimates for every U.S. county, derived by adding yearly estimates to the last decennial census baseline.

| Column | Description |
|---|---|
| `DIVISION` | Geographic division the county belongs to (9 total divisions) |
| `STATE` | Numeric state code |
| `STNAME` | State name |
| `COUNTY` | Numeric county code (FIPS) |
| `CTYNAME` | County name |
| `POPESTIMATE2010`–`POPESTIMATE2019` | Annual population estimates by year |

---

## Data Cleaning

Before building the report, the following preprocessing steps were applied:

- Removed unnecessary columns from both datasets
- Unpivoted the population estimate columns (wide → long format) to support yearly trend analysis
- Verified no null values were present in either dataset

---

## Report Visualizations & Key Findings

The report answers five analytical questions:

| Question | Visual | Finding |
|---|---|---|
| In each year, what percentage of water came from each source? | Stacked/pie chart by source type | **~80% of water came from the Great Lakes** |
| Which state had the highest population? | Bar/map chart | **California** had the highest population |
| Which industry consumed the most water? | Bar chart by industry | **Electric Power Generation** was the top consumer |
| Which county had the highest total water consumption? | County-level map | **Berrien County** had the highest water use |
| Which division had the highest population? | Division-level chart | **Division 5** had the highest population (out of 9 divisions) |

---

## Technical Architecture

### Report Components
- **Choropleth Map** — Mercator-projected polygon map of Michigan counties, color-coded using a graduated color range rule with legend
- **Multi-datasource Integration** — Two separate SQL Server databases queried simultaneously within one report
- **Dynamic Color Scaling** — Color range rules automatically adapt to data distribution

### SQL Queries

**Population data (Michigan counties):**
```sql
SELECT *
FROM [co-est2019-alldata]
WHERE STNAME = 'Michigan';
```

**Water use data (2014):**
```sql
SELECT *
FROM [water_use_data_2013_to_2022]
WHERE year = 2014;
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Microsoft Report Builder 15.0 | Report design and authoring |
| SQL Server Reporting Services (SSRS) | Report hosting and rendering |
| T-SQL | Data querying and filtering |
| RDL (Report Definition Language) | Report definition format |

---

## How to Open

1. Open **Microsoft Report Builder** (version 15.0+) or **SQL Server Data Tools (SSDT)**
2. Update the data source connection strings to point to your SQL Server instance:
   - `Data Source=<your-server>; Initial Catalog=population`
   - `Data Source=<your-server>; Initial Catalog=wateruse`
3. Open `assignment.rdl` and click **Run** to render the report

> **Note:** A screenshot of the rendered report is recommended in `/screenshots/` since `.rdl` files require SSRS to render.

---

## Conclusion

By combining U.S. Census population data with Michigan's water use records, this report reveals meaningful trends: the Great Lakes dominate as a water source, electric power generation is the heaviest industry consumer, and Berrien County leads in total water consumption. These insights can inform resource management strategies, environmental policy, and regional economic planning.

---

## Skills Demonstrated

- Designing geographic and industry-level visualizations in SSRS
- Connecting and querying multiple SQL Server databases within a single report
- Data cleaning: column removal, unpivoting, and null-value verification
- Working with real-world public datasets (U.S. Census Bureau, Kaggle)
- Applying color range rules for data-driven visual encoding in choropleth maps
- Writing and optimizing T-SQL queries for reporting contexts
