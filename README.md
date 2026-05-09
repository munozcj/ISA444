**Team Members:** Bhargava, Harmon, and Munoz  
**Track:** Option 1 — Hotel Demand Forecasting  
**Forecast Horizon:** h = 28 days per property


## Executive Summary 

For our project, we tested five different ways of prediction hotel room demand to see which one is actually readily applicable for real world data. We compared a plethora of different models, some being math based and others being purely machine-learning based. Our most significant finding was that the TimeCoPilot & LightGBM frameworks were the most accurate out of all of the models we tested. 

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

## Outputs
- 📁 [Evaluation CSVs](outputs/)
- 📓 [Notebooks](notebooks/)
- 📊 [Forecast Plots](plots/)
