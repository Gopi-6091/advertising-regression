Yes. I would update it now so the README reflects the finished analytical project, while being honest that the actual portfolio slide deck and final code polish are separate deliverables.

I also corrected a few important inconsistencies from the old README:

Diagnostics are now marked complete, not pending.
Multicollinearity/VIF is marked completed.
Business interpretation is complete.
The distinction between the three-variable OLS model and the preferred TV + Radio predictive model is made explicit.
I removed language suggesting coefficients are correlations.
I strengthened the prediction/causation/optimization distinction.
The roadmap now reflects the actual current state.
The final recommendation is more hiring-manager friendly.

Here is the complete replacement README:

Advertising Regression — Data Science Portfolio Project
🎯 Project Overview

This project uses linear regression to investigate the relationship between advertising expenditure and product sales.

The dataset contains advertising expenditure across three channels:

TV
Radio
Newspaper

The objective was not simply to build a predictive model. The project was designed to demonstrate an end-to-end data science workflow:

Business problem → Data understanding → Data quality → Cleaning → EDA → Hypotheses → Modeling → Evaluation → Diagnostics → Business interpretation → Limitations

The project also emphasizes an important analytical distinction:

Prediction ≠ correlation ≠ causation ≠ optimization

Because the dataset is observational, the regression models identify statistical associations and support prediction, but they do not establish that increasing advertising expenditure causes sales to increase.

1. Business Problem

A company invests in three advertising channels:

TV
Radio
Newspaper
Business objective

Understand how advertising expenditure is associated with sales and determine whether the available data can provide useful information for advertising-related business decisions.

Machine learning objective

Build a model that predicts sales using advertising expenditure.

Target

sales

Predictors
TV
radio
newspaper
Analytical framing

The observations represent different markets rather than sequential measurements over time.

Therefore, this project is treated as a cross-sectional regression problem, not a time-series forecasting problem.

Important distinction

The model can:

quantify associations,
estimate predicted sales,
compare predictive performance,
support hypothetical spending scenarios.

The model cannot, by itself:

establish causation,
prove that increasing advertising expenditure will increase sales,
determine the economically optimal advertising budget.
2. Dataset

Dataset: Advertising.csv

Source: ISLR — An Introduction to Statistical Learning

Official dataset resource:

ISLR Data Resources

The dataset contains 200 observations across different markets.

Columns
Column	Role	Meaning
Unnamed: 0	Identifier	Observation/index identifier
TV	Predictor	TV advertising expenditure
radio	Predictor	Radio advertising expenditure
newspaper	Predictor	Newspaper advertising expenditure
sales	Target	Product sales

The exact units of the advertising expenditure variables were not assumed and were treated according to the original dataset documentation.

3. Environment

The project was developed using:

VS Code
GitHub Codespaces
Python 3.12.1
.venv virtual environment
Jupyter Notebook
Main dependencies
pandas
numpy
matplotlib
seaborn
scikit-learn
statsmodels
jupyter
ipykernel

Dependencies are recorded in requirements.txt.

The virtual environment is excluded from Git.

4. Project Structure
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


.venv/, __pycache__/, and .ipynb_checkpoints/ are excluded from version control.

5. Data Audit & Cleaning
Dataset structure

The dataset was confirmed to contain:

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

Data quality checks

The following checks were completed:

Inspected initial observations
Checked dataset shape
Checked column names
Checked data types
Inspected df.info()
Checked missing values
Checked duplicate rows
Checked uniqueness
Inspected descriptive statistics
Checked for obvious negative values
Reviewed the original dataset documentation
Investigated potential outliers
Findings
No missing values were detected.
No duplicate rows were detected.
No obvious data-entry errors were identified.
Advertising variables represent advertising expenditure.
sales represents product sales.
Observations represent different markets.
Identifier assessment

Unnamed: 0 was identified as an observation/index identifier rather than a meaningful business predictor.

Decision
Exclude Unnamed: 0 from regression features.
Keep the original CSV unchanged.
Do not remove observations solely because they appear unusual.
Cleaning principle

