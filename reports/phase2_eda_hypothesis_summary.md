# Phase 2 – Exploratory Data Analysis (EDA) and Hypothesis Testing

## 1. Dataset Description

The working dataset is `data/processed/nyc_air_weather_traffic_2023_daily.csv`.  
It contains daily observations for New York City for the year 2023 after merging air quality, weather, and traffic sources.

- Number of days (rows): 170  
- Number of variables (columns): 10  

Columns:

- `date` – calendar date (datetime)  
- `pm25` – daily mean PM2.5 concentration (µg/m³), computed from EPA AQS FRM/FEM site data aggregated to city level  
- `temp_mean` – daily mean air temperature (°C) from Meteostat  
- `humidity_mean` – daily mean relative humidity (empty / all missing for this dataset)  
- `wind_speed_mean` – daily mean wind speed (m/s) from Meteostat  
- `precip_mm` – daily total precipitation (mm) from Meteostat  
- `traffic_volume` – daily sum of traffic volume counts from NYC Open Data “Automated Traffic Volume Counts”  
- `weekday` – day of week (0 = Monday, …, 6 = Sunday)  
- `is_weekend` – boolean, `True` for Saturday/Sunday, `False` otherwise  
- `traffic_level` – categorical, `"high"` if daily `traffic_volume` above the median, `"low"` otherwise  

Missing values:

- `pm25`, `temp_mean`, `wind_speed_mean`, `precip_mm`, `traffic_volume`, `weekday`, `is_weekend`, and `traffic_level` have 0 missing values.  
- `humidity_mean` has 170 missing values (no humidity data available from the chosen Meteostat source), so it is excluded from the statistical analyses.

The dataset therefore satisfies the course requirements for a clean, daily-level panel with one row per date and no missing values in the main analysis variables.

---

## 2. Exploratory Data Analysis

### 2.1 Univariate distributions

From the summary statistics (`df.describe()`):

- **PM2.5**  
  - Count: 170  
  - Mean: `[PM25_MEAN]` µg/m³  
  - Median: `[PM25_MEDIAN]` µg/m³  
  - Min: `[PM25_MIN]` µg/m³  
  - Max: `[PM25_MAX]` µg/m³  

  The histogram of `pm25` shows a right-skewed distribution, with most days at relatively moderate concentrations and a small number of high-pollution days.

- **Temperature (`temp_mean`)**  
  - Mean: `[TEMP_MEAN]` °C  
  - Range: `[TEMP_MIN]` to `[TEMP_MAX]` °C  

  Values follow a seasonal pattern, with lower temperatures in winter months and higher in summer.

- **Wind speed (`wind_speed_mean`)**  
  - Mean: `[WIND_MEAN]` m/s  
  - Range: `[WIND_MIN]` to `[WIND_MAX]` m/s  

- **Precipitation (`precip_mm`)**  
  - Most days have low or zero precipitation, with occasional heavy-rain days producing the right tail of the distribution.

- **Traffic volume (`traffic_volume`)**  
  - Mean: `[TRAFFIC_MEAN]`  
  - Range: `[TRAFFIC_MIN]` to `[TRAFFIC_MAX]`  

The time series plot of `pm25` over 2023 shows day-to-day variability with some higher peaks, especially in certain periods, but no extreme outliers beyond generally plausible urban background levels.

### 2.2 Bivariate patterns

- A scatter plot of `pm25` vs `traffic_volume` indicates `[describe visually: weak/clear/no obvious]` association by eye.  
- The correlation matrix across `pm25`, `temp_mean`, `wind_speed_mean`, `precip_mm`, and `traffic_volume` confirms that correlations between predictors are not extreme (no absolute correlations close to 1), so multicollinearity is not severe at this stage.

---

## 3. Hypothesis Tests

### 3.1 H1 – Relationship between weather variables and PM2.5

**Research question (RQ1):**  
How do daily weather conditions relate to daily average PM2.5 levels in New York City?

**Hypotheses (per variable):**

- H0: There is no linear relationship between daily average PM2.5 and the weather variable.  
- H1: There is a non-zero linear relationship between daily average PM2.5 and the weather variable.

Because `humidity_mean` is entirely missing, the analysis uses:

- `temp_mean`
- `wind_speed_mean`
- `precip_mm`

Pearson correlation tests (from the notebook):

- Temp_mean: r = `[R_TEMP]`, p = `[P_TEMP]`, n = `[N_TEMP]`  
- Wind_speed_mean: r = `[R_WIND]`, p = `[P_WIND]`, n = `[N_WIND]`  
- Precip_mm: r = `[R_PRECIP]`, p = `[P_PRECIP]`, n = `[N_PRECIP]`  

