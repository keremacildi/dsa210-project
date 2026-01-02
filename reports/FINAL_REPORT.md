# Final Report  
**DSA 210 – Introduction to Data Science**  
**Project Title:** Predicting Daily PM2.5 Levels in New York City  
**Student:** Kerem Açıldı  

---

## 1. Motivation

PM2.5 (fine particulate matter) is one of the most harmful air pollutants due to its ability to penetrate deep into the lungs and bloodstream. Urban environments such as New York City experience elevated PM2.5 levels due to dense traffic, weather conditions, and human activity. The motivation of this project is to analyze how **traffic volume and weather variables** relate to daily PM2.5 concentrations in NYC and to evaluate whether these factors can be used to **predict PM2.5 levels using machine learning models**.

---

## 2. Research Questions

This project addresses the following questions:

1. Is there a relationship between traffic volume and daily PM2.5 levels in New York City?
2. How do weather variables (temperature, wind speed, precipitation) relate to PM2.5 concentrations?
3. Are there observable temporal patterns in PM2.5 levels?
4. Can daily PM2.5 levels be predicted using traffic and weather data?

---

## 3. Data Collection and Preparation  
*(Notebook: `01_data_collection.ipynb`)*

Three datasets were collected and processed:

### 3.1 Air Quality (PM2.5)
Daily PM2.5 measurements were obtained from the **EPA Air Quality System (AQS)**. Data was filtered to:
- `State Name = New York`
- `City Name = New York`

Daily mean PM2.5 values were computed to represent city-level air quality.

### 3.2 Weather Data
Weather data was retrieved using the **Meteostat API** for a single location corresponding to central New York City (latitude 40.7128, longitude −74.0060). Daily variables such as temperature, wind speed, and precipitation were extracted.

### 3.3 Traffic Data
Traffic volume data was obtained from **NYC Open Data**, which contains traffic observations collected exclusively within New York City. Daily total traffic volume was calculated by aggregating all observations per day.

### 3.4 Dataset Merging
All datasets were aggregated to **daily resolution** and merged on the date variable, resulting in a final daily panel used in subsequent analyses.

---

## 4. Exploratory Data Analysis and Hypothesis Testing  
*(Notebook: `02_eda_hypothesis_testing.ipynb`)*

### 4.1 Exploratory Data Analysis
Initial sanity checks confirmed data consistency and completeness. Univariate analysis revealed temporal variation in PM2.5 levels, including noticeable seasonal patterns.

Bivariate analysis showed:
- A positive association between traffic volume and PM2.5.
- Negative associations between wind speed and PM2.5, suggesting dispersion effects.

### 4.2 Hypothesis Testing

Two main hypotheses were tested:

**H1: PM2.5 is correlated with weather variables.**  
Correlation tests showed statistically significant relationships between PM2.5 and certain meteorological variables, particularly wind speed.

**H2: PM2.5 levels differ between high-traffic and low-traffic days.**  
Traffic days were grouped based on traffic volume. Both a **t-test** and a **Mann–Whitney U test** were conducted, and results indicated that high-traffic days tend to have higher PM2.5 levels.

---

## 5. Machine Learning Modeling  
*(Notebook: `03_ml_modeling.ipynb`)*

### 5.1 Problem Formulation
The task was formulated as a **regression problem**, with daily PM2.5 concentration as the target variable and traffic and weather features as predictors.

### 5.2 Data Splitting and Preprocessing
A **time-based train/test split** was used to prevent temporal leakage. Preprocessing steps were implemented using pipelines to ensure reproducibility.

### 5.3 Models
The following models were trained and evaluated:
- Dummy Regressor (baseline)
- Ridge Regression
- Lasso Regression
- Random Forest
- Gradient Boosting

Hyperparameters were tuned using **time-series cross-validation**.

### 5.4 Results
The **tuned Random Forest** model achieved the best performance on the test set:

- Mean Absolute Error (MAE): ≈ 3.42  
- Root Mean Squared Error (RMSE): ≈ 4.38  
- R²: ≈ 0.28  

Feature importance analysis showed that traffic volume and weather variables such as wind speed contributed meaningfully to predictions.

---

## 6. Limitations and Future Work

This analysis operates at a **city-wide aggregation level**, which may mask localized pollution patterns. Additionally, factors such as industrial emissions, construction activity, and regional pollution transport were not included.

Future work could incorporate:
- Finer spatial resolution
- Additional pollution sources
- Advanced temporal models

---

## 7. Conclusion

This project demonstrates how integrating air quality, traffic, and weather data can provide insight into PM2.5 dynamics in New York City. Statistical analysis confirmed meaningful relationships between pollution, traffic, and weather, while machine learning models showed that PM2.5 levels can be partially predicted using these variables. The results highlight both the potential and limitations of data-driven approaches in environmental analysis.

---

## 8. Reproducibility

All analyses are fully reproducible using the provided GitHub repository.  
Detailed environment setup and execution instructions are included in the README.
