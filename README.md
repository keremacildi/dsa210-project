# PROJECT PROPOSAL  
## Predicting Urban PM2.5 Using Weather and Traffic Data

### Motivation

Air pollution is a major environmental and public health problem. Fine particulate matter (PM2.5) is particularly harmful because it can penetrate deep into the lungs and bloodstream. Predicting PM2.5 can help cities reduce exposure risks and plan traffic or industrial regulations.

This project aims to analyze the relationship between weather, traffic, and air pollution levels in a single urban area and to build a machine learning model that predicts daily PM2.5 (and, as an extension, air quality index – AQI) from environmental variables.

For concreteness, the initial focus is on New York City over a single full year (2023), using only open data.

---

### Research Questions

1. **Weather–Pollution Relationship**  
   How do daily weather conditions (temperature, humidity, wind speed, and precipitation) relate to daily average PM2.5 levels in New York City?

2. **Traffic–Pollution Relationship**  
   Are daily PM2.5 levels significantly different on days with higher traffic volume compared to days with lower traffic volume?

3. **Temporal Patterns**  
   Do daily PM2.5 levels differ systematically between weekdays and weekends, and are there meaningful seasonal differences (e.g., winter vs. summer)?

---

### Hypotheses

These hypotheses will be tested in the exploratory data analysis and hypothesis testing phase.

- **H1 – Weather vs PM2.5 (Correlation)**  
  - H0: There is no linear relationship between daily average PM2.5 and each weather variable (temperature, wind speed, precipitation, humidity).  
  - H1: At least one weather variable has a non-zero linear relationship with daily average PM2.5.

- **H2 – High vs Low Traffic Days (Group Comparison)**  
  - H0: Mean daily PM2.5 on “high-traffic” days equals mean daily PM2.5 on “low-traffic” days.  
  - H1: Mean daily PM2.5 on “high-traffic” days is different from mean daily PM2.5 on “low-traffic” days.

- **H3 – Weekday vs Weekend (Group Comparison)**  
  - H0: Mean daily PM2.5 on weekdays equals mean daily PM2.5 on weekends.  
  - H1: Mean daily PM2.5 on weekdays differs from that on weekends.

Additional hypotheses (e.g. seasonal effects) may be added later if data supports them.

---

### Data Sources (actual implementation)

1. **Air Quality Data (PM2.5)**  
   - Daily PM2.5 data from United States Environmental Protection Agency (EPA) — the pre-generated “daily_88101_2023.zip” dataset covering particulate matter (code 88101).  
   - Target variable: daily average PM2.5 for New York City (city-wide mean of available monitoring stations).

2. **Weather Data**  
   - Daily weather data (temperature, wind speed, precipitation, humidity if available) from Meteostat.  
   - Derived features: daily mean temperature (“temp_mean”), daily mean wind speed (“wind_speed_mean”), daily total precipitation (“precip_mm”), and humidity when available.

3. **Traffic Data (for enrichment)**  
   - Traffic volume counts from New York City Department of Transportation (NYC DOT) dataset “Automated Traffic Volume Counts”, accessed via public NYC Open Data API.  
   - Features: total daily traffic volume aggregated over all observation segments in 2023.

These sources are open-access and reproducible; all scripts to download and process them are included in the project repository.

---

### Data Collection Plan (actual implementation)

The data collection pipeline is implemented in `src/collect_data.py` (also reproducible in `notebooks/01_data_collection.ipynb`). The steps are:

1. Download and extract the EPA daily PM2.5 ZIP for 2023.  
2. Filter for New York City stations.  
3. Compute city-wide daily mean PM2.5 → save to `data/raw/openaq_nyc_2023.csv`.  
4. Download daily weather for NYC from Meteostat for 2023 → transform to daily means/totals → save as `data/raw/weather_nyc_2023.csv`.  
5. Fetch raw traffic volume counts from NYC DOT via the public API → convert timestamps to dates, sum volume per date → save to `data/raw/traffic_nyc_2023.csv`.  
6. Merge the three daily datasets on `date`, discard invalid or extreme values, and engineer additional variables:  
   - `weekday` (0–6),  
   - `is_weekend` (boolean),  
   - `traffic_level` (“high” vs “low” by median traffic volume).  
   Save merged dataset to `data/processed/nyc_air_weather_traffic_2023_daily.csv`.  

This pipeline ensures a clean daily-level panel dataset suitable for EDA, hypothesis testing, and later modeling.

---

### Planned Analysis (after Phase 2)

- **Exploratory Data Analysis (EDA)**: distributions, time-series plots, correlation matrix, scatter plots.  
- **Hypothesis Testing**: correlation tests (H1), two-sample tests (H2, H3).  
- **Prediction / Machine Learning**: baseline linear model + potential non-linear models (if time permits) to predict daily PM2.5 from weather + traffic features.  
- **Visualization & Reporting**: time-series, boxplots, summary report, optional interactive dashboard.

---

### Tools and Libraries

The project uses:

- `pandas`, `numpy` for data handling.  
- `requests` for HTTP downloads.  
- `meteostat` for weather retrieval.  
- `matplotlib` for plotting.  
- `scipy` for statistical tests.  
- `jupyter` for interactive notebooks.

All dependencies are listed in `requirements.txt`.  
