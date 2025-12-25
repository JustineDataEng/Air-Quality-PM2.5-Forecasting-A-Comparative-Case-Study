# Air Quality (PM2.5) Forecasting: A Comparative Case Study

## **Project Overview**
Predicting air quality is a high-stakes challenge due to the volatility of particulate matter. This project evaluates three architectural approaches to time-series forecasting using sensor data from Dar es Salaam, Tanzania. I developed three distinct pipelines, ranging from regularized machine learning to advanced statistical models—to identify which approach best captures atmospheric pollution dynamics.

---

## **Technical Architectures**

### **1. LASSO Regression (L1-Regularized Learning)**
* **Wrangling Strategy:** I transformed the time-series problem into a **supervised learning task** by creating 26 hourly "lag" features. To ensure data quality, I implemented an outlier filter capping PM2.5 readings at 100.
* **Model Depth:** I applied **Polynomial Features (Degree 2)** to model non-linear interactions between lags, using **LASSO (L1 Regularization)** for automated feature selection and to prevent overfitting.
* **Key Insight:** This model was effective at identifying general trends but struggled with the specific sequence of temporal shocks, leading to a Test MAE of 1.76.

### **2. SARIMAX Model (Seasonal Sequence Modeling)**
* **Architecture:** Configured with $(p, d, q) = (26, 0, 1)$ to account for seasonal auto-regressive components.
* **Validation Technique:** I used **Walk-Forward Validation (WFV)** to simulate a real-world production environment, predicting one step ahead before incorporating the actual observed value.
* **Key Insight:** While the training error was low (0.96), the jump to a Test MAE of 1.81 indicated significant overfitting to the training set.

### **3. ARIMA Model (Advanced Statistical Forecasting)**
* **Wrangling Strategy:** I optimized the architecture via **Grid Search** to an $(8, 0, 2)$ configuration. This required ensuring data stationarity and accounting for moving averages of past forecast errors.
* **Validation Technique:** Utilized **Walk-Forward Validation (WFV)**, retraining the model at each hourly step to incorporate the latest error data.
* **Result:** This was the **top-performing model**, maintaining the most robust balance between training fit and unseen data accuracy.

---

## **Performance Benchmarks**

| Model | Training MAE | Test MAE | $R^2$ Score | Methodology |
| :--- | :--- | :--- | :--- | :--- |
| **Baseline** | 3.80 | 3.80 | N/A | Mean Prediction |
| **ARIMA (8,0,2)** | 1.08 | **0.85** | **0.79** | Walk-Forward |
| **LASSO + Poly** | 0.98 | 1.76 | 0.75 | 80/20 Split |
| **SARIMAX (26,0,1)** | **0.96** | 1.81 | 0.75 | Walk-Forward |

---

## **Data Engineering Challenges Solved**
* **Timezone Localization:** Converted raw UTC timestamps to `Africa/Dar_es_Salaam` to align pollution spikes with local peak traffic cycles.
* **Missing Data Handling:** Resampled raw sensor data into **1-hour means** and used forward-filling to manage gaps without losing temporal context.
* **Feature Engineering:** Modeled complex "synergy" effects by generating 26-hour lagged variables and second-degree polynomial expansions.

---

## **Tech Stack**
* **Time Series:** `statsmodels` (ARIMA, SARIMAX, ACF/PACF plots, ADFuller test)
* **Machine Learning:** `scikit-learn` (LASSO, PolynomialFeatures, StandardScaler)
* **Visualization:** `plotly.express`, `seaborn`, `matplotlib`
* **Core & Deployment:** `pandas`, `numpy`, `joblib` (for exporting `best_model.pkl`)
