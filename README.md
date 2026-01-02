# DSA 210 Project – Air Pollution in New York City

## Project Overview
This project analyzes the relationship between **PM2.5 air pollution**, **traffic intensity**, and **weather conditions** in New York City using publicly available datasets. The goal is to understand temporal patterns and contributing factors to air pollution and to apply machine learning methods to predict daily PM2.5 levels.

---

## Research Questions
1. How does traffic volume relate to PM2.5 air pollution levels in New York City?
2. How do weather variables (temperature, wind speed, precipitation) affect PM2.5 concentrations?
3. Are there clear temporal patterns (seasonal or weekly) in PM2.5 levels?
4. Can daily PM2.5 concentrations be predicted from traffic and weather data using machine learning models?

---

## Datasets
- **Air Quality (PM2.5):** EPA Air Quality System daily summary data  
- **Traffic:** NYC Open Data traffic volume dataset  
- **Weather:** Historical daily weather data retrieved via API for NYC  

All datasets are aggregated to **daily resolution** and aligned at the **city level**.

---

## Spatial Assumptions and Justification
For air quality data, records were filtered to `State Name = New York` and `City Name = New York` in the EPA PM2.5 dataset. This selection is used as a proxy for New York City because EPA monitoring stations labeled under the city of New York are located within the NYC metropolitan boundary and are commonly used to represent city-level air quality.

The traffic dataset is sourced from NYC Open Data and contains only traffic observations collected within New York City administrative boundaries. Since the dataset is published and maintained by NYC authorities, all observation segments are assumed to spatially correspond to NYC road infrastructure, making it appropriate for city-wide aggregation.

Weather data is retrieved using a single latitude–longitude point corresponding to central New York City (40.7128, −74.0060). Given NYC’s relatively small geographic area and limited intra-city daily weather variation, a single representative location is sufficient for capturing city-wide weather patterns relevant to air pollution analysis.

All datasets are therefore spatially aligned at the city level and aggregated to daily resolution before merging on the date variable.

---

## Methodology
1. **Data Collection & Cleaning**
   - Downloaded raw datasets from official sources.
   - Filtered, cleaned, and aggregated data to daily city-level values.
   - Merged datasets on the date variable.

2. **Exploratory Data Analysis & Hypothesis Testing**
   - Visualized temporal trends and distributions.
   - Analyzed correlations between PM2.5, traffic, and weather variables.
   - Conducted statistical tests to evaluate hypothesized relationships.

3. **Machine Learning Modeling**
   - Defined a regression task with daily PM2.5 as the target variable.
   - Used time-based train/test splits to avoid data leakage.
   - Built a baseline model and multiple advanced models.
   - Evaluated models using MAE, RMSE, and R² metrics.

---

## Phase 3 – Machine Learning Summary
The machine learning task focuses on predicting **daily PM2.5 concentrations** using traffic volume and weather variables. A baseline dummy regressor was compared against several models, including Ridge, Lasso, Random Forest, and Gradient Boosting. The best-performing model was a **tuned Random Forest**, achieving approximately **MAE ≈ 3.42**, **RMSE ≈ 4.38**, and **R² ≈ 0.28** on the test set. Results indicate that traffic intensity and certain weather variables contribute meaningfully to PM2.5 prediction, though a substantial portion of variance remains unexplained, reflecting the complexity of air pollution dynamics.

---

## Reproducibility

All results in this project can be reproduced by following the steps below.

### Environment setup
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
````

### Execution order

Run the notebooks in the following order:

1. `notebooks/01_data_collection_cleaning.ipynb`
   Downloads raw datasets, cleans them, and produces the final merged dataset.

2. `notebooks/02_eda_hypothesis_testing.ipynb`
   Performs exploratory data analysis and statistical hypothesis testing.

3. `notebooks/03_ml_modeling.ipynb`
   Trains baseline and advanced machine learning models, evaluates performance, and reports final results.

All notebooks are self-contained and can be run sequentially without manual intervention once dependencies are installed.

---

## Repository Structure

```
.
├── notebooks/
│   ├── 01_data_collection_cleaning.ipynb
│   ├── 02_eda_hypothesis_testing.ipynb
│   └── 03_ml_modeling.ipynb
├── reports/
│   └── phase2_eda_hypothesis_summary.md
├── requirements.txt
└── README.md
```

---

## Notes

This project was completed as part of **DSA 210**. All code is written in Python, and all analyses are fully reproducible using the instructions above.
