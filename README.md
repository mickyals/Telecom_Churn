# Telecom Churn Prediction
> ### Problem Statement
In the telecom industry, customers are able to choose from multiple service providers and actively switch from one operator to another. In this highly competitive market, the telecommunications industry > > experiences an average of 15-25% annual churn rate.
Given the fact that it costs 5-10 times more to acquire a new customer than to retain an existing one, customer retention has now become even more important than customer acquisition.
For many incumbent operators, retaining high profitable customers is the number one business goal. To reduce customer churn, telecom companies need to predict which customers are at high risk of churn. In this project, you will analyze customer-level data of a leading telecom firm, build predictive models to identify customers at high risk of churn.
In this competition, your goal is to build a machine learning model that is able to predict churning customers based on the features provided for their usage.


> ### Objectives
The main goal of this case study is to develop machine learning models for predicting churn. The predictive models have the following purposes.
- Predicting whether a high-value customer is likely to churn in the near future (i.e., churn phase). This information can assist the company in taking proactive measures such as offering special plans or discounts on recharge to retain customers.
-  Identifying significant variables that strongly indicate churn. These variables can provide insights into the reasons why customers choose to switch to other networks.

Based on observations, strategies for managing customer churn are recommended.

> ### Contents

 | Filename | Description | Format |
 |----------|-------------|--------|
 |Telecom_Churn_Prediction_MA | notebook  |ipynb |
 |data_dictionary | meta data | csv |
 |sample | example submission | csv |
 |test | test dataset | csv |
 |train | train dataset | csv |


> ##### Summary of Findings

After numerous iterations of models and changes below will be a short summary of my findings.

The dataset was very imbalanced with the majority of the dataset having missign values. These values were imputed based on the assumption that NaN for fields relating to services requiring mobile data were in response to clients not having data service during that month which also allowed for the creation of new binary attributes that indicate whether data was on or off that month. For those fields with missing values related to calling and recharges, the assumption was made that if the client did not use the service therefore those values fopr usage were imputed with zeros. This actually meant that many fields held lots of zeros which means they did not have large amounts of information. Which is likely why 95% of variance is held in about 75 attributes out of the total 160. 

While PCA did help to see this, it did not improve predictive power. Only Support Vector Classifiers performed better with PCA but all other models especially the tree models suffered reductions in predictive power. Of additional note is that with PCA the logistic regression can be conducted much faster but its performance was still below that of the non PCA (original) data. 

Opting not to use PCA, it was already assumed that a tree model would likely give the best results. Here the best models were the gradient boosted models of HistGradientBoosting, XGBoost and CatBoost performed best (see their results in the image below).

These three models were evaluated against the real world data and the XGBoost and HistGradientBoosting models performed nearly identically with the XGBoost winning out in testing however, my best submission with a score of 0.94286 was made using the HistGradientBoostingClassifier. However, XGBoost has the ease of finding significant feature so this notebook uses it. It is very likely that with better imputation and a few more derived features that the model could be improved. My goal here was not only selecting a model with high accuracy for evaluation but a model with high precision, recall and ROC AUC. This is because it allows me to know that the model is also correctly assigning True Positive and True Negatives to the correct class and addtionally the false positive rate and true positive rates remained high. This means the model is robust, generalisable and capable of being used not only for accuracy of results but could be used to accurately identify clients who are more likely to churn that not. We can be confident that with the high recall we are finding most true churns and the high precision means that we are not likely to identify a non churn as a churn. 

Since there was no PCA used and we opted for a fonal model in XGBoost, we could identify the most importance features. In this dataset, it appears that drops in usage in the month of August whether that be data or calling was a strong indicator of churn. 8 of the 10 top features related to August and mobile calling and data usage (see table below).

|   Feature       |   Importance  |
|-----------------|---------------|
| total_ic_mou_8  |   0.136899    |
| loc_ic_mou_8    |   0.135123    |
| has_data_8      |   0.054572    |
| fb_user_8       |   0.053484    |
| roam_og_mou_8   |   0.052684    |
| sachet_2g_6     |   0.032284    |
| total_rech_data_8 | 0.025800    |
| has_data_7      |   0.022668    |
| days_since_rech_8 | 0.022214    |
| roam_ic_mou_8   |   0.020717    |


Total incoming call minutes in the month of august was the biggest factor and when you think about it, it makes sense. If a client will be leaving at the end of august then their usage will likely diminish significantly as they begin to transfer to another provider. This may also hold true for other months but without annual data, this can not be confirmed. 


- Final Classification Report

|           | Precision | Recall | F1-Score | Support |
|-----------|-----------|--------|----------|---------|
| not churn |    0.97   |  0.97  |   0.97   |  12552  |
|   churn   |    0.97   |  0.97  |   0.97   |  12595  |
|-----------|-----------|--------|----------|---------|
|  Accuracy |           |        |   0.97   |  25147  |
|-----------|-----------|--------|----------|---------|
| Macro Avg |    0.97   |  0.97  |   0.97   |  25147  |
|Weighted Avg|   0.97   |  0.97  |   0.97   |  25147  |


> ### Citation

 <p>Tannu Jain, Upgrad. (2023). Telecom Churn Case Study Hackathon. [Online]. Available: <a href="https://kaggle.com/competitions/telecom-churn-case-study-hackathon-c48">https://kaggle.com/competitions/telecom-churn-case-study-hackathon-c48</a></p>
