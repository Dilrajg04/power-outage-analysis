# Analyzing Major Power Outages in the United States
By Dilraj Grewal  

## Introduction
This project focuses on exploring power outages across the United States to discover patterns and factors that affect outage duration. The dataset covers different outage causes, climate conditions, outage durations, and how they affect customers. Furthermore, the project utilizes predictive models to estimate outage duration based on the features stated before. 

### Key Features

The dataset includes features such as `OUTAGE.DURATION`, which measures how long an outage lasts, `CAUSE.CATEGORY`, which describes the reason for the outage, and `CLIMATE.CATEGORY`, which captures environmental conditions. Other variables like `CUSTOMERS.AFFECTED` and `TOTAL.SALES` provide information about the scale and impact of each outage.

The dataset contains **1534 rows and 57 columns**. Each row represents an outage event.


## Data Cleaning and Exploratory Data Analysis

<iframe src="assets/outages_over_time.html" width="800" height="500" frameborder="0"></iframe>

This plot shows how the number of major outages changes over time. There is a noticeable spike around 2011, suggesting an increase in major outages during that period.

<iframe src="assets/duration_distribution.html" width="800" height="500" frameborder="0"></iframe>

Most outages are relatively short, but a small number of extreme outages create a highly right-skewed distribution. To better visualize this pattern, a log transformation is applied to the outage duration.

<iframe src="assets/duration_by_cause.html" width="800" height="500" frameborder="0"></iframe>

This box plot shows how outage duration varies across different causes. Outages caused by severe weather tend to have longer durations and more extreme values compared to outages caused by other factors.


This plot reinforces the hypothesis test by showing that severe weather outages tend to have higher durations and a wider spread than other outage causes.

---

## Assessment of Missingness
<iframe src="assets/missingness_plot.html" width="800" height="500" frameborder="0"></iframe>

This plot shows that the missingness of `CUSTOMERS.AFFECTED` changes across cause categories, which supports the result that its missingness depends on `CAUSE.CATEGORY`.
The missingness in CUSTOMERS.AFFECTED is MNAR as the amount of affected customers may be dependent on the scale of the outage. Smaller outages may lead to reports of only a few customers affected, while the larger outages may cause a flow of significant reports. According to the permutation test results, the missingness of CUSTOMERS.AFFECTED depends on the CAUSE.CATEGORY and not the YEAR.


## Hypothesis Testing
Null Hypothesis: Outages caused by severe weather have the same average duration  

Alternative Hypothesis: Outages caused by severe weather have longer average durations  

Test Statistic: Difference in mean  
(Mean of outages caused by severe weather − Mean of outages caused by other causes)  

Significance Level: 0.05 

The difference in mean outage duration is 2537 minutes.

p-value: Approximately 0.0

Since the p-value is below the 0.05 significance level, we reject the null hypothesis. The results provide strong evidence that outages caused by severe weather tend to be longer in duration than outages caused by other factors.

---

## Framing a Prediction Problem
The goal is to predict `OUTAGE.DURATION`, which requires this to be a regression problem since duration is a continuous variable. Our response variable is going to be `OUTAGE.DURATION`, which will allow us to generalize the severity of the outage and help us predict how long the outages may last. The goal is to compute the RMSE since it measures the mean magnitude of prediction errors in the same units as the response variable. Features such as `CAUSE.CATEGORY`, `YEAR`, and `CLIMATE.CATEGORY` are going to be used for the prediction, as they are most likely to be available at the time of an outage.

---
## Baseline Model
The baseline model predicts OUTAGE.DURATION using features like CAUSE.CATEGORY and YEAR. Since CAUSE.CATEGORY is a categorical feature, we use one-hot encoding to extract meaningful value for our model. My project utilizes a sklearn pipline to perform linear regression and analyize performance through RMSE scores on the test set.

Current Training RMSE: **4887.97**

Current Test RMSE: **7161.60**

## Final Model
<iframe src="assets/predicted_vs_actual.html" width="800" height="500" frameborder="0"></iframe>

This plot compares the model’s predicted outage durations to the actual outage durations. While the predictions are not perfect, the final model captures the overall pattern much better than the baseline model.
The final model achieves a training RMSE of **3179.62** and test RMSE of **3240.79**.

The new model uses grid search to find the best hyperparameters, max_depth** = **5 and n_estimators** = **100**. Furthermore, the added features (`CLIMATE.CATEGORY`, `CUSTOMERS.AFFECTED`, and `TOTAL.SALES`) are useful to understand outage duration, as they are likely to be influenced by environmental conditions, scale of the outage, and local electricity demand. Since both of the outputs are closer than before, the model is most likely not overfitting.

## Fairness Analysis
The fairness analysis will consist of two groups. Group X will be outages caused by severe weather and Group Y will be outages caused by other factors. We are using RMSE as a metric for this regression problem.

Null Hypothesis: The final model treats all causes of the outages the same, and any difference is due to random chance  

Alternative Hypothesis: The final model is unfair, and RMSE for weather-related outages is higher than outages caused by other factors  

Test Statistic: Difference in RMSE (Outages caused by severe weather − Outages caused by other factors)  

Significance Level: **0.05**  

Observed difference in RMSE is approximately: **1023.75**

Approximate p-value: **0.109**

Since the p-value is greater than our significance level, we fail to reject the null hypothesis. This suggests that any observed difference is likely due to random chance, and the model does not appear to systematically disadvantage either group.
