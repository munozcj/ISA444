# 🏨 ISA 444: Hotel Demand Forecasting
**Farmer School of Business | Miami University**

> Comparing traditional, ML, neural, and foundation model approaches for forecasting daily room demand across 17 hotel properties.

---

## 📋 Project Overview

This project forecasts **daily room demand** for the next 4 weeks (h = 28) across 17 hotel properties using the `sample_hotels.parquet` dataset. We compare a full suite of forecasting models using rigorous 5-fold time-series cross-validation and report standardized evaluation metrics.

**Team Members:** Bhargava, Harmon, and Munoz  
**Track:** Option 1 — Hotel Demand Forecasting  
**Forecast Horizon:** h = 28 days per property


## Executive Summary 

For our project, we tested five different ways of prediction hotel room demand to see which one is actually readily applicable for real world data. We compared a plethora of different models, some being math based and others being purely machine-learning based. Our most significant finding was that the TimeCoPilot & LightGBM frameworks were the most accurate out of all of the models we tested. 

---

## 📁 Repository Structure

```
isa444-hotel-forecasting/
│
├── notebooks/
│   └── hotel_forecasting.ipynb       # Full modeling pipeline
│
├── data/
│   └── sample_hotels.parquet         # Source data (17 hotel properties)
│
├── outputs/
│   ├── cv_results.csv                # Cross-validation metrics by series & model
│   ├── cv_summary_wins.csv           # Win counts per model per metric
│   ├── test_forecasts.csv            # Final test forecasts for all properties
│   └── plots/
│       ├── hotel_01_forecast.png
│       ├── hotel_02_forecast.png
│       └── ...                       # One plot per property
│
└── README.md
```

---

## 🤖 Models Compared

| Category | Model |
|---|---|
| **Baseline** | Naive, Seasonal Naive |
| **Statistical** | AutoETS, AutoARIMA (StatsForecast) |
| **ML** | LightGBM (MLForecast) |
| **Neural** | AutoNBEATS, AutoNHITS (NeuralForecast) |
| **Foundation** | Chronos (TimeCoPilot) |

---

## 📊 Evaluation Methodology

- **Cross-validation:** 5-fold time-series CV (non-overlapping windows)
- **Metrics reported per series and per model:**
  - ME (Mean Error — bias)
  - MAE (Mean Absolute Error)
  - RMSE (Root Mean Squared Error)
  - MAPE (Mean Absolute Percentage Error, where applicable)
- **Win counting:** A model "wins" on a series if it achieves the lowest value for a given metric on that series.

> ⚠️ **MAPE Note:** MAPE is excluded for series with near-zero demand values to avoid division instability. MAE and RMSE are the primary metrics for those properties.

---

## 📈 Key Results

### Cross-Validation Summary (CV MAE — lower is better)

| Model | Avg MAE | Avg RMSE | Wins (MAE) |
|---|---|---|---|
| Naive | — | — | — |
| Seasonal Naive | — | — | — |
| AutoETS | — | — | — |
| AutoARIMA | — | — | — |
| LightGBM | — | — | — |
| AutoNBEATS | — | — | — |
| AutoNHITS | — | — | — |
| Chronos | — | — | — |

*Fill in after running the notebook.*

### Findings

- **Best overall model:** [Model name] — consistently lowest MAE across the majority of properties.
- **Seasonal patterns:** Seasonal Naive performs well on properties with strong weekly seasonality, outperforming more complex models on [N] series.
- **Foundation model:** Chronos achieved competitive accuracy without any training, particularly on [describe pattern].
- **Where ML excels:** LightGBM benefited from longer training histories and performed best on [describe].

---

## 🗺️ Forecast Plots

Below are sample forecast plots (actuals vs. predicted) for selected properties. Full plots for all 17 hotels are in [`outputs/plots/`](outputs/plots/).

| Property | Plot |
|---|---|
| Hotel 01 | ![Hotel 01](outputs/plots/hotel_01_forecast.png) |
| Hotel 02 | ![Hotel 02](outputs/plots/hotel_02_forecast.png) |

---

## ▶️ Reproducing This Project

### 1. Clone the repository
```bash
git clone https://github.com/[your-username]/isa444-hotel-forecasting.git
cd isa444-hotel-forecasting
```

### 2. Install dependencies
```bash
pip install statsforecast mlforecast neuralforecast utilsforecast pandas pyarrow matplotlib
pip install git+https://github.com/time-series-foundation-models/timecopi lot  # or per TimeCoPilot docs
```

### 3. Run the notebook
Open `notebooks/hotel_forecasting.ipynb` in Jupyter, JupyterLab, or [DeepNote](https://deepnote.com) and run all cells top to bottom.

All outputs (CSVs and plots) will be saved automatically to the `outputs/` folder.

---

## 📦 Dependencies

```
statsforecast
mlforecast
neuralforecast
utilsforecast
pandas
pyarrow
matplotlib
lightgbm
```