Observe → Investigate → Reason → Decide → Document

An IQR-identified outlier is not automatically a data error and should not automatically be removed.

6. Exploratory Data Analysis
Univariate analysis

Histograms with KDE curves and boxplots were used to investigate distributions and potential outliers.

Observed distributions
TV: multimodal / non-normal appearance
Radio: relatively flat distribution
Newspaper: right-skewed distribution
Sales: approximately bell-shaped with slight right skew

The distributions were treated as descriptive evidence rather than automatic reasons for transforming or removing variables.

7. Outlier Investigation

Potential outliers were investigated using the IQR method.

IQR=Q3−Q1

Lower Bound=Q1−1.5(IQR)

Upper Bound=Q3+1.5(IQR)

Results
Variable	Potential IQR outliers
TV	0
Radio	0
Newspaper	2
Sales	0

The two potential Newspaper upper-tail observations were investigated using their complete observations.

No obvious data-entry errors or implausible values were identified.

Decision

Both observations were retained.

This reflects the principle that statistical unusualness does not automatically imply incorrect data.

8. Bivariate & Multivariate EDA
TV Advertising vs Sales

The scatter plot showed a clear positive association between TV advertising expenditure and Sales.

Observed characteristics:

Approximately linear relationship
Relatively strong visual relationship
Increasing spread at higher TV expenditure
Some evidence of a funnel-shaped residual pattern
Radio Advertising vs Sales

The scatter plot showed a clear positive association between Radio advertising expenditure and Sales.

Observed characteristics:

Approximately linear relationship
Relatively strong visual relationship
Increasing spread at higher Radio expenditure
Newspaper Advertising vs Sales

The scatter plot showed a substantially weaker relationship.

Observed characteristics:

Slight positive association
Wide scatter
No strong linear pattern visually apparent
Increasing spread at higher Newspaper expenditure

These observations are exploratory and do not establish causation.

9. Correlation Analysis

Pearson correlation was used to quantify the strength and direction of linear associations.

Correlation ranges from -1 to +1.

Predictor	Correlation with Sales
TV	0.782
Radio	0.576
Newspaper	0.228
Interpretation
TV has the strongest positive linear association with Sales.
Radio has a moderate positive linear association with Sales.
Newspaper has a weak positive linear association with Sales.
Important distinction

Correlation ≠ causation

Correlation measures pairwise linear association. It does not establish that changing one variable will cause the other variable to change.

A correlation close to zero also does not necessarily mean that no relationship exists; it specifically indicates weak linear association.

10. Predictor Correlations
Predictor pair	Correlation
TV ↔ Radio	0.05
TV ↔ Newspaper	0.06
Radio ↔ Newspaper	0.35
Interpretation
TV has almost no pairwise linear association with Radio or Newspaper.
Radio and Newspaper have a weak positive association.
No strong pairwise predictor correlation was observed.

Pairwise correlation alone cannot fully rule out multicollinearity, so VIF was assessed during model diagnostics.

11. Initial EDA Hypotheses

Based on scatterplots, correlations, and the pairplot:

TV: expected to have a strong positive association with Sales.
Radio: expected to have a positive association with Sales.
Newspaper: expected to have a weaker positive association with Sales.

These were treated as exploratory hypotheses rather than causal claims.

12. Simple Linear Regression — TV → Sales

Simple linear regression was fitted first to understand the relationship between TV advertising expenditure and Sales.

Regression equation

Sales^=7.0326+0.04754(TV)

Coefficients
Term	Estimate
Intercept	7.0326
TV coefficient	0.04754
TV coefficient interpretation

A one-unit increase in TV advertising expenditure is associated with approximately 0.0475 higher predicted Sales, on average.

This is an association estimated from observational data, not a causal effect.

Intercept

The intercept represents predicted Sales when TV = 0.

However, TV = 0 is not represented in the observed data, so the intercept has limited direct business interpretation.

13. Hypothesis Testing — TV Coefficient
Statistical question