Interpretation:

- For variables with **p < 0.05**, reject H0 and conclude there is statistically significant evidence of a linear relationship with PM2.5.  
- For variables with **p ≥ 0.05**, do not reject H0; evidence for a linear relationship is not strong at the 5% level.

Summarize in words according to your actual numbers. Example structure:

- Temperature shows `[no / a weak / a moderate]` `[positive/negative]` correlation with PM2.5 (r = …, p = …), so `[we / we do not]` reject H0.  
- Wind speed shows `[no / some]` evidence that higher wind is associated with `[lower / higher]` PM2.5, depending on the sign and significance.  
- Precipitation similarly shows `[describe]` relationship.

If you computed Spearman correlations as a robustness check, you can add a short sentence noting whether they qualitatively agree with the Pearson results.

---

### 3.2 H2 – High vs low traffic days

**Research question (RQ2):**  
Are daily PM2.5 levels significantly different on days with higher traffic volume compared to days with lower traffic volume?

**Hypotheses:**

- H0: Mean daily PM2.5 on high-traffic days equals mean daily PM2.5 on low-traffic days.  
- H1: Mean daily PM2.5 on high-traffic days is different from that on low-traffic days.

The variable `traffic_level` splits the days by the median `traffic_volume` into `high` and `low`. From the notebook:

- Number of high-traffic days: `[N_HIGH]`  
- Number of low-traffic days: `[N_LOW]`  
- High-traffic mean PM2.5: `[HIGH_MEAN]` µg/m³  
- Low-traffic mean PM2.5: `[LOW_MEAN]` µg/m³  

Statistical tests:

- Welch t-test: p = `[P_T_H2]`  
- Mann–Whitney U test: p = `[P_U_H2]`  

Interpretation:

- If the primary test (e.g., Welch t-test) yields p < 0.05, reject H0 and conclude that average PM2.5 is significantly different on high-traffic days compared to low-traffic days.  
- If p ≥ 0.05, do not reject H0; the data does not provide strong evidence of a difference in average PM2.5 between high and low traffic days.

Summarize according to your actual values, e.g.:

> In this dataset, high-traffic days have mean PM2.5 of `[HIGH_MEAN]` µg/m³ versus `[LOW_MEAN]` µg/m³ on low-traffic days. The t-test p-value of `[P_T_H2]` indicates `[no significant / a statistically significant]` difference at the 5% level.

---

### 3.3 H3 – Weekday vs weekend

**Research question (RQ3):**  
Do daily PM2.5 levels differ between weekdays and weekends?

**Hypotheses:**

- H0: Mean daily PM2.5 on weekdays equals mean daily PM2.5 on weekends.  
- H1: Mean daily PM2.5 on weekdays differs from that on weekends.

From the notebook:

- Weekday mean PM2.5: **11.60 µg/m³** (Weekday mean: 11.602831085817929)  
- Weekend mean PM2.5: **10.21 µg/m³** (Weekend mean: 10.208876320494019)  

Statistical tests:

- Welch t-test: p = **0.5115**  
- Mann–Whitney U test: p = **0.762**  

Interpretation:

- Both tests give p-values well above 0.05.  
- Therefore, do not reject H0.  
- The observed difference in means (~1.4 µg/m³ higher on weekdays) is small relative to the day-to-day variability and is not statistically significant at the 5% level.

---

## 4. Overall Summary

- The dataset combines EPA PM2.5, Meteostat weather data, and NYC traffic counts into a clean daily panel of 170 days for NYC in 2023.  
- PM2.5 values are within a plausible urban range, with some higher-pollution days but no extreme outliers.  
- Weather variables (temperature, wind, precipitation) show `[insert qualitative summary based on your H1 results: e.g., weak correlations, moderate negative correlation with wind, etc.]` with PM2.5; humidity could not be analyzed due to missing data.  
- Comparing high vs low traffic days (H2) yields `[insert conclusion based on your p-values: e.g., no statistically significant difference / a significant difference]` in average PM2.5.  
- Comparing weekdays to weekends (H3) shows a slightly higher mean PM2.5 on weekdays, but the difference is not statistically significant (t-test p = 0.5115, Mann–Whitney p = 0.762), so the data does not support a strong weekend effect in this year.

This completes the Phase 2 requirements: data collection, cleaning, exploratory analysis, and hypothesis testing aligned with the original research questions and hypotheses.
