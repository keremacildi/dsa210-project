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
  - Mean: 11.21 µg/m³  
  - Median: 7.35 µg/m³  
  - Min: 2.17 µg/m³  
  - Max: 189.99 µg/m³  

  The histogram of `pm25` shows a right-skewed distribution, with most days at relatively moderate concentrations and a small number of high-pollution days.

- **Temperature (`temp_mean`)**  
  - Mean: 15.24 °C  
  - Range: –8.0 to 25.9 °C  

  Values follow a seasonal pattern, with lower temperatures in winter months and higher in summer.

- **Wind speed (`wind_speed_mean`)**  
  - Mean: 2.99 m/s  
  - Range: 1.22 to 10.86 m/s  

- **Precipitation (`precip_mm`)**  
  - Most days have low or zero precipitation, with occasional heavy-rain days producing the right tail of the distribution.

- **Traffic volume (`traffic_volume`)**  
  - Mean: 25,047  
  - Range: 1,512 to 187,381  

The time series plot of `pm25` over 2023 shows day-to-day variability with some higher peaks, especially in certain periods, but no extreme outliers beyond generally plausible urban background levels.

### 2.2 Bivariate patterns

- A scatter plot of `pm25` vs `traffic_volume` indicates no obvious association by eye.  
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

Pearson correlation tests:

- Temp_mean: r = 0.213, p = 0.0052, n = 170  
- Wind_speed_mean: r = –0.220, p = 0.0040, n = 170  
- Precip_mm: r = –0.041, p = 0.596, n = 170  

Interpretation:

- Temperature shows a weak positive correlation with PM2.5 and the relationship is statistically significant at the 5% level, so H0 is rejected.  
- Wind speed shows a weak negative correlation with PM2.5 and the relationship is statistically significant, indicating higher wind speeds are associated with lower PM2.5 levels.  
- Precipitation does not show a statistically significant linear relationship with PM2.5 at the 5% level, so H0 is not rejected for precipitation.

---

### 3.2 H2 – High vs low traffic days

**Research question (RQ2):**  
Are daily PM2.5 levels significantly different on days with higher traffic volume compared to days with lower traffic volume?

**Hypotheses:**

- H0: Mean daily PM2.5 on high-traffic days equals mean daily PM2.5 on low-traffic days.  
- H1: Mean daily PM2.5 on high-traffic days is different from that on low-traffic days.

The variable `traffic_level` splits the days by the median `traffic_volume` into `high` and `low`.

- Number of high-traffic days: 85  
- Number of low-traffic days: 85  
- High-traffic mean PM2.5: 11.36 µg/m³  
- Low-traffic mean PM2.5: 11.06 µg/m³  

Statistical tests:

- Welch t-test: p = 0.882  
- Mann–Whitney U test: p = 0.734  

Interpretation:

The p-values from both tests are well above 0.05. Therefore, H0 is not rejected, and the data does not provide strong evidence of a difference in average PM2.5 between high-traffic and low-traffic days.

---

### 3.3 H3 – Weekday vs weekend

**Research question (RQ3):**  
Do daily PM2.5 levels differ between weekdays and weekends?

**Hypotheses:**

- H0: Mean daily PM2.5 on weekdays equals mean daily PM2.5 on weekends.  
- H1: Mean daily PM2.5 on weekdays differs from that on weekends.

From the analysis:

- Weekday mean PM2.5: 11.60 µg/m³  
- Weekend mean PM2.5: 10.21 µg/m³  

Statistical tests:

- Welch t-test: p = 0.5115  
- Mann–Whitney U test: p = 0.762  

Interpretation:

Both tests yield p-values greater than 0.05. Therefore, H0 is not rejected. Although weekdays show a slightly higher average PM2.5 concentration, the difference is not statistically significant at the 5% level.

---

## 4. Overall Summary

- The dataset combines EPA PM2.5, Meteostat weather data, and NYC traffic counts into a clean daily panel of 170 days for NYC in 2023.  
- PM2.5 values are within a plausible urban range, with some higher-pollution days but no extreme outliers.  
- Weather variables show weak relationships with PM2.5: temperature is weakly positively correlated, wind speed is weakly negatively correlated, and precipitation shows no significant relationship.  
- Comparing high vs low traffic days does not reveal a statistically significant difference in average PM2.5 levels.  
- Comparing weekdays to weekends shows slightly higher PM2.5 on weekdays, but the difference is not statistically significant.

This completes the Phase 2 requirements for exploratory data analysis and hypothesis testing.
