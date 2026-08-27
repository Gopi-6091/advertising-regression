# Advertising Regression — Data Science Portfolio Project

## 🎯 Project Goal

Build a portfolio-quality data science project using linear regression to understand the relationship between advertising expenditure and product sales.

The goal is not simply to build a model.

The goal is to demonstrate the complete data-science thinking process:

**Business problem → Data understanding → Data quality → Cleaning → EDA → Hypotheses → Modeling → Evaluation → Diagnostics → Business insight → Limitations**

---

# 1. Business Problem

A company invests in three advertising channels:

- TV
- Radio
- Newspaper

### Business objective

Understand how advertising expenditure is associated with sales and use the findings to support better advertising-budget decisions.

### ML objective

Predict sales using advertising expenditure.

### Target

`sales`

### Predictors

- `TV`
- `radio`
- `newspaper`

### Important distinction

**Prediction ≠ correlation ≠ causation ≠ optimization**

This is observational data.

Regression can identify associations and make predictions, but it does not automatically prove that increasing advertising expenditure causes sales to increase.

A predictive model also does not automatically determine the optimal advertising budget.

---

# 2. Dataset

Dataset: `Advertising.csv`

Source: ISLR — An Introduction to Statistical Learning

Official resource:

https://trevorhastie.github.io/ISLR/data.html

The dataset contains 200 observations across different markets.

The observations are not sequential time measurements, so this is framed as a **cross-sectional regression problem**, not a time-series forecasting problem.

### Columns

| Column | Role | Meaning |
|---|---|---|
| `Unnamed: 0` | Identifier | Observation/index identifier |
| `TV` | Predictor | TV advertising budget |
| `radio` | Predictor | Radio advertising budget |
| `newspaper` | Predictor | Newspaper advertising budget |
| `sales` | Target | Product sales |

The exact units of the advertising budgets should be documented from the original dataset source rather than assumed.

---

# 3. Environment

- VS Code
- GitHub Codespaces
- Python 3.12.1
- `.venv` virtual environment
- Jupyter Notebook

### Dependencies

- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- statsmodels
- jupyter
- ipykernel

Dependencies are recorded in `requirements.txt`.

`.venv/` is excluded from Git.

---

# 4. Project Structure

