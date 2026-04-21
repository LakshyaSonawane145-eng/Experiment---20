# Experiment 20 — Project Analysis: COVID-19 Data Analysis

**Name:** Lakshya Sonawane &nbsp;|&nbsp; **PRN:** 25070123068

---

## 📌 Introduction

The COVID-19 pandemic resulted in millions of confirmed cases and deaths globally, making data analysis critical for tracking spread and informing policy. This project applies a complete data science pipeline — **ingestion, preprocessing, feature engineering, aggregation, and geospatial visualization** — to a real-world COVID-19 dataset to analyze global case distribution, country-wise impact, time-series trends, and India's state-level statistics.

---

## 📖 Theory

### 1. Data Loading and Preprocessing
- Dataset loaded using `pd.read_csv()`; irrelevant columns `SNo` and `Last Update` removed with `data.drop()`
- `ObservationDate` cast to `datetime64[ns]` using `.astype()` to enable date-based filtering and arithmetic
- `Confirmed`, `Deaths`, and `Recovered` cast to `int64` for accurate numerical aggregation

### 2. Feature Engineering — Active Cases
- New `Active` column derived as `data['Confirmed'] - data['Recovered'] - data['Deaths']`
- Unlike cumulative confirmed counts, active cases represent the real-time infectious burden at any point in time

### 3. Latest Date Snapshot
- Most recent date retrieved using `data['ObservationDate'].max()`
- Dataset filtered to that date via boolean indexing: `data[data['ObservationDate'] == data['ObservationDate'].max()]`
- Produces a current global snapshot used for country-level comparison

### 4. Country-Level Aggregation
- Latest snapshot grouped by `Country/Region` using `.groupby()` and summed across all case columns
- `.reset_index()` flattens the result back into a DataFrame
- Specific countries queried via boolean filtering, e.g., `countries[countries['Country/Region'] == "India"]`

### 5. Choropleth Map — Global Visualization
- Global case distribution visualized using `px.choropleth()` from Plotly Express
- `locationmode="country names"` resolves region names to map boundaries; `color_continuous_scale` sets the gradient
- `range_color=[0, 10000000]` prevents outliers from collapsing the color scale; separate maps created for confirmed (`"reds"`) and recovered (`"greens"`)

### 6. Time-Series Aggregation
- Full dataset grouped by `ObservationDate` and summed using `.groupby().sum().reset_index()`
- Produces a daily worldwide total time series for Confirmed, Deaths, Recovered, and Active cases throughout the outbreak

### 7. Top 20 Most Affected Countries
- Country totals sorted descending using `.sort_values(['Confirmed'], ascending=False)`
- Top 20 nations identified using `.head(20)`, revealing the most severely impacted countries

### 8. India State-Level Analysis
- India's records isolated using `data[data['Country/Region'] == "India"]`
- Missing `Province/State` values filled with `"Unknown"` using `.fillna()` to prevent group-by errors
- States ranked by confirmed cases; worst-affected state found using `top_state['Confirmed'].max()` with boolean indexing and `.values[0]`

---

## ✅ Conclusion

- A complete data science pipeline was applied to real-world COVID-19 data from raw ingestion to interactive maps
- Key steps covered type casting, active case derivation, snapshot filtering, country and time-series aggregation
- Choropleth maps using Plotly effectively visualized global disparities in confirmed and recovered cases
- India-specific drill-down demonstrated multi-level analysis from global → national → state granularity
