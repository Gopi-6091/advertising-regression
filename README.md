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

.venv/
__pycache__/
.ipynb_checkpoints/


5. Current Project State
✅ Completed
Business understanding

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

Data audit

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

Confirmed findings

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

Cleaning assessment

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
    radio: relatively flat distribution
    newspaper: right-skewed distribution
    sales: approximately bell-shaped with slight right skew

Outlier investigation

Potential outliers were investigated using the IQR method:

IQR = Q3 - Q1

Lower Bound = Q1 - 1.5 × IQR
Upper Bound = Q3 + 1.5 × IQR

Results:

    TV: no potential IQR outliers
    radio: no potential IQR outliers
    newspaper: two potential upper-tail outliers
    sales: no potential IQR outliers

The two potential newspaper outliers were investigated using their complete observations.

No obvious data-entry errors or implausible values were identified.

Decision: retain both observations.
Current cleaning principle

Observe → Investigate → Reason → Decide → Document

An IQR-identified outlier is not automatically a data error and should not automatically be removed.
6. Current Open Questions

The following remain to be investigated.
Column naming

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
EDA relationships

We have not yet investigated:

    Advertising expenditure vs sales
    Correlations
    Relationships between advertising channels
    Multivariate patterns
    Potential hypotheses

These will be addressed during EDA.
7. 🔴 NEXT ACTION

Continue Phase 4 — Exploratory Data Analysis.

Next:

    Begin bivariate analysis
    Visualize advertising expenditure vs sales
    Start with scatter plots
    Examine relationships between each advertising channel and sales
    Calculate and interpret correlations
    Investigate relationships between predictors
    Develop evidence-based hypotheses
    Move toward multivariate analysis

Do not jump into regression before understanding the relationships in the data.
8. Roadmap
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
    Multivariate relationships
    Visualizations
    Hypothesis formation

Status: 🟡 In Progress
Completed in Phase 4

    Univariate distributions
    Histogram + KDE analysis
    Boxplot analysis
    IQR-based outlier investigation

Next

    Bivariate analysis
    Advertising expenditure vs sales
    Correlation analysis

Phase 5 — Regression

    Simple linear regression
    Regression equation
    Least squares
    Coefficients
    Intercept
    Residuals
    Multiple linear regression
    Compare models

Status: ⬜ Not started
Phase 6 — Evaluation

    Train/test split
    MAE
    MSE
    RMSE
    R²
    Understand why accuracy is inappropriate

Status: ⬜ Not started
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
9. Mentor Instructions

When working on this project, act as my senior data scientist mentor and hiring-manager-level coach.
Teaching style

    Use Socratic questioning.
    Let me attempt reasoning first.
    Do not give the complete solution upfront.
    Give hints before corrections.
    Explain the "why" behind decisions.
    Challenge weak assumptions.
    Keep the project hands-on.
    Avoid giant code blocks.
    Move forward only when I understand the current concept.

Professional mindset

For important decisions, help me answer:

    What did we observe?
    What does it mean?
    What evidence supports our conclusion?
    What decision should we make?
    What are the limitations?
    How would I explain this to a hiring manager?

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

Do not make causal claims from this observational dataset without appropriate justification.
10. Learning Principles

Start with:

Question → Data → Assumptions → Method → Evidence → Interpretation
Univariate

One variable.

Example:

Distribution of sales
Bivariate

Two variables.

Example:

TV vs sales
Multivariate

Multiple variables considered together.

Example:

TV + radio + newspaper → sales

These describe the number of variables being analyzed, not whether variables are numeric or categorical.

Variable type determines which analytical methods and visualizations are appropriate.
11. Interview Questions to Develop

Do not answer these all at once.

Introduce them when relevant during the project.

    Why linear regression?
    Why not a time-series model?
    Why these features?
    Why exclude Unnamed: 0?
    What does a coefficient mean?
    What does the intercept mean?
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

12. Session Continuation Rule

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

