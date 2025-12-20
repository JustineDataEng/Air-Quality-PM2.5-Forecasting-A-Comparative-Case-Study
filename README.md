# Air Quality (PM2.5) Forecasting: A Comparative Case Study

## **Project Overview**
Predicting air quality is a complex task due to the high volatility of particulate matter (PM2.5). This project presents a technical comparison of three different architectural approaches to time-series forecasting. I developed three distinct pipelines—ranging from traditional machine learning to advanced statistical modeling—to determine which approach best captures the temporal dynamics of air pollution in Dar es Salaam.

---

## ** Technical Architectures**

### **1. LASSO Regression (Feature-Engineered Supervised Learning)**
* **Wrangling Strategy:** I transformed the time-series problem into a supervised learning task. I implemented an outlier filter (capped at 100 PM2.5) and engineered manual **"lag" features** by shifting the target variable to create feature columns.
* **Model Depth:** Applied **Polynomial Features** to capture non-linear interactions between lags, using **LASSO (L1 Regularization)** to prevent overfitting.
* **Key Insight:** This model excelled at identifying general trends but struggled with the specific sequence of temporal shocks.

### **2. Auto-Regressive (AR) Model**
* **Wrangling Strategy:** Shifted from feature-engineering to sequence-modeling. I used a **Grid Search** to optimize the "lag" parameter, identifying how many previous hours of data are statistically significant.
* **Validation:** Implemented a **Time-Series Split** to ensure no data leakage from the future into the past.

### **3. ARIMA Model (Advanced Time-Series)**
* **Wrangling Strategy:** Prepared the data for a `(p, d, q)` architecture. This required ensuring the data was **stationary** and accounting for moving averages of past errors.
* **Validation Technique:** Used **Walk-Forward Validation (WFV)**. Unlike a standard split, WFV retrains the model at each step, simulating a real-world production environment.
* **Result:** This was the top-performing model, proving that moving average components are vital for environmental forecasting.

---

## ** Performance Benchmarks**

| Model | MAE (Mean Absolute Error) | R² Score | Methodology |
| :--- | :--- | :--- | :--- |
| **Baseline** | 3.56 | N/A | Mean Prediction |
| **LASSO + Poly** | 1.12 | 0.75 | 80/20 X/y Split |
| **AR Model** | 0.98 | 0.74 | Grid Search Lags |
| **ARIMA (8,0,2)** | **0.85** | **0.79** | Walk-Forward Validation |

---

## ** Data Engineering Challenges Solved**
* **Timezone Localization:** Converted raw timestamps to `Africa/Dar_es_Salaam` to capture local peak traffic hours.
* **Non-Linearity:** Used **Polynomial expansion** to model the complex "synergy" effects in air pollution spikes.
* **Resampling:** Aggregated raw sensor data into 1-hour means to reduce noise while maintaining temporal patterns.



---

## ** Tech Stack**
* **Time Series:** `statsmodels` (ARIMA, AutoReg, ACF/PACF plots)
* **Machine Learning:** `scikit-learn` (LASSO, PolynomialFeatures, StandardScaler)
* **Visualization:** `plotly.express`, `seaborn`, `matplotlib`
* **Core:** `pandas`, `numpy`
