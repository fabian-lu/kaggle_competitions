# Kaggle Competitions

This repository contains my work on various Kaggle competitions. I recently started participating in Kaggle competitions and plan to continuously add more challenges as I progress in my data science journey.

---

## Competitions

### 1. MABe Challenge - Social Action Recognition in Mice

**Status:** 🚧 Ongoing

#### Competition Overview
Identify 30+ social and non-social behaviors in groups of mice from pose tracking data. Must predict behavioral events (agent, target, action, start/stop frames) from x,y body part coordinates over time.

#### Visualization
![Mouse Behavior Animation](MABE-Social_Action_Recognition_in_Mice/animations/mice_behavior_10%20000_frames.gif)

*Animation showing mouse positions, orientations, and bounding boxes over 1000 frames*

### Setup & Understanding
- Downloaded competition data (2.7GB training tracking, 3.5MB annotations)
- Analyzed data structure: 863 labeled videos + 7,926 unlabeled videos (MABe22)
- Identified 19 different labs with varying tracking equipment and body parts

#### EDA (In Progress)
- Loaded and explored metadata (train.csv: 8,789 videos)
- Implemented direction calculation (tail→nose angle) for each mouse
- Started visualization of mouse positions and orientations in arena

#### Data Structure
- `train.csv` - video metadata (lab, fps, duration, arena specs, behaviors)
- `train_tracking/{lab}/{video_id}.parquet` - frame-by-frame x,y coordinates (features)
- `train_annotation/{lab}/{video_id}.parquet` - behavioral events with start/stop frames (targets)

#### Next Steps
- Complete EDA: behavior distributions, temporal patterns, trajectory analysis
- Feature engineering: distances, velocities, relative angles, interactions
- Baseline model with gradient boosting (XGBoost/LightGBM)
- Explore unlabeled data for pre-training

---

### 2. China Real Estate Demand Prediction

**Status**: Completed
**Competition Link**: [Real Estate Demand Prediction](https://www.kaggle.com/competitions/china-real-estate-demand-prediction)
**Goal**: Predict monthly residential demand in China's real estate market

#### Approach

**Models & Techniques**:
- **Multi-model ensemble** combining XGBoost, LightGBM, and CatBoost
- **Custom asymmetric loss functions** to penalize over-predictions more heavily than under-predictions
  - Adaptive penalties based on error magnitude (10x penalty for >50% over-prediction)
- **Hyperparameter optimization** using Optuna (Bayesian optimization)
- **Ensemble weight optimization** through grid search on validation set

**Feature Engineering**:
- Time-based features (year, month, quarter, cyclic encodings)
- Lag features (1, 2, 3, 4, 5, 6, 7, 9, 12, 18, 24 months)
- Rolling statistics (mean, std) over 3, 6, 12-month windows
- Exponential moving averages (EMA) with spans of 3, 6, 12 months
- Momentum and acceleration indicators
- Pre-owned house market dynamics
- Land transaction features
- Population and POI (Point of Interest) density metrics
- City-level economic indicators (GDP, employment, retail sales)
- Search volume aggregations

**Data Strategy**:
- Dense grid construction (filled missing month-sector combinations with zeros)
- Sector clustering using K-Means on POI features
- Time-based train/validation split (last 6 months for validation)
- Focus on recent 24 months to capture current market regime
- Iterative forecasting (predict one month ahead at a time, using previous predictions as features)

**Post-processing**:
- Sector-wise caps based on recent historical maxima
- Zero-history heuristic (sectors with no recent activity set to 0)
- Manual sector zeroing for consistently inactive sectors

**Results**:
- Validation score: 0.5235 (competition metric)
- Ensemble weights: LightGBM 70%, CatBoost 30%, XGBoost 0%
- Successfully handled the competition's two-stage evaluation metric

**Key Learnings**:
- Avoiding target leakage is critical in time series forecasting
- Custom loss functions can significantly improve performance for domain-specific metrics
- Iterative forecasting with feedback loops provides more realistic predictions
- Bayesian optimization (Optuna) is more efficient than grid search
- Ensemble diversity matters more than individual model complexity

#### Files
- `model_ensemble_enhanced.ipynb` - Final solution with custom losses and Bayesian optimization
- `model_ensemble.ipynb` - Earlier ensemble approach
- `submission.csv` - Final predictions submitted to Kaggle

---

