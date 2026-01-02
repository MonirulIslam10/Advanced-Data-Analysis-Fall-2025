# Stroke Risk Prediction 

## Project Overview 
This project investigates whether demographic and physiological factors can predict the likelihood of a stroke using logistic regression modeling. The analysis emphasizes interpretability, proper validation, and robust evaluation under class imbalance. The study compares a baseline logistic regression model with a refined model including interaction terms, evaluating predictive performance and statistical significance using multiple metrics.

## Dataset 
Observations: ~43,400 initial patients; reduced to 29,072 after excluding records with missing values.

Target Variable: Stroke occurrence (binary)

Key Covariates:
- Demographics: age, gender, marital status, residence type, work type
- Health indicators: hypertension, heart disease, BMI, average glucose level
- Behavioral factor: smoking status

## Exploratory Data Analysis
EDA was conducted to understand distributions and detect skewness, imbalance, and potential outliers:
- Age: Fairly symmetric distribution with a mean in the late 40s.
- BMI & Glucose Levels: Both variables are right-skewed, indicating the presence of outliers.
- Stroke Outcome: Highly imbalanced with 28,524 "No Stroke" cases vs 548 "Stroke" cases.
- Hypertension & Heart Disease: Low prevalence in the dataset, but retained for their influence on stroke risk.

## Modeling 
### Baseline Regression Model 
- Included all covariates to establish baselines performance. 
- Assessed predictor significance using Type II ANOVA.
  - Age, hypertension, heart disease, and average glucose levels were statistically significant ($p < 0.05). 
- 10-fold stratified cross-validation was used to prevent data leakage and maintain class proportions.
  - Continuous predictors were standardized within each fold. 
- Classification threshold reduced from 0.5 to 0.10 to address class imbalance. 

### New Regression Model 
- Retained relevant covariates. 
- Included interaction terms of interest:
  - Age × Hypertension
  - Age × Heart Disease
  - Age × Smoking Status

Odds Ratio Analysis
- Age is the primary driver of stroke risk; in the new model, a one standard deviation increase in age is associated with 5.14 times higher odds of a stroke.
- In the base model, individuals with hypertension have 53% higher odds, and those with heart disease have 78% higher odds of a stroke.
- The interaction terms (Age:Hypertension and Age:Smoking) yielded odds ratios less than one, suggesting these factors have stronger relative effects at younger ages.

## Evaluation Results 
- **Baseline Model**
  -  Accuracy: 95.99%
  -  Specificity: 97.51%
  -  Sensitivity: 16.46%
  -  AUC: 0.8396
  -  AIC: 3234.57
  -  BIC: 3353.38

- **New Model**
  -  Accuracy: 95.94% 
  -  Specificity: 97.48% 
  -  Sensitivity: 15.85% 
  -  AUC: 0.8414 
  -  AIC: 3210.30 
  -  BIC: 3297.43

Since the new regression model has a lower AIC, BIC, and a slight improvement in AUC, it is preferred over the base model.

 ## Bootstrap Procedure 
- 5,000 bootstrap replications sampled with replacement from the test set.
- Mean sensitivity was 0.158 with a 95% confidence interval of [0.103, 0.22].
  - The wide interval and low mean highlight that the model is highly sensitive to the chosen threshold and class imbalance.

## Limitations & Future Work
- The low sensitivity (failure to identify true stroke cases) makes the current model insufficient for clinical diagnosis.
- Performance is extremely sensitive to the 0.10 threshold. 
- Future improvements:
  - Trial alternative cutoffs to optimize the sensitivity-specificity balance.
  - Apply resampling techniques such as down-sampling the majority class or SMOTE during training.
  - Utilize more advanced machine learning models to capture non-linear patterns.
 
