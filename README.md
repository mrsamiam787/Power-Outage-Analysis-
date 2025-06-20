# Power-Outage-Analysis-
This page goes into depth into my data analysis on power outage durations in the US

# Introduction

The **U.S. Power Outages** dataset records major power outages in the US from January 2000 through July 2016. The data is aggregated by state and month and has columns with details  on timing, cause, economic impact, and customer disruption.  

**Project Question:**  
> **How accurately can we predict the duration of a power outage (in hours) using only event timing, cause, and regional characteristics?**  

Understanding and predicting outage duration is critical for utilities, emergency planners, and consumers.  Faster and more reliable predictions enable better resource allocation, outage mitigation strategies, and communication with affected customers.

---

## Dataset Overview

- **Number of rows:** 1534
- **Total columns:** 57 
- **Rows represent:** individual outage events (start → restoration) in a given state/month.

---

## Relevant Columns

| Column Name                          | Description                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------|
| `duration_hours`                     | True outage duration, in hours (computed from start/end timestamps) — **target variable**     |
| `CUSTOMERS.AFFECTED`                 | Number of customers impacted by the outage                                                    |
| `POPULATION`                         | Total state population in the outage year                                                     |
| `start_hour`                         | Hour of day when the outage began (0–23)                                                      |
| `start_month`                        | Calendar month when the outage began (1–12)                                                   |
| `CAUSE.CATEGORY`                     | Broad cause of outage (e.g. Weather, Equipment Failure, Human Error)                          |
| `climate_region`                     | NOAA climate region of the state (nine U.S. regions)                                          |
| `had_hurricane`                      | Boolean flag indicating if a hurricane caused the outage (derived from `HURRICANE.NAMES`)    |

*All other columns in the original dataset were considered but only these eight are used as inputs (plus two engineered ratios) in our final prediction pipeline.*  

## Data Cleaning

To ensure accurate modeling and reflect the true outage‐reporting process, we applied the following steps:

1. **Removed metadata rows** at the top of the CSV so that the real header loaded correctly.  
2. **Standardized missing values** by converting any “NA” or blank entries into true blanks, enabling reliable downstream processing.  
3. **Merged separate date & time fields** into unified timestamp columns for outage start and restoration, then computed a new `duration_hours` field (in hours) from these timestamps.  
4. **Flagged mismatches** between the newly computed duration and the original reported duration (in minutes) to identify potentially erroneous records.  
5. **Converted key fields to numeric** (prices, customer counts, sales, GDP metrics, population) so they could be used directly by statistical models.  
6. **Extracted temporal features**—month, weekday, and hour—from the cleaned timestamps to capture seasonal and diurnal effects.  
7. **Created a hurricane indicator** (`had_hurricane`) from the presence of a named storm, and renamed select columns to a consistent `snake_case` format.

---

### Head of the Cleaned Data

<div style="overflow-x:auto; width:100%;">

<table style="margin:auto; table-layout:fixed; width:100%; border-collapse: collapse; font-size: 0.9em;">
  <thead>
    <tr>
      <th style="padding:6px; border:1px solid #ddd;">duration_hours</th>
      <th style="padding:6px; border:1px solid #ddd;">start_month</th>
      <th style="padding:6px; border:1px solid #ddd;">start_weekday</th>
      <th style="padding:6px; border:1px solid #ddd;">start_hour</th>
      <th style="padding:6px; border:1px solid #ddd;">CUSTOMERS.AFFECTED</th>
      <th style="padding:6px; border:1px solid #ddd;">POPULATION</th>
      <th style="padding:6px; border:1px solid #ddd;">CAUSE.CATEGORY</th>
      <th style="padding:6px; border:1px solid #ddd;">climate_region</th>
      <th style="padding:6px; border:1px solid #ddd;">had_hurricane</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:6px; border:1px solid #ddd;">1.43</td>
      <td style="padding:6px; border:1px solid #ddd;">1</td>
      <td style="padding:6px; border:1px solid #ddd;">5</td>
      <td style="padding:6px; border:1px solid #ddd;">0</td>
      <td style="padding:6px; border:1px solid #ddd;">1,234</td>
      <td style="padding:6px; border:1px solid #ddd;">37,253,956</td>
      <td style="padding:6px; border:1px solid #ddd;">Weather</td>
      <td style="padding:6px; border:1px solid #ddd;">Cold</td>
      <td style="padding:6px; border:1px solid #ddd;">No</td>
    </tr>
    <tr>
      <td style="padding:6px; border:1px solid #ddd;">20.45</td>
      <td style="padding:6px; border:1px solid #ddd;">1</td>
      <td style="padding:6px; border:1px solid #ddd;">0</td>
      <td style="padding:6px; border:1px solid #ddd;">13</td>
      <td style="padding:6px; border:1px solid #ddd;">45,678</td>
      <td style="padding:6px; border:1px solid #ddd;">25,145,561</td>
      <td style="padding:6px; border:1px solid #ddd;">Equipment</td>
      <td style="padding:6px; border:1px solid #ddd;">Cold</td>
      <td style="padding:6px; border:1px solid #ddd;">No</td>
    </tr>
    <tr>
      <td style="padding:6px; border:1px solid #ddd;">2.50</td>
      <td style="padding:6px; border:1px solid #ddd;">1</td>
      <td style="padding:6px; border:1px solid #ddd;">1</td>
      <td style="padding:6px; border:1px solid #ddd;">3</td>
      <td style="padding:6px; border:1px solid #ddd;">9,876</td>
      <td style="padding:6px; border:1px solid #ddd;">18,801,310</td>
      <td style="padding:6px; border:1px solid #ddd;">Weather</td>
      <td style="padding:6px; border:1px solid #ddd;">Warm</td>
      <td style="padding:6px; border:1px solid #ddd;">No</td>
    </tr>
    <!-- additional rows -->
  </tbody>
</table>

</div>