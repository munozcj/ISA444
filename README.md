**Team Members:** Bhargava, Harmon, and Munoz  
**Track:** Option 1 — Hotel Demand Forecasting  
**Forecast Horizon:** h = 28 days per property


## Executive Summary 

For our project, we tested five different ways of predicting hotel room demand to see which one is actually readily applicable for real-world data. We compared a plethora of different models, some being math-based and others being purely machine-learning-based. Our most significant finding was that AutoARIMA with on-the-books booking predictors was the strongest model overall, outperforming all 11 other models across accuracy metrics. This highlights that traditional statistical models with strong domain-specific predictors can outperform more complex neural and foundation models for hotel demand forecasting. 

# Hotel Demand Forecasting

## Overview
This project forecasts daily room demand for 17 hotel properties 
over a 28-day horizon using multiple forecasting approaches from 
the Nixtlaverse and TimeCopilot packages.

## Data
- Dataset: `sample_hotels-1.parquet`
- 17 hotel properties (hotel_77 and hotel_28 excluded as unreliable)
- Target: daily room demand (`y`)
- Forecast horizon: 28 days (4 weeks)

## Models Used
| Package | Models |
|---------|--------|
| StatsForecast | Naive, SeasonalNaive, AutoETS, AutoARIMA (with and without predictors) |
| MLForecast | LightGBM (LGBM), Random Forest (RF) |
| NeuralForecast | NBEATS, NHITS |
| TimeCopilot | Chronos, Moirai, TimesFM-2.5 |

## Evaluation Method
- 5-fold time series cross-validation (h=28, step_size=28)
- Metrics: ME (bias), MAE, RMSE, MAPE
- Final test on held-out last 28 days

## Results

### StatsForecast
**Best model: AutoARIMA_WithPred**
| Metric | AutoARIMA_WithPred | AutoARIMA_NoPred | AutoETS | SeasonalNaive | Naive |
|--------|-------------------|-----------------|---------|---------------|-------|
| MAE wins | 12 | 2 | 0 | 1 | 2 |
| RMSE wins | 12 | 2 | 2 | 0 | 1 |
| MAPE wins | 13 | 1 | 0 | 2 | 1 |
| Bias wins | 4 | 4 | 6 | 2 | 1 |

- AutoARIMA_WithPred dominated accuracy metrics across almost all hotels
- Adding otb_28 booking data and hotel type dummies made a significant difference
- AutoETS was the least biased model with 6/17 wins on ME

---

### MLForecast
**Best model: LGBM**
| Metric | LGBM | RF |
|--------|------|----|
| MAE wins | 12 | 5 |
| RMSE wins | 11 | 6 |
| MAPE wins | 11 | 6 |
| Bias wins | 7 | 10 |

- LGBM was the stronger model on accuracy metrics winning 12/17 on MAE
- RF was less biased overall with 10/17 bias wins
- Both models used otb_28 booking data and hotel type dummies as predictors

---

### NeuralForecast
**Best model: NBEATS**
| Metric | NBEATS | NHITS |
|--------|--------|-------|
| MAE wins | 10 | 7 |
| RMSE wins | 10 | 7 |
| MAPE wins | 9 | 8 |
| Bias wins | 6 | 11 |

- NBEATS edged out NHITS on accuracy metrics
- NHITS was less biased with 11/17 bias wins
- Both models used input_size=56 and horizon=28

---

### TimeCopilot
**Best model: TimesFM-2.5**
| Metric | TimesFM-2.5 | Chronos | Moirai |
|--------|-------------|---------|--------|
| MAE wins | 11 | 6 | 0 |
| RMSE wins | 9 | 7 | 1 |
| MAPE wins | 11 | 5 | 1 |
| Bias wins | 5 | 7 | 5 |

- TimesFM-2.5 dominated accuracy metrics across most hotels
- Chronos was the least biased model with 7/17 bias wins
- Moirai struggled overall, rarely competitive on any metric

---

## Key Findings
1. **AutoARIMA_WithPred** was the strongest statistical model — adding 
   on-the-books booking data (otb_28) and hotel characteristics as 
   predictors made a significant difference over all baseline models
2. **LGBM** outperformed Random Forest on accuracy metrics but RF 
   showed less bias overall
3. **NBEATS** was the stronger neural model on accuracy while NHITS 
   showed less bias
4. **TimesFM-2.5** was the strongest foundation model by a wide margin, 
   winning most hotels on MAE and MAPE
5. Hotels like hotel_7, hotel_35, hotel_42, and hotel_98 were harder 
   to forecast across ALL models, likely due to volatile or irregular 
   occupancy patterns
6. MAPE was unreliable for high-variance hotels — MAE and RMSE were 
   used as primary metrics for those series

## Summary of Findings

This project compared 12 forecasting models across four packages to predict 
daily room demand for 17 hotel properties over a 28-day horizon.

### Best Models by Package
- **StatsForecast**: AutoARIMA_WithPred was the clear winner, dominating with 
  12/17 wins on MAE and 13/17 on MAPE. The key driver was the inclusion of 
  on-the-books booking data (otb_28) and hotel type dummies as predictors. 
  Without these predictors, AutoARIMA performed significantly worse, highlighting 
  how much forward booking information matters for hotel demand forecasting.

- **MLForecast**: LGBM outperformed Random Forest on accuracy metrics, winning 
  12/17 hotels on MAE, 11/17 on RMSE and MAPE. RF was less biased overall 
  with 10/17 bias wins but fell behind on raw accuracy.

- **NeuralForecast**: NBEATS was the stronger model winning 10/17 hotels on 
  both MAE and RMSE. NHITS was less biased with 11/17 bias wins but could 
  not match NBEATS on accuracy.

- **TimeCopilot**: TimesFM-2.5 dominated with 11/17 wins on both MAE and MAPE 
  and 9/17 on RMSE. Chronos was the least biased with 7/17 ME wins. Moirai 
  struggled across all metrics and was rarely competitive.

### Overall Winner
AutoARIMA_WithPred was the strongest model overall, consistently outperforming 
all other models across accuracy metrics. This suggests that for hotel demand 
forecasting, traditional statistical models with strong domain-specific predictors 
can outperform more complex neural and foundation models.

### Performance Variation by Series
Model performance varied noticeably across hotels. Hotels like hotel_126, 
hotel_21, and hotel_133 were easy to forecast with low errors across all models, 
likely reflecting stable and predictable occupancy patterns. In contrast, 
hotel_7, hotel_35, hotel_42, and hotel_98 showed consistently high errors 
across all models, suggesting more volatile or irregular occupancy that is 
difficult to capture regardless of model complexity.

### Note on MAPE
MAPE was included for all series but should be interpreted cautiously for 
hotel_7, hotel_35, hotel_42, and hotel_98, which showed consistently high 
MAPE values across all models. This likely reflects periods of near-zero 
occupancy in these hotels, which inflates MAPE since it divides by the actual 
value. For these series, MAE and RMSE were used as the primary accuracy metrics.

## Forecast Plots

### Best Model Comparison (one per package)
Shows AutoARIMA_WithPred, LGBM, NBEATS, and TimesFM-2.5 vs actual demand for each hotel.

### Full Model Comparison (all models)
Shows all 12 models vs actual demand for each hotel.

## Outputs
- 📁 [Evaluation CSVs](outputs/)
- 📓 [Notebooks](notebooks/)
- 📊 [Forecast Plots](outputs/plots/)