Is the population slope for TV different from zero?

Hypotheses

H0:β1=0

H1:β1≠0

Results
Statistic	TV
Coefficient	0.0475
Standard Error	0.003
t-statistic	17.668
p-value	< 0.001
95% CI	[0.042, 0.053]
Interpretation

The estimated TV coefficient is positive and statistically significant.

Because the p-value is below conventional significance levels, there is strong statistical evidence that the population TV slope differs from zero under the assumptions of the model.

Therefore, the analysis provides strong evidence of a positive linear association between TV advertising expenditure and Sales.

This does not establish causation.

14. Model Evaluation — Simple Regression

An 80/20 train/test split was used:

Training set: 80%
Test set: 20%
random_state = 42
TV-only model performance
Metric	Train	Test
MAE	2.583	2.444
MSE	10.604	10.205
RMSE	3.256	3.194
R²	0.591	0.677
Interpretation
Training and test performance were relatively similar for this split.
No obvious evidence of severe overfitting was observed from this split.
Test RMSE was approximately 3.19 Sales units.
Test R² was approximately 0.677.
Approximately 67.7% of the test-set variation in Sales was explained by the TV-only model.

A single train/test split can be split-dependent, so it was not treated as sufficient evidence for final model selection.

15. Multiple Linear Regression

Because three advertising channels were available, multiple linear regression was used to model Sales using:

TV
Radio
Newspaper
Full multiple-regression equation

Sales^=2.9791+0.0447(TV)+0.1892(Radio)+0.0028(Newspaper)

Coefficients
Predictor	Coefficient
TV	0.0447
Radio	0.1892
Newspaper	0.0028
Coefficient interpretation

TV

A one-unit increase in TV expenditure is associated with approximately 0.0447 higher predicted Sales, holding Radio and Newspaper expenditure constant.

Radio

A one-unit increase in Radio expenditure is associated with approximately 0.1892 higher predicted Sales, holding TV and Newspaper expenditure constant.

Newspaper

The Newspaper coefficient is close to zero and requires statistical and predictive evaluation before deciding whether it adds useful information.

Important

Regression coefficients are not correlations.

The magnitude of a coefficient also should not be used by itself to determine which advertising channel is "better", because coefficient magnitude depends on the predictor's measurement scale and range.

16. Multiple Regression — Predictive Evaluation
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

Adding Radio and Newspaper substantially improved predictive performance on this particular train/test split.

However, a single split can produce split-dependent results.

Therefore, cross-validation was used for a more robust comparison.

17. OLS Statistical Analysis — Multiple Regression

statsmodels OLS was used to obtain statistical inference for the full multiple regression model.

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
Interpretation

TV and Radio show statistically significant positive associations with Sales after accounting for the other predictors.

Newspaper does not show statistically significant evidence of an independent linear association with Sales after accounting for TV and Radio.

The Newspaper p-value is 0.696, and its 95% confidence interval includes zero.

Important model-selection principle

A high p-value does not automatically mean a variable should be removed.

Feature selection should consider:

statistical evidence,
predictive performance,
cross-validation,
model simplicity,
diagnostics,
and the purpose of the model.
18. Cross-Validation & Model Selection

Because a single train/test split can produce different RMSE values depending on which observations are selected, cross-validation was used to obtain a more robust estimate of predictive performance.

The following models were compared:

TV + Radio
TV + Radio + Newspaper
Cross-validation results
Model	Mean CV RMSE
TV + Radio	1.663
TV + Radio + Newspaper	1.682

The analysis also examined CV RMSE variability using the standard deviation.

Model preference

The TV + Radio model is currently preferred because:

It achieved slightly lower cross-validated RMSE.
It uses fewer predictors.
It is simpler to interpret.
Newspaper did not provide strong statistical evidence of an independent linear association in the full OLS model.

The difference in CV RMSE is relatively small, so this should not be interpreted as evidence that Newspaper is completely useless in every possible modeling context.

19. Statistical Significance vs Predictive Performance

These answer different questions.

