PROJECT PROPOSAL

## Predicting Air Quality Using Weather and Traffic Data

### Motivation

Air pollution is a major environmental and public health problem. Predicting air quality can help cities reduce exposure risks and plan traffic or industrial regulations. This project aims to analyze the relationship between weather, traffic, and air pollution levels and to build a machine learning model that predicts air quality index (AQI) from environmental variables.

### Data Sources

1. **Air Quality Data:** Public data from [OpenAQ](https://openaq.org/) or the U.S. EPA’s [AirNow API](https://docs.airnowapi.org/).
2. **Weather Data:** Historical weather data (temperature, humidity, wind speed, precipitation) from [OpenWeatherMap API](https://openweathermap.org/api).
3. **Traffic Data (for enrichment):** Open traffic datasets from city portals (e.g., [NYC Open Data Traffic Volume](https://data.cityofnewyork.us/)) or TomTom traffic index.

### Data Collection Plan

* Query OpenAQ and OpenWeatherMap APIs for overlapping city and date ranges.
* Merge datasets on `datetime` and `city`.
* Clean data: handle missing values, convert units, and standardize timestamps.
* Store data as CSVs for reproducibility.

### Planned Analysis

1. **Exploratory Data Analysis (EDA):**

   * Visualize AQI trends over time and by weather condition.
   * Correlation heatmaps between pollutants and weather variables.
2. **Hypothesis Testing:**

   * Test if temperature, humidity, or wind speed significantly affect PM2.5 levels.
3. **Machine Learning:**

   * Regression models (Linear Regression, Random Forest, XGBoost).
   * Evaluate with RMSE and R².
4. **Visualization and Reporting:**

   * Time-series and geospatial plots.
   * Interactive dashboard (optional).

### Tools and Libraries

Python with:
`pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `requests`, `xgboost`, `plotly`

A `requirements.txt` will list dependencies.

### Ethical Considerations

All data sources are open-access. No personal data will be used. All processing steps will be transparent and reproducible.
