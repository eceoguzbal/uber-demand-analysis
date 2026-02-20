# Uber Demand & Fleet Efficiency Analysis (Jan–Feb)

This project analyzes Uber operational data to understand **demand patterns** and **fleet efficiency**, and builds a **time-series forecasting model** to predict daily trip volume. The focus is on producing actionable business insights and a reproducible machine learning pipeline.

## Project Goals
- Explore daily demand trends and weekly patterns
- Compare operational performance across dispatch bases
- Derive fleet efficiency metrics (e.g., *trips per active vehicle*)
- Forecast daily trips using machine learning with temporal feature engineering

## Dataset
**File:** `Uber-Jan-Feb-FOIL.csv`  
**Columns:**
- `dispatching_base_number`: Uber dispatch base identifier (e.g., B02512)
- `date`: daily date
- `active_vehicles`: number of active vehicles for that base and day
- `trips`: number of trips for that base and day

**Scope:**
- 6 dispatch bases
- 59 days per base (balanced dataset)

## Key Analyses
### 1) Demand Patterns
- Daily trip volume shows an overall upward trend during the observed period
- Weekend demand is higher than weekdays
- Saturday is the peak day in weekly demand distribution

### 2) Fleet Efficiency
We computed:
- `trips_per_vehicle = trips / active_vehicles`

Findings:
- **B02682** achieves the highest average trips per vehicle (best operational efficiency)
- Efficiency shows diminishing returns at very high fleet sizes (potential oversupply effect)

## Machine Learning: Daily Trips Forecasting
We framed the problem as a **time-series regression** task: predict total daily trips.

### Baseline Model
- **Linear Regression** using lag features (e.g., lag1, lag7)
- Result: MAE ≈ **7,555**, RMSE ≈ **9,908**

### Improved Model (with Feature Engineering)
We engineered temporal features:
- Lag features: `lag1`, `lag2`, `lag3`, `lag7`
- Rolling statistics: `rolling_mean_3`, `rolling_mean_7`
- Trend index: `trend`
- Weekly signal: `weekday`

Model:
- **Random Forest Regressor**
- Result: MAE ≈ **5,702**, RMSE ≈ **8,899**
- ~**25% improvement** in MAE compared to baseline

### Model Interpretation
Feature importance indicates that demand is strongly influenced by:
- Previous day demand (`lag1`)
- Smoothed weekly demand (`rolling_mean_7`)
- Weekly recurrence (`lag7`)

## Business Recommendations
- Increase driver/fleet availability on weekends (especially Saturdays)
- Monitor base-level efficiency; replicate best practices from high-efficiency bases (e.g., **B02682**)
- Avoid oversupplying vehicles beyond optimal utilization thresholds
- Use the demand forecast to support staffing and capacity planning

## Project Structure
