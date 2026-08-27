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

A regression model can identify associations and make predictions, but it does not automatically prove that increasing advertising expenditure causes sales to increase.

A predictive model also does not automatically determine the optimal advertising budget.

---

# 2. Dataset

Dataset: `Advertising.csv`

Source: ISLR — An Introduction to Statistical Learning

Official resource:

https://trevorhastie.github.io/ISLR/data.html

The dataset contains 200 observations across different markets.

The observations are not sequential time measurements, so we frame this as a **cross-sectional regression problem**, not a time-series forecasting problem.

### Actual columns observed

- `Unnamed: 0`
- `TV`
- `radio`
- `newspaper`
- `sales`

### Current interpretation

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

`.venv/` must not be committed to Git.

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

.gitignore includes:

.venv/
__pycache__/
.ipynb_checkpoints/

The raw dataset remains unchanged.

5. Current Project State
✅ Completed
Business Understanding

    Defined business problem
    Defined business objective
    Defined ML objective
    Identified target
    Identified predictors
    Distinguished prediction, correlation, causation, and optimization
    Established cross-sectional rather than time-series framing

Environment

    Created GitHub repository
    Created Codespace
    Created .venv
    Installed dependencies
    Created project directories
    Created requirements.txt
    Created .gitignore
    Added dataset
    Created notebook
    Selected .venv kernel
    Successfully loaded dataset

Data Audit

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

Confirmed Findings

    200 rows
    5 columns
    4 numerical business variables:
        TV
        radio
        newspaper
        sales
    1 identifier-like column:
        Unnamed: 0
    All columns contain 200 non-null values
    No missing values detected
    No duplicate rows detected
    Advertising variables represent advertising budgets
    sales represents product sales
    Observations represent different markets

Cleaning Assessment

    Unnamed: 0 identified as an observation identifier rather than a meaningful business predictor
    Unnamed: 0 will be excluded from regression features
    Raw CSV remains unchanged
    No observations were removed
    No values were modified
    No evidence of data-entry errors was found

Univariate EDA

Histograms with KDE curves and boxplots were used to investigate the distributions.

Observed distributions:

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
Current Cleaning Principle

Observe → Investigate → Reason → Decide → Document

An IQR-identified outlier is not automatically a data error and should not automatically be removed.
6. EDA Findings
Bivariate Analysis

Scatterplots were used to investigate the relationship between each advertising channel and sales.
TV → Sales

    Clear positive association
    Approximately linear relationship
    Relatively strong visual relationship
    Increasing spread at higher TV expenditure, creating a funnel-shaped pattern

Radio → Sales

    Clear positive association
    Approximately linear relationship
    Relatively strong visual relationship
    Increasing spread at higher radio expenditure, creating a funnel-shaped pattern

Newspaper → Sales

    Slight positive association
    Much weaker relationship than TV and radio
    No clearly defined linear pattern
    Points are substantially more scattered
    Increasing spread at higher newspaper expenditure

These are observational patterns and do not establish causation.
Correlation Analysis

Pearson correlation was used to quantify the strength and direction of linear relationships.

Correlation ranges from -1 to +1:

    +1 → perfect positive linear relationship
    0 → no linear relationship
    -1 → perfect negative linear relationship

Correlation with Sales
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

A high correlation does not prove that one variable causes another.

Because this is observational data, these correlations should be interpreted as associations, not causal effects.

Correlation also measures linear association. A correlation close to zero does not necessarily mean that no relationship exists; a non-linear relationship may still be present.
Correlation Between Predictors

Pairwise correlations between advertising predictors:
Predictor Pair	Correlation
TV ↔ Radio	0.05
TV ↔ Newspaper	0.06
Radio ↔ Newspaper	0.35
Interpretation

    TV has almost no linear association with either radio or newspaper.
    Radio and newspaper have a weak positive association.
    There is no evidence of strong pairwise correlation among the predictors.

However, pairwise correlation alone cannot fully rule out multicollinearity.

Multicollinearity will be assessed more formally later using VIF during regression diagnostics.
Multivariate EDA

A pairplot was used to visualize relationships among all numerical variables simultaneously.

The pairplot generally agrees with the correlation results:

    TV shows the clearest relationship with sales.
    Radio shows a noticeable positive relationship with sales.
    Newspaper shows the weakest and most scattered relationship.
    Predictor-to-predictor relationships are generally weak.

Initial EDA Hypotheses

Based on the scatterplots, correlation analysis, and pairplot:

    TV: Expected to have a strong positive association with sales.
    Radio: Expected to have a positive association with sales.
    Newspaper: Expected to have a weaker positive association with sales.

