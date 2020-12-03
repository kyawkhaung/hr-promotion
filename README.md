# HR Analytics - Will you be nomiated for promotion this year?
<img src="https://github.com/kyawkhaung/hr-promotion/blob/main/images/hr_promotion_image.jpg" width="200" height="100">

This is binary classification problem that [Analytics Vidhya](https://datahack.analyticsvidhya.com/) hosted for competition in 2018.

### Problem Statement

The objective is to predict whether a potential promotee at checkpoint in the test set will be promoted or not after the evaluation process. Detail information can be accessed [here](https://datahack.analyticsvidhya.com/contest/wns-analytics-hackathon-2018-1/#ProblemStatement)

Evaluation metric used in the competition is <b>F1 Score</b>.

### Dataset Features
Train Dataset: 54808<br/>
Test Dataset: 23490
<br/>
|     Variable Name     |     Description     |
|--------------------|--------------|
|     employee_id     |     Unique ID for employee      |
|department|	Department of employee|
|region	|Region of employment (unordered)|
|education	|Education Level|
|gender	|Gender of Employee|
|recruitment_channel	| Channel of recruitment for employee|
|no_of_trainings|	no of other trainings completed in previous year on soft skills, technical skills etc.|
|age	|Age of Employee|
|previous_year_rating	|Employee Rating for the previous year|
|length_of_service	|Length of service in years|
|KPIs_met >80%	|if Percent of KPIs(Key performance Indicators) >80% then 1 else 0|
|awards_won?	|if awards won during previous year then 1 else 0|
|avg_training_score	|Average score in current training evaluations|
|is_promoted	|(Target) Recommended for promotion|

### My Findings
As feature engineering, I have used one hot encoding for independent variables - Department, Region, and Gender because I do not want to any value of an individual feature gets influenced due to their higher ranking. I have treated education differnt from the rest of cateogrical data types because it is logical to create assigning ranking order because master degree holder should have given more weight compared to bachelor degree holder. Other feature categories do not  Those missing values are imputed for mean value from both train and test datasets. When building with Random Forest and XGBosst together with hyperparameter tuning, observed that F1 scroe is not good at performing and managed to get 0.388, which indicates that model is underfitting and not learning at all. 

Then, I discarded one hot encoding; intead, I used label encoding. Random Forest and XGboost seems to handle pretty good with my input data and F1 score went up to 0.49. I have used Random Forest hyperparameter tuning and Xgboost hyperparamter tuning. Observed that XGboost has shown better prerformance at predicting target values. I also tried with ensembeling method of stacking these two models, which still can't perform better than xGboost alone. 

|     Model     | Final F1 Score |     Before Hyperparameter Tuning     | After Hyperparameter Tuning|
|-------------------|---------------|--------------|--------------------|
|Random Forest |0.47 | ![Random Forest](https://github.com/kyawkhaung/hr-promotion/blob/main/images/rf_cm.png)| ![Random Forest](https://github.com/kyawkhaung/hr-promotion/blob/main/images/rf_cm_hp.png)|
|XGBoost |0.51| ![Random Forest](https://github.com/kyawkhaung/hr-promotion/blob/main/images/xgb_cm.png) | ![Random Forest](https://github.com/kyawkhaung/hr-promotion/blob/main/images/xgb_cm_hp.png)|

I believe that dataset has somewhat not cohesive and consistent in terms of mapping with independent vairables and target variable. When performing data exploration (see [Actual Code](https://github.com/kyawkhaung/hr-promotion/blob/main/Data%20Exploration.ipynb)), observed that there is no strong correlation among independent variables and target variable.

Below table summarizes the numberical variables correlation.

![Correlation Table](https://github.com/kyawkhaung/hr-promotion/blob/main/correlation_table.png)
#### Correlation with respect to "is_promoted"
- correlation between "award_won?" and "is_promoted" is 0.19 <br/> 
- correlation between "avg_training_score" and "is_promoted" is 0.18<br/>
- correlation between "KPIs_met>80%" and "is_promoted" is 0.22<br/>
- correlation between "previous_year_rating" and "is_promoted" is 0.15<br/>

#### Correlation among Independent Features
- correlation between "award_won?" and "avg_training_score" is 0.072 <br/> 
- correlation between "KPIs_met>80%" and "previous_year_rating" is 0.351<br/>
- correlation between "avg_training_score" and "previous_year_rating" is 0.075<br/>

This showed that those input features can't influence to target "is_promoted" or among each other. This lead to poor performance during model training. A few things can be improved in order to increase F1-score. 
1. Perform oversampling - add more data points to miniorty class
2. Add more complexity on data - generate more data fields
3. Correct bad data in existing dataset
