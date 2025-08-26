# Analysis and Prediction of Indoor Air Quality at Brindisi Airport

## Project Overview

This repository contains a comprehensive data analysis and machine learning project focused on monitoring and predicting the indoor air quality (IAQ) within the transit area of Brindisi Airport, Italy. The project utilizes a high-resolution dataset from three IoT sensor nodes collected over a one-year period (October 2022 - October 2023).

The workflow covers the entire data science pipeline:
*   **Data Cleaning and Preprocessing:** Handling missing values and removing outliers using IQR and node-specific thresholds.
*   **Feature Engineering:** Creating meaningful new features, including a calculated Air Quality Index (AQI), temporal variables (hour, month, season), and an activity-based `Holidays` flag.
*   **Data Enrichment:** Integrating daily flight traffic data from Eurocontrol to analyze the impact of airport operations on IAQ.
*   **Statistical Analysis:** Using non-parametric hypothesis tests (Kruskal-Wallis, Mann-Whitney U, Spearman's Correlation) to validate observations and draw statistically significant conclusions.
*   **Predictive Modeling:** Building and evaluating high-performance regression models (Decision Tree, Random Forest, XGBoost) to predict CO2 concentration and AQI.

---

## 📋 Table of Contents
*   [Dataset](#-dataset)
*   [Project Workflow](#-project-workflow)
*   [Key Statistical Findings](#-key-statistical-findings)
*   [Modeling Results](#-modeling-results)
*   [Technologies & Libraries Used](#-technologies--libraries-used)
*   [How to Run this Project](#-how-to-run-this-project)

---

## 📊 Dataset

The project uses two main data sources:

1.  **Sensor Data:**
    *   **Source:** [Davoli, L., Belli, L., & Ferrari, G. (2023). Air quality dataset from an indoor airport travelers transit area. Data in Brief, 109821.](https://doi.org/10.1016/j.dib.2023.109821)
    *   **Description:** A dataset of over 28 million records from three IoT nodes, capturing CO2, Particulate Matter (PM), temperature, humidity, and gas concentrations at a 2-second frequency.

2.  **Flight Traffic Data:**
    *   **Source:** [Eurocontrol. "Daily Traffic Variation - ACCs."](https://www.eurocontrol.int/Economics/DailyTrafficVariation-ACCs.html)
    *   **Description:** Daily flight statistics for the Brindisi ACC for 2022 and 2023, used to correlate air traffic volume with IAQ trends.

---

## ⚙️ Project Workflow

1.  **Data Integration:** Combined CSV data from the three IoT nodes and merged it with daily flight data from Eurocontrol based on the timestamp.
2.  **Data Cleaning:** Identified and removed rows with missing values. Applied a multi-stage outlier removal process using the IQR method followed by tailored, node-specific thresholds based on empirical observations.
3.  **Feature Engineering:**
    *   **`AQI`:** Calculated using the EPA standard formula for PM2.5 and PM10.
    *   **Temporal Features:** Extracted `Hour`, `Day_of_Week`, `Month`, and `Season`.
    *   **`Holidays`:** Engineered a binary feature for Italian public holidays and extended weekends.
    *   **Aggregate Metrics:** Created `PM_Total_Count` and `PM_Total_Mass` to simplify particulate data.
4.  **Data Sampling:** Employed stratified random sampling to create smaller, representative datasets (0.001% for statistical tests, 5% for modeling) that preserved the original data distribution across nodes.
5.  **Statistical & Machine Learning Analysis:** Conducted a series of statistical tests and developed predictive models as detailed below.

---

## 💡 Key Statistical Findings

The **Anderson-Darling test** confirmed that the key variables did not follow a normal distribution, justifying the use of non-parametric tests for hypothesis validation.

*   **Spatial Variations (Node Differences):**
    *   The **Kruskal-Wallis Test** revealed statistically significant differences (p < 0.05) in AQI, CO2, temperature, humidity, and particulate matter levels across the three sensor nodes. This confirms that sensor location is a critical factor influencing IAQ measurements.

*   **Temporal Trends (Time-Based Variations):**
    *   Significant fluctuations in air quality metrics were observed across different hours and months (Kruskal-Wallis, p < 0.05). CO2 levels, for example, peaked during high passenger traffic hours (10 AM - 8 PM).

*   **Holiday & Operational Impact:**
    *   The **Mann-Whitney U Test** showed that AQI, CO2, and daily flight counts were significantly different between holiday and non-holiday periods (p < 0.05), linking operational schedules to air quality changes.

*   **Correlations:**
    *   **Spearman's Correlation Test** identified significant monotonic relationships. Notably, strong positive correlations were found between `daily_flights` and both `temperature` (ρ = 0.713) and `humidity` (ρ = 0.630), highlighting the link between peak travel seasons and environmental conditions.

---

## 🚀 Modeling Results

Predictive models were trained to forecast CO2 concentration and the Air Quality Index (AQI). The models were evaluated using a **10-fold cross-validation** strategy to ensure robustness. The **Random Forest Regressor** consistently delivered the best performance for both tasks.

### Task 1: Predicting CO2 Concentration (`scd30_co2`)
The model demonstrated high accuracy in predicting CO2 levels based on environmental, temporal, and operational features.

| Algorithm         | Mean R² Score | Mean MSE   | Mean MAE  |
|-------------------|---------------|------------|-----------|
| Decision Tree     | 0.947         | 628.059    | 11.738    |
| **Random Forest** | **0.978**     | **257.675**| **9.309** |
| XGBoost           | 0.860         | 1658.694   | 29.145    |

### Task 2: Predicting Air Quality Index (`AQI`)
The model was highly effective at predicting the AQI, which is crucial for public health information.

| Algorithm         | Mean R² Score | Mean MSE  | Mean MAE  |
|-------------------|---------------|-----------|-----------|
| Decision Tree     | 0.907         | 8.587     | 1.876     |
| **Random Forest** | **0.954**     | **4.248** | **1.451** |
| XGBoost           | 0.784         | 19.903    | 3.328     |

---

## 🛠️ Technologies & Libraries Used

*   **Python 3**
*   **Pandas & NumPy:** For data manipulation and numerical operations.
*   **Matplotlib & Seaborn:** For data visualization.
*   **Scipy:** For statistical analysis.
*   **Scikit-learn:** For machine learning, preprocessing, and model evaluation.
*   **XGBoost:** For gradient boosting implementation.