These are exploratory hypotheses based on observed patterns.

They do not establish causation.
7. Simple Linear Regression — TV → Sales

Simple linear regression was used first to understand the relationship between TV advertising expenditure and sales before moving to multiple regression.
Regression Equation

The fitted model is:

Sales^=7.0326+0.04754(TV)
Coefficient Interpretation

    Intercept = 7.0326
    TV coefficient = 0.04754

The TV coefficient means:

    For a 1-unit increase in TV advertising expenditure, predicted sales increase by approximately 0.0475 units, on average.

This represents an association, not a causal effect.
Intercept Interpretation

The intercept represents predicted sales when TV = 0.

However, TV = 0 is not represented in the observed data, so the intercept has limited direct business interpretation.

The intercept is still mathematically necessary to define the regression line.
Predictions and Residuals

Predictions were generated using:

y_pred = model.predict(X)

A residual is defined as:

Residual=Actual−Predicted

Residuals represent the difference between the observed sales value and the value predicted by the regression line.

The scatterplot showed increasing spread in the observations at higher TV expenditure, producing a funnel-shaped pattern.

This will be investigated formally during regression diagnostics.
8. Hypothesis Testing — TV Coefficient

The statistical question is:

    Is the true population slope for TV different from zero?

Hypotheses

H0:β1=0

The true population slope is zero; there is no linear association between TV advertising expenditure and sales.

H1:β1≠0

The true population slope differs from zero.
Regression Results
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

This result does not establish that TV advertising causes sales to increase.
Statistical Significance vs Predictive Performance

Statistical significance and predictive performance answer different questions.

    Hypothesis testing asks whether the coefficient provides evidence of a non-zero population association.
    Model evaluation asks how accurately the model predicts unseen observations.

Predictive performance will be evaluated separately using train/test evaluation and metrics such as MAE, MSE, RMSE, and R².
9. Current Open Questions
Column Naming

The original dataset uses:

    TV
    radio
    newspaper
    sales

We have not yet decided whether column names should be standardized.

Any renaming should be deliberate and documented.
Sampling

We know the dataset contains 200 markets.

We have not established the sampling method used to select those markets.

Do not assume random sampling without evidence.
Model Evaluation

The simple regression model has been fitted and statistically evaluated, but its performance on unseen observations has not yet been assessed.

Next we need to investigate:

    Train/test split
    MAE
    MSE
    RMSE
    R²
    Generalization to unseen data

10. 🔵 NEXT ACTION

Begin Phase 6 — Model Evaluation.

Next:

    Split the data into training and test sets
    Train the TV → Sales model on the training data
    Generate predictions on the test data
    Evaluate predictions using:
        MAE
        MSE
        RMSE
        R²
    Understand why accuracy is not appropriate for regression
    Compare training and test performance

Do not jump directly into multiple regression.

First evaluate how well the simple regression model generalizes to unseen data.
11. Roadmap
Phase 1 — Business Understanding

    Business problem
    Business objective
    ML objective
    Target/features
    Prediction vs causation vs optimization
    Cross-sectional vs time-series framing

Status: ✅ Complete
Phase 2 — Environment & Data

    Repository
    Codespace
    Virtual environment
    Dependencies
    Dataset
    Notebook

Status: ✅ Complete
Phase 3 — Data Audit & Cleaning

    Structure
    Schema
    Missing values
    Duplicates
    Descriptive statistics
    Identifier assessment
    Plausibility checks
    Outlier investigation
    Cleaning decisions
    Final analytical interpretation

Status: ✅ Complete
Phase 4 — EDA

    Univariate analysis
    Distributions
    Outlier investigation
    Bivariate relationships
    Scatter plots
    Correlations
    Predictor correlations
    Correlation heatmap
    Multivariate relationships
    Pairplot
    Hypothesis formation

Status: ✅ Complete
Key EDA Findings

    TV has the strongest positive linear association with sales (r = 0.782)
    Radio has a moderate positive linear association with sales (r = 0.576)
    Newspaper has a weak positive linear association with sales (r = 0.228)
    Predictor pairwise correlations are generally weak
    No strong pairwise predictor correlation was observed
    TV and radio appear more strongly associated with sales than newspaper
    These findings represent associations, not causal effects

Phase 5 — Regression

    Simple linear regression
    Regression equation
    Least squares
    Coefficients
    Intercept
    Residuals
    Hypothesis testing
    Null hypothesis
    Alternative hypothesis
    t-statistic
    p-value
    Statistical significance
    Confidence intervals
    Multiple linear regression
    Compare models

