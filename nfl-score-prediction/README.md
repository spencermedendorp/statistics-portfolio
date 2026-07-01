# NFL Score Prediction Model

## Overview
This project builds a regression-based model to predict NFL team points scored in a given game using weekly offensive and defensive statistics. The goal was to identify which on-field factors most reliably predict scoring output and evaluate model performance on unseen data.

---

## Data
- **Source:** Kaggle — Weekly NFL Team Offensive & Defensive Stats
- **Coverage:** 2012–present, regular season games
- **Features:** Weekly team-level offensive stats (passing yards, rushing yards, third down conversion rate, turnovers, etc.) merged with opponent defensive stats for the same game
- **Target Variable:** Points scored by the offensive team

---

## Methodology

### Train / Test Split
- **Training set:** Weeks 1–12 of each season (early-to-mid season data)
- **Test set:** Weeks 13+ (late season games the model had not seen)

This simulates a real-world forecasting scenario where you predict late-season performance using patterns learned earlier in the year.

### Modeling Approach

**Step 1 — Full Linear Regression**  
Built an initial OLS regression using all available features after removing redundant columns (touchdowns, field goals, averages) to avoid data leakage and multicollinearity.

**Step 2 — Stepwise Selection**  
Applied bidirectional stepwise selection (AIC-based) to reduce the feature set to only statistically meaningful predictors.

**Step 3 — LASSO Regularization**  
Used cross-validated LASSO (glmnet) to further identify the most important predictors, shrinking irrelevant coefficients to zero. The selected variables were used to fit a final clean linear model.

**Step 4 — Diagnostic Testing**  
Ran a full suite of regression diagnostics on the final model:
- Shapiro-Wilk test (normality of residuals)
- Breusch-Pagan test (heteroskedasticity)
- Durbin-Watson test (autocorrelation)
- Variance Inflation Factor / VIF (multicollinearity)
- Cook's Distance and Influence Plot (outlier detection)
- RESET test (model specification)
- AIC / BIC (model comparison)

---

## Evaluation
- **Metric:** Mean Absolute Error (MAE) on the held-out test set (weeks 13+)
- **Visualization:** Actual vs. Predicted scatter plot with a reference line

---

## Tools & Libraries
| Tool | Purpose |
|------|---------|
| R | Primary language |
| dplyr | Data cleaning and feature selection |
| glmnet | LASSO regression and cross-validation |
| lmtest | Breusch-Pagan and Durbin-Watson tests |
| car | VIF and outlier diagnostics |
| ggplot2 | Actual vs. Predicted visualization |

---

## Key Takeaways
- LASSO regularization meaningfully reduced model complexity while maintaining predictive accuracy
- Passing yards, third down conversion rate, and opponent defensive pressure were among the strongest predictors of scoring output
- Late-season games showed slightly higher prediction error, likely due to teams resting starters or adjusting playcalling
