# power-outage-analysis
# Understanding Major Power Outages in the United States

## Introduction
This project focuses on exploring power outages across the United States to discover patterns and factors that affect outage duration. The dataset covers different outage causes, climate conditions, outage durations, and how they affect customers. Furthermore, the project utilizes predictive models to estimate outage duration based on the features stated before. 

The dataset contains 1534 rows and 57 columns. Each row represents an outage event.

## Data Cleaning and Exploratory Data Analysis

## Assessment of Missingness
The missingness in CUSTOMERS.AFFECTED is MNAR as the amount of affected customers may be dependent on the scale of the outage. Smaller outages may lead to reports of only a few customers affected, while the larger outages may cause a flow of significant reports. According to the permutation test results, the missingness of CUSTOMERS.AFFECTED depends on the CAUSE.CATEGORY and not the YEAR.

## Hypothesis Testing
Null Hypothesis: Outages caused by severe weather have the same average duration.

Alternative Hypothesis: Outages caused by severe weather have longer average durations.

Test Statistic: Difference in mean
(Mean of outages caused by severe weather - Mean of outages caused by other causes)

Significance Level: 0.05

## Framing a Prediction Problem
The goal is to predict OUTAGE.DURATION, which requires this to be a regression problem since duration is a continuous variable. Our response variable is going to be OUTAGE.DURATION which will allows us to generalize the severity of the outage and help us predict how long the outages may last for. The goal is compute the RSME since it measures the mean magnitude of prediction erros in the same units as the response variable. Features such as CAUSE.CATEGORY,YEAR and CLIMATE.CATEGORY are going to be used for the prediction as they are most likely to be available at the time of an outage.
## Baseline Model
Our baseline model outputs a 4887.97 for the training RMSE and 7161.60 for the test RMSE.
Since the model only uses two features and the test RMSE is still higher than we would like, we could try adding more relevant features to reduce that number.

WEBSITE: The baseline model predicts OUTAGE.DURATION using features like CAUSE.CATEGORY and YEAR. Since CAUSE.CATEGORY is a catigorical feature, we use one-hot encoding to extract meaningful value for our model. My project utilizes a sklearn pipline to perform linear regression and analyize performance through RMSE scores on the test set.

## Final Model
The final model achieves a training RMSE of 3179.62 and test RMSE of 3240.79.

The new model uses grid search to find the best hyperparameters, which are 5 for max_depth and 100 for n_estimators. Furthermore, the added features(CLIMATE.CATEGORY, CUSTOMER.AFFECTED, and TOTAL.SALES) are useful to understand outage duration as they are likely to be influenced by enviornmental conditions, scale of the outage and local electricty demand. Since both of the output are closer than before, the model is most likely not overfitting.
## Fairness Analysis
The fairness analysis will consist of two groups. Group X will be outages caused by severe weather and Group Y will be outages caused by other factors. We are using the RMSE as a metric for this regression problem.

Null Hypothesis: The final model treats all causes of the outages the same and any difference is by random chance

Alternative Hypothesis: The final model is unfair and RMSE for weather realted outages is higher than outages caused by other factors. 

Test Statistic: Difference in RMSE( Outages caused by severe weather - Outages caused by other factors)

Significance Level: 0.05