Status: 🟡 In Progress
Completed in Phase 5

    Fitted simple linear regression for TV → Sales
    Interpreted intercept
    Interpreted TV coefficient
    Generated predictions
    Introduced residuals
    Performed hypothesis testing
    Interpreted t-statistic
    Interpreted p-value
    Interpreted 95% confidence interval

Remaining in Phase 5

    Multiple linear regression
    Compare simple vs multiple regression models
    Interpret multiple regression coefficients

Phase 6 — Evaluation

    Train/test split
    MAE
    MSE
    RMSE
    R²
    Understand why accuracy is inappropriate
    Evaluate generalization to unseen data

Status: 🔵 Next
Phase 7 — Diagnostics

    Linearity
    Independence
    Homoscedasticity
    Residual analysis
    Normality where relevant
    Multicollinearity
    Outliers
    Influential observations

Status: ⬜ Not started
Phase 8 — Business Interpretation

    Translate model results into business language
    Compare advertising channels
    Assess predictive usefulness
    Discuss budget decisions
    Discuss prediction vs optimization
    Discuss correlation vs causation
    Discuss limitations

Status: ⬜ Not started
Phase 9 — Portfolio & Interview

    Final notebook
    Final README
    Business presentation
    Hiring-manager questions
    Strong interview answers
    Real-world improvements

Status: ⬜ Not started
12. Mentor Instructions

When working on this project, act as my senior data scientist mentor and hiring-manager-level coach.
Teaching Style

    Use Socratic questioning.
    Let me attempt reasoning first.
    Do not give the complete solution upfront.
    Give hints before corrections.
    Explain the "why" behind decisions.
    Challenge weak assumptions.
    Keep the project hands-on.
    Avoid giant code blocks.
    Move forward only when I understand the current concept.

Professional Mindset

For important decisions, help me answer:

    What did we observe?
    What does it mean?
    What evidence supports our conclusion?
    What decision should we make?
    What are the limitations?
    How would I explain this to a hiring manager?

Core Distinctions

Always distinguish:

    Business problem
    Analytical problem
    Statistical question
    ML problem
    Prediction
    Association/correlation
    Causation
    Optimization

Do not make causal claims from this observational dataset without appropriate justification.
13. Learning Principles

Start with:

Question → Data → Assumptions → Method → Evidence → Interpretation
Univariate

One variable.

Example:

Distribution of sales
Bivariate

Two variables.

Example:

TV vs Sales
Multivariate

Multiple variables considered together.

Example:

TV + Radio + Newspaper → Sales

These describe the number of variables being analyzed, not whether variables are numeric or categorical.

Variable type determines which analytical methods and visualizations are appropriate.
EDA Quick Reference

Question → Data Type → Appropriate Visualization → Evidence → Interpretation

    Numerical distribution → Histogram / KDE
    Potential outliers → Boxplot
    Numerical X vs Numerical Y → Scatterplot
    Linear relationship → Correlation
    Multiple numerical variables → Heatmap / Pairplot
    Categorical distribution → Count plot / Bar chart
    Categorical X vs Numerical Y → Boxplot / Violin plot
    Time-based data → Line chart
    Geographic data → Map

The workflow is reusable, but the exact visualizations depend on the data and analytical question.
14. Interview Questions to Develop

Do not answer these all at once.

Introduce them when relevant during the project.

    Why linear regression?
    Why not a time-series model?
    Why these features?
    Why exclude Unnamed: 0?
    What does a coefficient mean?
    What does the intercept mean?
    What is a null hypothesis?
    What is an alternative hypothesis?
    What is a p-value?
    What does statistical significance mean?
    Does correlation imply causation?
    How do you know the model is good?
    Why R²?
    Why not accuracy?
    What are the assumptions of linear regression?
    What happens when assumptions are violated?
    What is multicollinearity?
    How would you detect outliers?
    Can this model prove advertising causes sales?
    How could the model support budget decisions?
    What would you change with real company data?

15. Session Continuation Rule

When I return to this project:

    Read this README.
    Do not restart the project.
    Identify the current unchecked task.
    Ask me for my output/reasoning.
    Review it with me.
    Challenge misconceptions.
    Teach the relevant concept.
    Update the project status when a meaningful milestone is completed.
    Do not jump ahead unnecessarily.

Golden Rule

The objective is not to finish the project quickly.

The objective is to become capable of defending every important decision in the project.
Current Checkpoint

Data Audit & Cleaning ✅ → EDA ✅ → Simple Regression + Hypothesis Testing ✅ → Model Evaluation 🔵