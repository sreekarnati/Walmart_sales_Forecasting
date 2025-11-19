# 🛒 Walmart Weekly Sales Forecasting & Professional EDA

This repository contains a comprehensive analysis and time-series forecasting model for Walmart's weekly sales data, focusing on identifying key seasonal drivers and providing actionable, granular predictions for inventory management.

The analysis utilizes data from 45 stores across a 143-week period, incorporating macroeconomic factors (CPI, unemployment, fuel price) and holiday events.

---

## 🎯 Project Goal

The primary goal of this project is to build a robust forecasting pipeline capable of predicting **Weekly Sales** at the **Store and Department level**, moving beyond aggregated, high-level forecasts.

---

## 🛠️ Methodology & Tools

* **Language:** Python
* **Core Libraries:** Pandas, NumPy, Matplotlib, Seaborn
* **Modeling:** **Facebook Prophet** (chosen for its strong performance in handling retail seasonality and holidays).
* **Output:** Jupyter Notebook (`Walmart_Final_Analysis.ipynb`)

### Data Engineering & Preprocessing

1.  **Data Merging:** Combined `train.csv`, `stores.csv`, and `features.csv` into a master DataFrame.
2.  **Imputation:** Missing macroeconomic data (`CPI`, `Unemployment`, `Fuel Price`) was handled using **Store-Level Forward/Backward Filling** to preserve local trends, followed by global median imputation.
3.  **Feature Creation:** Engineered temporal features (`Year`, `Month`, `Week`) and created **Lag Features** (sales from 1 and 52 weeks ago) to capture autocorrelation for future model expansion.

---

## 📊 Key Analytical Insights

The Exploratory Data Analysis (EDA) revealed several critical findings that informed the modeling strategy:

| Insight | Finding | Impact on Forecasting |
| :--- | :--- | :--- |
| **Seasonality** | Massive, rigid, and recurring sales spikes were observed in **November (Thanksgiving) & December (Christmas)**. | Seasonal components in Prophet were crucial; models must account for holiday flags. |
| **External Regressors** | **Fuel Price, CPI, and Unemployment** showed negligible correlation (near 0) with `Weekly_Sales`. | These features were deemed non-predictive for this dataset and excluded from the core Prophet model. |
| **Data Quality** | **Negative `Weekly_Sales`** values were present, indicating product returns. These were retained to ensure the integrity of total revenue tracking. | Outlier handling should be targeted at returns rather than revenue spikes. |

---

## 🔮 Forecasting & Scaling

The project moves from a simple aggregated baseline to a scalable, operational solution:

1.  **Baseline Forecast (Aggregated):** An initial Prophet model was trained on total company sales to confirm global seasonality and trend components.
2.  **Granular Forecasting Loop:** The final pipeline implements a Python loop to automatically:
    * Identify the top **Store + Department** combinations by revenue.
    * Train a separate, optimized Prophet model for each combination.
    * Generate a 26-week (6-month) prediction with confidence intervals.

This approach provides **actionable forecasts** that can directly feed into inventory and logistics systems for specific store locations.

---

## 💡 Recommendations for the Business

1.  **Prioritize Granularity:** Use the **per-store, per-department predictions** for stock allocation and shelf space planning, as global averages mask essential store-level variance.
2.  **Strategic Promotions:** Align marketing and promotions to **low-demand weeks** (e.g., late January/early February) to smooth demand and optimize warehouse capacity utilization.
3.  **Flag Outliers:** Sales outliers (which often correspond to holiday events) should be flagged and reviewed, but not removed, as they are crucial for accurate seasonal models.

---

## 🚀 Next Steps

* **Model Expansion:** Integrate the lag features into an advanced machine learning model (e.g., **LightGBM**) to capture non-linear relationships and compare performance against the Prophet baseline.
* **Deployment:** Build an automated ETL pipeline to **retrain the models weekly** and export the predictions as a CSV/JSON file to a cloud storage (S3) or database.
* **Visualization:** Create a user-friendly **Power BI or Tableau dashboard** connected to the prediction output, allowing business users (Store Managers, Supply Chain Analysts) to easily visualize their specific forecasts.
