# PROJECT PROPOSAL  
## Predicting Urban PM2.5 Using Weather and Traffic Data

### Motivation

Air pollution is a major environmental and public health problem. Fine particulate matter (PM2.5) is particularly harmful because it can penetrate deep into the lungs and bloodstream. Predicting PM2.5 can help cities reduce exposure risks and plan traffic or industrial regulations.

This project aims to analyze the relationship between weather, traffic, and air pollution levels in a single urban area and to build a machine learning model that predicts daily PM2.5 (and, as an extension, air quality index – AQI) from environmental variables.

For concreteness, the initial focus is on New York City over a single full year (e.g., 2023), using only open data.

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
  - H0: There is no linear relationship between daily average PM2.5 and each weather variable (temperature, humidity, wind speed, precipitation).  
  - H1: At least one weather variable has a non-zero linear relationship with daily average PM2.5.

- **H2 – High vs Low Traffic Days (Group Comparison)**  
  - H0: Mean daily PM2.5 on “high-traffic” days equals mean daily PM2.5 on “low-traffic” days.  
  - H1: Mean daily PM2.5 on “high-traffic” days is different from that on “low-traffic” days.

- **H3 – Weekday vs Weekend (Group Comparison)**  
  - H0: Mean daily PM2.5 on weekdays equals mean daily PM2.5 on weekends.  
  - H1: Mean daily PM2.5 on weekdays differs from that on weekends.

Additional hypotheses (e.g., seasonal effects) may be added later if the data supports them.

---

### Data Sources

Planned data sources are open and reproducible:

1. **Air Quality Data (PM2.5)**  
   - Public data from [OpenAQ](https://openaq.org) or the U.S. EPA’s [AirNow API](https://docs.airnowapi.org).  
   - Target variable: daily average PM2.5 for New York City.

2. **Weather Data**  
   - Historical weather data (temperature, humidity, wind speed, precipitation) from the [OpenWeatherMap API](https://openweathermap.org) or equivalent open historical datasets (e.g., national meteorological services).  
   - Features: daily aggregates (means/sums) of key weather variables.

3. **Traffic Data (for enrichment)**  
   - Open traffic datasets from city portals, e.g., [NYC Open Data Traffic Volume](https://data.cityofnewyork.us) or similar sources such as TomTom traffic index (if accessible and allowed).  
   - Features: daily traffic volume or congestion index.

The goal is to align all three sources on a common city and date range (e.g., NYC, 2023) to create a single, merged daily-level dataset.

---

### Data Collection Plan

- Query OpenAQ and OpenWeatherMap (or equivalent) for overlapping city and date ranges (e.g., New York City, 2023).
- Obtain traffic data for the same city and period from NYC Open Data or similar.
- Merge datasets on `date` (and `city` where applicable).
- Clean data:
  - Handle missing values.
  - Convert units and standardize timestamps.
  - Remove clearly erroneous values (e.g., impossible temperatures, negative concentrations).
- Store all intermediate and final datasets as CSVs under a versioned `data/` folder for reproducibility.

---

### Planned Analysis

The project will follow the course phases:

1. **Exploratory Data Analysis (EDA)**  
   - Summarize distributions of PM2.5, weather, and traffic variables.  
   - Visualize PM2.5 trends over time (daily, weekly, seasonal).  
   - Compare PM2.5 distributions across:
     - High vs low traffic days.
     - Weekdays vs weekends.
     - Seasons (e.g., winter/spring/summer/fall).  
   - Compute correlation matrices between PM2.5, weather variables, and traffic measures.

2. **Hypothesis Testing**  
   - Test H1 using correlation tests (Pearson and/or Spearman) between PM2.5 and weather variables.  
   - Test H2 and H3 using two-sample tests (e.g., t-test or Mann–Whitney U) to compare mean PM2.5 between groups:
     - High vs low traffic days.
     - Weekdays vs weekends.  
   - Optionally extend to multi-group tests (e.g., ANOVA or Kruskal–Wallis) for seasonal differences.

3. **Machine Learning (Prediction)**  
   - Frame PM2.5 prediction as a regression problem with weather and traffic features as inputs.  
   - Baseline models: Linear Regression, Ridge/Lasso.  
   - Non-linear models: Random Forest, Gradient Boosting, XGBoost (if time and complexity allow).  
   - Evaluation: RMSE, MAE, and R² on a held-out test set or via cross-validation.  
   - Optional: Compare models with and without traffic features to quantify the added value of traffic data.

4. **Visualization and Reporting**  
   - Time-series and correlation plots for key variables.  
   - Clear visual comparisons of PM2.5 across groups (e.g., boxplots for high vs low traffic, weekday vs weekend).  
   - Optional: simple interactive dashboard (e.g., with Plotly or similar) for exploring the merged dataset.

---

### Tools and Libraries

The main implementation environment will be Python, using:

- `pandas`, `numpy` for data handling and preprocessing.  
- `matplotlib`, `seaborn`, `plotly` for visualization.  
- `scikit-learn` for machine learning models and evaluation.  
- `requests` (and/or similar) for API calls.  
- `xgboost` for gradient boosting models (if used).  

A `requirements.txt` file in the repository lists all dependencies needed to reproduce the analysis.

---

### Ethical Considerations

- All data sources are open-access and non-identifiable.  
- No personal or sensitive data will be used in this project.  
- All code, data-processing steps, and modeling choices will be kept transparent and reproducible through:
  - Version-controlled notebooks and scripts.
  - Documented preprocessing and feature engineering steps.
- Results will not be interpreted as medical or regulatory advice; they are intended for educational and exploratory purposes.
