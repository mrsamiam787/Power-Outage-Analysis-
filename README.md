# Power-Outage-Analysis-
This page goes into depth into my data analysis on power outage durations in the US

# Introduction

The **U.S. Power Outages** dataset records major power outages in the US from January 2000 through July 2016. The data is aggregated by state and month and has columns with details  on timing, cause, economic impact, and customer disruption.  

**Project Question:**  
> **How accurately can we predict the duration of a power outage (in hours) using only event timing, cause, and regional characteristics?**  

Understanding and predicting outage duration is critical for utilities, emergency planners, and consumers.  Faster and more reliable predictions enable better resource allocation, outage mitigation strategies, and communication with affected customers.

---

## Dataset Overview

- **Number of rows:** `<NUMBER_OF_ROWS>`  <!-- replace with `df.shape[0]` -->
- **Total columns:** 68  
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


