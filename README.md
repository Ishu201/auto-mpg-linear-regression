# Automobile Fuel Efficiency: Linear Regression Analysis

Predicting miles per gallon (MPG) from vehicle design attributes using
linear regression with backward elimination and VIF-based
multicollinearity diagnostics.

Built as part of ALY 6020: Predictive Analytics at Northeastern University.

---

## Business Context

A car manufacturer known for producing large automobiles commissioned
this analysis to identify which design attributes most strongly predict
fuel efficiency. The goal was to produce a quantitative model that
engineers can use to estimate MPG for proposed configurations before
committing to a prototype.

---

## Dataset

**Source:** [UCI Auto MPG Dataset](https://archive.ics.uci.edu/ml/datasets/auto+mpg)  
**Records:** 398 vehicles from model years 1970 to 1982  
**Features:** Cylinders, Displacement, Horsepower, Weight, Acceleration,
Model Year, US Made (binary)  
**Target:** Miles per gallon (MPG)

> Download the dataset from the UCI link above and place `auto-mpg.data`
> in a `/data` folder before running the notebook.

---

## Methodology

### Part 1: Data Cleansing
- Detected non-numeric placeholders (`?`) in Horsepower using
  `pd.to_numeric(errors='coerce')`
- Imputed 6 missing Horsepower values with the median (93.5) to avoid
  distortion from right skew
- Confirmed zero duplicates and validated all variable ranges against
  known 1970s vehicle specifications
- Retained outliers (high-MPG compacts from the oil crisis era) as
  legitimate real-world observations
- Noted right skew in MPG, Horsepower, Displacement, and Weight;
  bimodal Cylinders distribution (4 and 8)

### Part 2: Full Model
- All seven predictors included: Cylinders, Displacement, Horsepower,
  Weight, Acceleration, Model Year, US Made
- 80/20 train/test split (random_state=42)
- P-values computed manually via t-distribution from residual variance
  and design matrix

### Part 3: Optimization
- VIF analysis revealed severe multicollinearity: Weight (131.5),
  Cylinders (119.5), Displacement (101.1), Horsepower (61.6)
- Backward elimination iteratively removed Cylinders, Acceleration,
  Horsepower, and Displacement
- Final model retained Weight, Model Year, and US Made

---

## Results

| Metric | Full Model (7 predictors) | Optimized Model (3 predictors) |
|---|---|---|
| R-Squared | 0.8463 | 0.8388 |
| Adjusted R-Squared | 0.8313 | 0.8324 |
| RMSE | 2.8749 | 2.9441 |
| MAE | 2.2620 | 2.2943 |

The optimized model preserved nearly identical accuracy with four fewer
predictors. The adjusted R-squared actually improved slightly,
confirming the removed variables were not contributing independently.

---

## Key Findings

- Weight was the dominant predictor (coefficient: -0.006 per pound).
  A 500-pound reduction yields approximately 3 additional MPG
- Model Year coefficient of +0.807 reflects cumulative engineering
  gains from fuel injection and aerodynamic improvements
- US-made vehicles achieved approximately 2.2 fewer MPG than foreign
  equivalents in this period
- Cylinders, Displacement, and Horsepower were eliminated due to
  multicollinearity: their effects are captured through Weight

**Practical demonstration:** A 1970 US vehicle at 4,000 lbs predicts
11.70 MPG. A redesigned 1982-equivalent at 2,800 lbs with foreign
platform standards predicts 30.70 MPG, a gain of 19 MPG.

---

## Tech Stack

- Python 3
- pandas, NumPy
- scikit-learn
- matplotlib, seaborn
- statsmodels

---

## Files

| File | Description |
|---|---|
| `auto_mpg_regression.ipynb` | Full analysis notebook |
| `Isuri_Module2_Regression_Report.pdf` | Written report with interpretation |

---

## Author

**Isuri Nawodya**  
Master of Professional Studies in Analytics, Northeastern University Vancouver  