Statistical inference asks:

Is there evidence that a population coefficient differs from zero?

Tools include:

t-statistics
p-values
confidence intervals
F-tests
Predictive evaluation asks:

How well does the model predict observations that were not used to fit it?

Tools include:

MAE
MSE
RMSE
R²
train/test evaluation
cross-validation
Key principle

A statistically significant predictor is not automatically practically important.

Likewise:

A variable that is not individually statistically significant does not automatically make a model useless.

Model decisions should reflect the objective and multiple forms of evidence.

20. Regression Diagnostics

The regression assumptions and model behavior were investigated using:

Residual distribution
Residuals vs fitted values
Q-Q plot
Linearity assessment
Homoscedasticity assessment
Breusch-Pagan test
Variance Inflation Factor (VIF)
Cook's distance
Influential observation investigation
Diagnostic conclusion

No major regression assumption violations were identified.

However, some limitations remain:

Some tail deviations were visible in the residual/Q-Q analysis.
Possible mild curvature remains relevant to the linear-model assumption.
Some observations were influential and were investigated.
These observations were not automatically removed because influence does not necessarily indicate erroneous data.

Overall, the diagnostics did not reveal a major reason to reject the linear regression approach for this project.

21. Regression Metrics — Quick Reference
MAE — Mean Absolute Error

Measures the average absolute prediction error.

Lower is better.

MSE — Mean Squared Error

Measures squared prediction error and penalizes larger errors more strongly.

Lower is better.

RMSE — Root Mean Squared Error

Measures prediction error in the same units as the target.

Lower is better.

For the preferred TV + Radio model:

Cross-validated RMSE ≈ 1.663 Sales units

This does not mean every prediction is exactly ±1.663 away from the actual value. Individual prediction errors can be positive or negative, while RMSE itself is always non-negative.

R² — Coefficient of Determination

Measures the proportion of variation in the target explained by the model under the fitted-data framework.

Higher is generally better for predictive comparison, but it should not be interpreted alone.

Evaluation approach

RMSE was used as the primary prediction-error metric, with MAE and R² providing additional context.

22. OLS vs Machine Learning Models

OLS provides both model estimation and statistical inference.

For example:

coefficients
standard errors
t-statistics
p-values
confidence intervals
F-tests

Many machine-learning algorithms focus primarily on predictive performance rather than coefficient-based statistical inference.

For models such as:

decision trees,
random forests,
gradient boosting,
neural networks,

evaluation typically focuses on:

test performance,
cross-validation,
error metrics,
feature importance,
permutation importance,
SHAP or other interpretation methods where appropriate.

The appropriate modeling approach depends on whether the primary objective is:

inference,
prediction,
interpretation,
or optimization.
23. Business Interpretation
What did we learn?
TV

TV shows a strong positive association with Sales and provides substantial predictive information.

Radio

Radio also shows a positive association with Sales and provides statistically significant predictive information after accounting for the other advertising variables.

Newspaper

Newspaper shows a much weaker pairwise association with Sales and does not show statistically significant evidence of an independent linear association after accounting for TV and Radio.

Preferred model

The current preferred predictive model is:

TV + Radio

It achieved a slightly better cross-validated RMSE than the three-variable model while using fewer predictors.

24. Prediction vs Causation

This is a central limitation of the project.

The model estimates statistical associations from observational data.

Therefore, we can say:

"Higher TV expenditure is associated with higher predicted Sales, holding the other modeled variables constant."

We should not say:

"Increasing TV expenditure will cause Sales to increase by 0.0447 units."

The second statement is causal and is not established by this observational regression analysis.

Key distinction

Association:
Variables move together in the observed data.

Prediction:
The model estimates an outcome for a given set of predictor values.

Causation:
Changing one variable produces a change in another.

These are different analytical claims.

25. Can the Model Support Advertising-Budget Decisions?
Yes — but cautiously.

The regression equation can be used for scenario analysis.

For example, the business could ask:

"What Sales does the model predict for a particular TV and Radio spending combination?"

