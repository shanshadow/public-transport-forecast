-----

# 📈 Public Transport Ridership Forecasting Project: Post-COVID Recovery Analysis

## 🎯 Project Thesis & Executive Summary

This project addresses the challenge of forecasting **multivariate time series** data in a post-pandemic environment. We utilized a **Machine Learning (ML) approach** (LightGBM) to forecast daily passenger journeys across five key public transport service types for the next 7 days.

The core objective was to build a robust model that can explicitly capture **complex, non-linear exogenous factors** (like weekends, school holidays, and public holidays) alongside core time series patterns (lags and seasonality).

| Metric | Target | Result |
| :--- | :--- | :--- |
| **Model Type** | Multi-Target Regression | 5 Independent **LightGBM Regressors** |
| **Validation** | Time Series Split (90 Days) | Achieved high accuracy (Low RMSE) across all service types. |
| **Key Insight** | Weekly Seasonality & Exogenous Shocks | **Lag 7** and **is\_school\_day** were the highest importance features. |

-----

## ✨ Technical Architecture & Methodology

### 1\. Repository Structure

The project follows a standard, reproducible data science structure.

```
public-transport-forecast/
│
├── data/
│   └── Daily_Public_Transport_Passenger_Journeys.csv  (Source Data)
│
├── notebooks/
│   ├── 01_Data_Exploration_and_Insights.ipynb        (EDA & Feature Motivation)
│   └── 02_Forecasting_Model.ipynb                   (ML Pipeline, Training, & Forecasting)
│
├── reports/
│   ├── insights_report.md                           (Detailed Analysis Report)
│   └── forecasting_technical_report.md              (Algorithm Rationale & Parameters)
│
├── requirements.txt                                 (Dependency Management)
└── README.md
```

### 2\. Forecasting Engine: Decoupled LightGBM

We selected **Light Gradient Boosting Machine (LightGBM)** for its speed and ability to handle high-dimensional, non-linear feature sets. A decoupled strategy was implemented:

  * **Five independent models** were trained, allowing each regressor to optimize its parameters and feature importance specifically for the unique patterns of its respective service type (`School` vs. `Local Route`).
  * **Validation:** A 90-day time-series split was used to simulate a production "walk-forward" scenario, ensuring realistic performance assessment.

-----

## 📊 Key Findings & Insights (Task 1)

These insights were critical in informing our feature engineering strategy, transforming the raw time series data into highly predictive features.

| Insight | Technical Impact | Service Examples |
| :--- | :--- | :--- |
| **Structural Break (COVID-19)** | Requires explicit time features (`year`, `month`) to model the long-term trend change and recovery path post-2020. | Total Ridership dropped \>80% overnight. |
| **Dominant Weekly Seasonality** | **7-Day Lag features** are mandatory; these consistently ranked as the **highest importance features**. | All services show a profound drop on Saturday/Sunday. |
| **Niche Service Specialization** | Validated the need for the binary **`is_school_day`** and **`is_holiday`** exogenous features. | `Peak Service` and `School` services operate near-zero outside of standard weekdays. |
| **Service Hierarchy** | `Rapid Route` and `Local Route` account for over **70% of total volume**, requiring the most careful attention to model error (RMSE). | - |

-----

## 📈 7-Day Production Forecast (Task 2 & Output)

The forecast successfully captures the expected sharp drop in ridership over the weekend (Oct 5th/6th), demonstrating the effectiveness of the engineered holiday and day-of-week features.

### Forecast Table (Structured Output)

| Service Type | 2024-09-30 | 2024-10-01 | 2024-10-02 | 2024-10-03 | 2024-10-04 | **2024-10-05** | **2024-10-06** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Local Route | 16,850 | 17,100 | 17,050 | 16,900 | 16,500 | 4,500 | 2,800 |
| Light Rail | 11,020 | 11,200 | 11,150 | 11,100 | 10,800 | 6,100 | 4,600 |
| Peak Service | 345 | 350 | 348 | 342 | 310 | **0** | **0** |
| Rapid Route | 20,110 | 20,500 | 20,450 | 20,300 | 19,500 | 8,300 | 6,500 |
| School | 4,850 | 4,900 | 4,870 | 4,820 | 4,600 | 5 | **0** |

### Forecast Visualization (Diagram)

This chart shows the continuity of the forecast against the last 60 days of historical data.

### Feature Importance (Interpretability Diagram)

The feature importance analysis validates that the model relies heavily on our engineered features, demonstrating model interpretability.

-----

## 🛠️ How to Run & Reproduce

### Dependencies

All necessary packages are listed in `requirements.txt`.

### Steps

1.  **Clone the Repository:**

    ```bash
    git clone [" https://github.com/shanshadow/public-transport-forecast "]
    cd [ public-transport-forecast ]
    ```

2.  **Setup Environment (Recommended):**

    ```bash
    python -m venv venv
    source venv/bin/activate  # Windows: venv\Scripts\activate
    ```

3.  **Install Dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4.  **Execute the Analysis:**

    ```bash
    jupyter notebook
    ```

      * Run `notebooks/01_Data_Exploration_and_Insights.ipynb` for EDA.
      * Run `notebooks/02_Forecasting_Model.ipynb` to train the LightGBM models, generate the forecast, and produce all feature importance plots and final report tables.
