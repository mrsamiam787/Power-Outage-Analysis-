# Power-Outage-Analysis-
This page goes into depth into my data analysis on power outage durations in the US

# Introduction  
This project uses a comprehensive U.S. power‐outage dataset to answer one central question:

> **Can we predict the true duration of a power outage (in hours) based on event characteristics and regional factors?**

Understanding outage duration helps utilities and policy‐makers allocate resources, improve response times, and minimize social and economic impacts for millions of customers.

# Dataset Overview  
- **Number of rows**:  
  ```python
  n_rows = df_clean.shape[0]
  print(f"Number of outage events: {n_rows}")