The model can therefore help evaluate hypothetical spending scenarios.

However:

Scenario prediction ≠ optimal budget allocation.

The model should not automatically be used to claim:

"The optimal advertising budget is X for TV and Y for Radio."

26. Why Prediction Does Not Automatically Provide Optimization

Optimal advertising allocation is a different problem.

A true budget-optimization analysis would require additional information and assumptions, such as:

causal effects of advertising expenditure,
total available budget,
channel-specific costs,
diminishing returns,
nonlinear relationships,
profit margins,
business constraints,
opportunity costs,
and potentially experimental or stronger causal evidence.

The regression equation alone does not provide all of this information.

27. Final Business Recommendation

The analysis indicates that TV and Radio are useful predictors of Sales, while Newspaper provides limited additional statistical and predictive evidence after accounting for TV and Radio. The TV + Radio model is therefore preferred for this project because it provides slightly better cross-validated predictive performance with fewer predictors. The model can support Sales prediction and hypothetical advertising-spend scenarios, but because the data is observational, the results should not be interpreted as causal effects or as a definitive solution for optimal advertising-budget allocation.

28. Final Project Findings
Finding 1 — TV

TV has the strongest pairwise linear association with Sales:

r=0.782

It also has a statistically significant positive coefficient in the multiple regression.

Finding 2 — Radio

Radio has a moderate pairwise linear association with Sales:

r=0.576

It also has a statistically significant positive coefficient in the multiple regression.

Finding 3 — Newspaper

Newspaper has a weak pairwise association:

r=0.228

Its multiple-regression coefficient is not statistically significant:

p=0.696

Finding 4 — Predictive performance

The TV + Radio model achieved:

CV RMSE≈1.663

compared with:

CV RMSE≈1.682

for TV + Radio + Newspaper.

Finding 5 — Model diagnostics

No major regression assumption violations were identified, although some tail deviations, possible mild curvature, and influential observations remain limitations.

29. Limitations

Important limitations include:

The dataset is observational.
Causal conclusions cannot be established from regression alone.
The sampling method is not established.
Only 200 observations are available.
Train/test performance can depend on the particular random split.
Cross-validation provides a more robust estimate but does not eliminate all uncertainty.
Advertising expenditure units should not be assumed without reference to the original documentation.
Linear regression assumes a functional form that may not capture all nonlinear relationships.
Some residual tail deviations and possible mild curvature remain.
Influential observations exist and were investigated.
Statistical significance does not automatically imply business importance.
The model does not automatically determine an optimal advertising budget.
A real budget-optimization system would require economic and causal information beyond this dataset.
30. End-to-End Analytical Workflow
BUSINESS QUESTION
        │
        ▼
Does advertising expenditure
relate to / predict Sales?
        │
        ▼
DATA AUDIT
        │
        ▼
200 observations
No missing values
No duplicates
Identifier assessed
        │
        ▼
EDA
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
TV    Radio   Newspaper
 │      │        │
Strong Moderate Weak
positive positive positive
 │      │        │
 └──────┼────────┘
        ▼
CORRELATION
        │
        ▼
TV strongest
Radio moderate
Newspaper weak
        │
        ▼
SIMPLE REGRESSION
TV → Sales
        │
        ▼
β = 0.0475
p < 0.001
        │
        ▼
Evidence of positive association
        │
        ▼
MULTIPLE REGRESSION
        │
 ┌──────┼──────────┐
 ▼      ▼          ▼
TV    Radio    Newspaper
0.0447 0.1892     0.0028
  ✓      ✓          ?
                     │
                  p = 0.696
                     │
                     ▼
              Weak statistical
                 evidence
        │
        ▼
TRAIN / TEST EVALUATION
        │
        ▼
Prediction error
        │
        ▼
CROSS-VALIDATION
        │
 ┌──────┴─────────────┐
 ▼                    ▼
