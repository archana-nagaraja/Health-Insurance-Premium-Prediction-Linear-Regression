# Health-Insurance-Premium-Prediction-Linear-Regression

**Goal of the project**: Analyze the relationship between explanatory variables like age, gender, BMI, smoking behavior, and response variable – premium amount charged to the policyholder. Further, perform F-test & t-test to determine which of the explanatory variables are significant predictors of the premium-amount charged.

• **Data Analysis**: Look for missing values, stratified samples from each of the 4 different regions to which policyholders belong to.

• **Data Visualization**: plots to visualize the spread of data for each of the individual variables. Pair-wise plots to visualize the relationship of each of the explanatory variables with the response variable – premium amount.

• **Regression Model for predicting premium**: Fit a multiple linear regression (MLR) model to the data and performed Regression Diagnostics to analyze the fit of the MLR model. Reviewed if the assumptions of linear regression model are met.

**Results**: The residual plot showed clusters across the residuals as well as fitted values. The histogram of residuals also seemed to deviate from normality and hence I concluded that the relationship between the explanatory and response variables is not linear. Therefore, subsequent global F-Test & t-test were not performed.

• Next Steps:
- I noticed a large difference in premium for smokers vs non-smokers during visualization – performed two-sample test of means to compare the average premium for smokers vs non-smokers. The 95% confidence interval is (21418.37, 24762.77) i.e, we are 95% confident that, at alpha=0.05, the true difference in average premium for smokers & non-smokers is between $21418.37 & $24762.77 (p-value = 2.2e-16).

- Since a linear model was not a good fit, a more complex model like k-nearest neighbors could be explored to analyze the fit.