```text
advertising-regression/
│
├── data/
│   └── Advertising.csv
│
├── notebooks/
│   └── 01_data_understanding.ipynb
│
├── src/
│
├── README.md
├── requirements.txt
├── .gitignore
└── .venv/


.venv/
__pycache__/
.ipynb_checkpoints/

5. Data Audit & Cleaning
Dataset Structure

Confirmed:

    200 rows
    5 columns
    4 numerical business variables:
        TV
        radio
        newspaper
        sales
    1 identifier-like column:
        Unnamed: 0

All columns contain 200 non-null values.
Data Quality Checks

Completed:

    Inspected first five rows
    Checked dataset shape
    Checked column names
    Checked data types
    Checked df.info()
    Checked missing values
    Checked duplicate rows
    Checked uniqueness
    Inspected descriptive statistics
    Checked for obvious negative values
    Checked original dataset documentation

Findings

    No missing values detected
    No duplicate rows detected
    No obvious data-entry errors found
    Advertising variables represent advertising budgets
    sales represents product sales
    Observations represent different markets

Identifier Assessment

Unnamed: 0 was identified as an observation/index identifier rather than a meaningful business predictor.

Decision:

    Exclude Unnamed: 0 from regression features
    Keep the raw CSV unchanged
    Do not modify or remove observations

Cleaning Principle

Observe → Investigate → Reason → Decide → Document

An IQR-identified outlier is not automatically a data error and should not automatically be removed.
6. Exploratory Data Analysis
Univariate Analysis

Histograms with KDE curves and boxplots were used to investigate distributions.
Observed distributions

    TV: multimodal / non-normal appearance
    Radio: relatively flat distribution
    Newspaper: right-skewed distribution
    Sales: approximately bell-shaped with slight right skew

Outlier Investigation

Potential outliers were investigated using the IQR method:

IQR = Q3 − Q1

Lower Bound = Q1 − 1.5 × IQR

Upper Bound = Q3 + 1.5 × IQR

Results:

    TV: no potential IQR outliers
    Radio: no potential IQR outliers
    Newspaper: two potential upper-tail outliers
    Sales: no potential IQR outliers

The two potential newspaper outliers were investigated using their complete observations.

No obvious data-entry errors or implausible values were identified.

Decision: retain both observations.
7. Bivariate & Multivariate EDA
TV Advertising vs Sales

The scatter plot shows a clear positive association between TV advertising expenditure and sales.

    Approximately linear relationship
    Relatively strong visual relationship
    Increasing spread at higher TV expenditure
    Funnel-shaped residual pattern is visible

Radio Advertising vs Sales

The scatter plot shows a clear positive association between radio advertising expenditure and sales.

    Approximately linear relationship
    Relatively strong visual relationship
    Increasing spread at higher radio expenditure
    Funnel-shaped pattern is visible

Newspaper Advertising vs Sales

The scatter plot shows a weak and less clearly defined relationship between newspaper advertising expenditure and sales.

    Slight positive association
    Points are widely scattered
    No strong linear pattern is visually apparent
    Increasing spread at higher newspaper expenditure
    Relationship is substantially weaker than TV and radio

These are observational patterns and do not establish causation.
8. Correlation Analysis

Pearson correlation was used to quantify the strength and direction of linear relationships.

Correlation ranges from -1 to +1.
Predictor	Correlation with Sales
TV	0.782
Radio	0.576
Newspaper	0.228
Interpretation

    TV has the strongest positive linear association with sales.
    Radio has a moderate positive linear association with sales.
    Newspaper has a weak positive linear association with sales.

Important

Correlation ≠ Causation

Because this is observational data, correlations should be interpreted as associations rather than causal effects.

Correlation measures linear association. A correlation close to zero does not necessarily mean that no relationship exists.
9. Predictor Correlations
Predictor Pair	Correlation
TV ↔ Radio	0.05
TV ↔ Newspaper	0.06
Radio ↔ Newspaper	0.35
Interpretation

    TV has almost no linear association with radio or newspaper.
    Radio and newspaper have a weak positive association.
    No strong pairwise predictor correlation was observed.

Pairwise correlation alone cannot fully rule out multicollinearity.

Multicollinearity will be assessed later using VIF.
10. Initial EDA Hypotheses

Based on scatterplots, correlations, and the pairplot:

    TV: expected to have a strong positive association with sales.
    Radio: expected to have a positive association with sales.
    Newspaper: expected to have a weaker positive association with sales.

These are exploratory hypotheses.

They do not establish causation.
11. Simple Linear Regression — TV → Sales

Simple linear regression was fitted first to understand the relationship between TV advertising expenditure and sales.
Regression equation

Saleŝ = 7.0326 + 0.04754(TV)

Coefficients

    Intercept = 7.0326
    TV coefficient = 0.04754

TV coefficient interpretation

For a 1-unit increase in TV advertising expenditure, predicted sales increase by approximately 0.0475 sales units, on average.

This represents an association, not a causal effect.
Intercept

The intercept represents predicted sales when TV = 0.

However, TV = 0 is not represented in the observed data, so the intercept has limited direct business interpretation.
12. Hypothesis Testing — TV Coefficient
Statistical question

Is the true population slope for TV different from zero?
Hypotheses

H₀: β₁ = 0

H₁: β₁ ≠ 0

Results
Statistic	TV
Coefficient	0.0475
Standard Error	0.003
t-statistic	17.668
p-value	< 0.001
95% CI	[0.042, 0.053]
Interpretation

The estimated TV coefficient is positive and statistically significant.

Because the p-value is below conventional significance levels, we reject the null hypothesis that the population slope is zero.

There is strong statistical evidence of a positive linear association between TV advertising expenditure and sales.

This does not establish causation.
13. Model Evaluation — Simple Regression

A train/test split was used to evaluate performance on unseen observations.
Split

    80% training
    20% testing
    random_state = 42

TV-only model performance
Metric	Train	Test
MAE	2.583	2.444
MSE	10.604	10.205
RMSE	3.256	3.194
R²	0.591	0.677
Interpretation

    Training and test performance are similar.
    No obvious evidence of overfitting is visible from this split.
    Test RMSE is approximately 3.19 sales units.
    Test R² is approximately 0.677.
    Approximately 67.7% of the variation in test-set sales is explained by the TV-only model.

One train/test split is not sufficient to make a strong generalization claim.
14. Multiple Linear Regression

Because three advertising channels are available, multiple linear regression was used to model sales using:

TV + Radio + Newspaper

Model equation

Saleŝ =
2.9791
+ 0.0447(TV)
+ 0.1892(Radio)
+ 0.0028(Newspaper)

Coefficients
Predictor	Coefficient
TV	0.0447
Radio	0.1892
Newspaper	0.0028
Coefficient interpretation

TV

A 1-unit increase in TV expenditure is associated with approximately 0.0447 higher predicted sales, holding radio and newspaper expenditure constant.

Radio

A 1-unit increase in radio expenditure is associated with approximately 0.1892 higher predicted sales, holding TV and newspaper expenditure constant.

Newspaper

The newspaper coefficient requires statistical interpretation before deciding whether the variable provides useful evidence after accounting for TV and radio.

These coefficients represent associations, not causal effects.
15. Multiple Regression — Predictive Evaluation
Performance
Metric	Train	Test
MAE	1.198	1.461
MSE	2.705	3.174
RMSE	1.645	1.782
R²	0.896	0.899
Comparison with TV-only model
Model	Test RMSE	Test R²
TV only	3.194	0.677
TV + Radio + Newspaper	1.782	0.899
Interpretation

Adding Radio and Newspaper substantially improved predictive performance on the test set.

    RMSE improved from 3.194 → 1.782
    R² improved from 0.677 → 0.899
    Training and test performance remain relatively similar.
    No obvious evidence of overfitting is visible from this split.

However, the improvement should be assessed more robustly using cross-validation.
16. OLS Statistical Analysis — Multiple Regression

Statsmodels OLS was used to obtain statistical inference for the multiple regression model.
Results
Predictor	Coefficient	t-statistic	p-value	95% CI
Intercept	2.9791	8.427	< 0.001	[2.281, 3.677]
TV	0.0447	28.544	< 0.001	[0.042, 0.048]
Radio	0.1892	19.518	< 0.001	[0.170, 0.208]
Newspaper	0.0028	0.392	0.696	[-0.011, 0.017]
Overall model

    R² = 0.896
    Adjusted R² = 0.894
    F-statistic = 446.6
    Prob(F-statistic) < 0.001

Current interpretation

TV and Radio show statistically significant positive associations with sales after accounting for the other predictors.

Newspaper does not show statistically significant evidence of a linear association with sales after accounting for TV and Radio.

The Newspaper p-value is 0.696, and its 95% confidence interval includes zero.

This does not automatically mean Newspaper should be removed. Feature selection should consider predictive performance, statistical evidence, model simplicity, and cross-validation.
17. Statistical Significance vs Predictive Performance

These answer different questions.
Statistical inference

Asks:

    Is there evidence that a population coefficient differs from zero?

Tools include:

    t-statistic
    p-value
    confidence interval
    F-test

Predictive evaluation

Asks:

    How well does the model predict unseen observations?

Tools include:

    MAE
    MSE
    RMSE
    R²
    Train/test evaluation
    Cross-validation

A statistically significant predictor is not automatically a practically important predictor.

A predictive model can also be useful without every individual coefficient being statistically significant.
18. Regression Metrics — Quick Reference
MAE

Mean Absolute Error.

Measures the average absolute prediction error.

Lower is better.
MSE

Mean Squared Error.

Penalizes large errors more heavily.

Lower is better.
RMSE

Root Mean Squared Error.

Measures prediction error in the same units as the target.

Lower is better.
R²

Coefficient of determination.

Measures the proportion of variation in the target explained by the model.

Higher is better.
Current evaluation approach

RMSE is used as the primary error metric, with MAE and R² providing additional context.
19. OLS vs Machine Learning Models

OLS provides both model estimation and statistical inference.

For example:

    coefficients
    standard errors
    t-statistics
    p-values
    confidence intervals

Many machine-learning algorithms focus primarily on predictive performance rather than coefficient-based statistical inference.

For models such as neural networks, tree ensembles, and boosting algorithms, evaluation typically focuses on:

    test performance
    cross-validation
    error metrics
    feature importance
    permutation importance
    SHAP or other interpretation methods where appropriate

Statistical evaluation is still possible, but it is not necessarily based on OLS-style coefficient significance tests.
20. Current Limitations

Important limitations identified so far:

    Dataset is observational.
    Causal conclusions cannot be established from regression alone.
    Sampling method is not established.
    Only 200 observations are available.
    One train/test split can produce split-dependent results.
    Advertising budget units should not be assumed without documentation.
    Regression assumptions have not yet been fully diagnosed.
    Multicollinearity has not yet been formally assessed.
    The model does not automatically determine an optimal advertising budget.

21. Roadmap
Phase 1 — Business Understanding

Status: ✅ Complete

    Business problem
    Business objective
    ML objective
    Target/features
    Prediction vs causation vs optimization
    Cross-sectional vs time-series framing

Phase 2 — Environment & Data

Status: ✅ Complete

    Repository
    Codespace
    Virtual environment
    Dependencies
    Dataset
    Notebook

Phase 3 — Data Audit & Cleaning

Status: ✅ Complete

    Structure
    Schema
    Missing values
    Duplicates
    Descriptive statistics
    Identifier assessment
    Plausibility checks
    Outlier investigation
    Cleaning decisions

Phase 4 — EDA

Status: ✅ Complete

    Univariate analysis
    Distributions
    Outlier investigation
    Bivariate relationships
    Scatter plots
    Correlations
    Predictor correlations
    Heatmap
    Pairplot
    Hypothesis formation

Phase 5 — Regression

Status: 🟡 In Progress

Completed:

    Simple linear regression
    Regression equation
    Least squares
    Coefficients
    Intercept
    Residuals
    Hypothesis testing
    t-statistic
    p-value
    Confidence intervals
    Multiple linear regression
    Train/test evaluation
    OLS multiple regression

Remaining:

    Fully interpret multiple regression coefficients
    Assess whether Newspaper should remain in the model
    Compare models more rigorously

Phase 6 — Model Evaluation

Status: 🟡 In Progress

Completed:

    Train/test split
    Test predictions
    MAE
    MSE
    RMSE
    R²
    Train/test comparison
    Simple vs multiple regression comparison

Remaining:

    Cross-validation
    More robust model comparison

Phase 7 — Diagnostics

Status: ⬜ Not Started

    Linearity
    Independence
    Homoscedasticity
    Residual analysis
    Normality where relevant
    Multicollinearity
    VIF
    Outliers
    Influential observations

Phase 8 — Business Interpretation

Status: ⬜ Not Started

    Translate model results into business language
    Compare advertising channels
    Assess predictive usefulness
    Discuss budget decisions
    Discuss prediction vs optimization
    Discuss correlation vs causation
    Discuss limitations

Phase 9 — Portfolio & Interview

Status: ⬜ Not Started

    Final notebook
    Final README
    Business presentation
    Hiring-manager questions
    Strong interview answers
    Real-world improvements

22. Mentor Instructions

The project is being developed using a senior data scientist / hiring-manager mindset.
Teaching style

    Use Socratic questioning.
    Let reasoning come before solutions.
    Give hints before corrections.
    Explain why decisions are made.
    Challenge weak assumptions.
    Keep the project hands-on.
    Avoid unnecessary giant code blocks.
    Move forward only after understanding the current concept.

Professional mindset

For important decisions, ask:

    What did we observe?
    What does it mean?
    What evidence supports the conclusion?
    What decision should we make?
    What are the limitations?
    How would this be explained to a hiring manager?

Core distinctions

Always distinguish:

    Business problem
    Analytical problem
    Statistical question
    ML problem
    Prediction
    Association/correlation
    Causation
    Optimization

23. Learning Principle

Use:

Question → Data → Assumptions → Method → Evidence → Interpretation

The objective is not to finish the project quickly.

The objective is to become capable of defending every important analytical decision.
24. Current Checkpoint

Data Audit & Cleaning ✅ → EDA ✅ → Simple Regression + Hypothesis Testing ✅ → Train/Test Evaluation ✅ → Multiple Regression ✅ → OLS Statistical Analysis ✅ → Cross-Validation 🔵 → Diagnostics ⬜


One important change I made deliberately: **Cross-validation is now the next step before we finalize the multiple-regression model.** We shouldn't jump straight to diagnostics yet, because we've already recognized that our current comparison depends on one random split.

The next session can therefore start exactly here:

> **Why should we use cross-validation when we already have a train/test split?**

That will connect directly to the concern you raised earlier about different random splits producing different RMSE values.