TV + Radio      TV + Radio +
RMSE 1.663      Newspaper
                RMSE 1.682
 │                    │
 └──────────┬─────────┘
            ▼
       TV + Radio
       preferred
            │
            ▼
       DIAGNOSTICS
            │
            ▼
No major violations
identified
            │
            ▼
 BUSINESS INTERPRETATION
            │
            ▼
TV + Radio useful
for prediction
            │
            ▼
 SCENARIO ANALYSIS
            │
            ▼
Prediction ≠ Causation
Prediction ≠ Optimization
            │
            ▼
FINAL RECOMMENDATION
Use model for prediction and
scenario analysis, not standalone
budget optimization.

31. Portfolio Presentation Story

The project can be presented in approximately 5–7 minutes using the following structure:

Slide 1 — Business Problem
Advertising expenditure across TV, Radio, Newspaper
Objective: understand relationships and predict Sales
Important limitation: observational data
Slide 2 — Data & Quality
200 observations
Data structure
No missing values
No duplicates
Outliers investigated
Identifier excluded from modeling
Slide 3 — EDA
TV: strongest relationship
Radio: moderate relationship
Newspaper: weak relationship
Correlation analysis
Slide 4 — Modeling
Simple TV regression
Multiple regression
Coefficients
Statistical inference
Slide 5 — Model Evaluation
Train/test metrics
Cross-validation
TV + Radio vs all three predictors
Preferred model: TV + Radio
Slide 6 — Diagnostics
Residual analysis
Q-Q plot
Breusch-Pagan
VIF
Cook's distance
Slide 7 — Business Findings
TV and Radio are useful predictors
Newspaper adds limited evidence
TV + Radio provides the preferred predictive model
Slide 8 — Recommendation & Limitations
Use model for prediction and scenario analysis
Do not interpret coefficients causally
Do not claim an optimal advertising budget
Further causal/economic analysis would be required for optimization
32. Interview-Ready Project Summary

A concise explanation of the project is:

I analyzed advertising expenditure across TV, Radio, and Newspaper using linear regression to understand their relationship with Sales and evaluate their predictive usefulness. I performed data auditing, exploratory analysis, simple and multiple regression, statistical inference, train/test evaluation, cross-validation, and regression diagnostics. TV and Radio showed statistically significant positive associations with Sales, while Newspaper provided limited additional evidence. The TV + Radio model was preferred because it achieved slightly better cross-validated RMSE of approximately 1.66 while using fewer predictors. Because the data is observational, I interpret the results as associations and use the model for prediction and scenario analysis rather than causal inference or direct budget optimization.

33. Project Status
Phase	Status
Business Understanding	✅ Complete
Environment & Data	✅ Complete
Data Audit & Cleaning	✅ Complete
Exploratory Data Analysis	✅ Complete
Regression Modeling	✅ Complete
Statistical Inference	✅ Complete
Model Evaluation	✅ Complete
Cross-Validation	✅ Complete
Regression Diagnostics	✅ Complete
Business Interpretation	✅ Complete
Business Storytelling	✅ Complete
Interview Preparation	🔵 Next
Final Portfolio Packaging	🔵 Next
34. Learning Framework

The project followed the principle:

Question → Data → Assumptions → Method → Evidence → Interpretation

For important analytical decisions, the project asked:

What did we observe?
What does it mean?
What evidence supports the conclusion?
What decision should we make?
What are the limitations?
How would this be explained to a hiring manager?

The objective was not simply to produce a model.

The objective was to develop the ability to defend the reasoning behind the analysis.

35. Final Takeaway

The Advertising Regression project demonstrates an end-to-end approach to statistical modeling: starting with a business question, validating the data, exploring relationships, building interpretable regression models, evaluating predictive performance, testing assumptions, and translating the findings into a responsible business recommendation.

The strongest conclusion is not that one advertising channel "causes" more sales. Rather, the evidence suggests that TV and Radio are useful predictors of Sales, and a simpler TV + Radio model provides slightly better cross-validated predictive performance than the model including Newspaper. The model can support prediction and scenario analysis, while causal inference and true budget optimization require additional evidence and business information.