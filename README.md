# Customer Behavior and Predicting Churn

## Scenario
You work as a data analytics engineer for a Ritz Jager Bank, analyzing customer behavior and predicting churn. The dataset provides information on existing credit card customers, including demographic data, financial behavior, and interaction history. Your goal is to build a predictive model to identify customers likely to churn (Attrition_Flag = "Attrited Customer") based on their attributes and behavior. This model will help the bank retain valuable customers and reduce customer churn.

## Connect Database to Dataiku
First, let's create a project named Weekly.
![project](https://github.com/aisyahputami/customer-churn/blob/main/connect-dataset/create-project.png)

Then, due to license limitations, we **connect the dataset** used **by uploading the CSV file**. Go to our project and click **Import Your First Dataset**. After that, choose the CSV file used. In this case, we use the BankChurners file.

![dataset](https://github.com/aisyahputami/customer-churn/blob/main/connect-dataset/import-dataset.png)

![upload file](https://github.com/aisyahputami/customer-churn/blob/main/connect-dataset/upload-file.png)

Ensure that the dataset used is correct, then click **Create**.
![create](https://github.com/aisyahputami/customer-churn/blob/main/connect-dataset/preview-dataset.png)


## Data Quality Report
The main purpose of a data quality report is to evaluate, identify, and communicate the level of reliability, accuracy, completeness, and consistency of the existing data. In this case, several indicators will be identified as samples of dataset quality. These indicators include:

**1. No empty values exist in all columns**
![1](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-1.png)

   Ensuring that there are no empty values in all columns of the data is important for data quality because it ensures that the data used for analysis or decision-making does not have gaps or missing information that could affect the validity and accuracy of the results.

**2. All values of CLIENTNUM are unique**
![2](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-2.png)

   Every unique CLIENTNUM value serves as a distinct identifier for each individual customer or client. This uniqueness ensures that each customer's account or relationship with the bank can be accurately distinguished and tracked. Additionally, having unique CLIENTNUM values helps maintain the integrity of the dataset. It prevents duplication or confusion of customer records, which could lead to errors in analysis or decision-making.
   
**3. Min of Customer_Age is above 25**
![3](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-3.png)

   By considering the minimum age value of customers above 25 years in churn analysis, banks can identify more stable customer segments, prioritize retention efforts in these segments, and develop more effective marketing strategies to reduce churn.
   
**4. The values ​​of the Attrition_Flag column are only two, namely Attrited Customer and Existing Customer**
![4](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-4.png)

   It is important that the values of the Attrition_Flag column are only two, namely "Attrited Customer" and "Existing Customer," because it simplifies the analysis and classification of customers into distinct groups based on their churn status. Having only two categories makes it easier to understand and interpret the data, and allows for straightforward comparison between customers who have churned (Attrited Customer) and those who are still active (Existing Customer).
   
**5. Min of Months_on_book is above 12**
![5](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-5.png)

   Overall, ensuring that the minimum value of Months_on_book is above 12 enables banks to focus their efforts on analyzing and serving their most valuable and loyal customers, leading to improved customer retention, satisfaction, and profitability.
   
**6. Max of Credit_Limit is below 35000**
![6](https://github.com/aisyahputami/customer-churn/blob/main/data-quality/data-quality-6.png)

   It is important that the maximum value of Credit_Limit is below 35000 because it helps in understanding the credit risk exposure of the bank and the financial stability of its customers.


## Exploratory Data Analysis (EDA)
### Univariate Analysis
#### Marital Status
![marital](https://github.com/aisyahputami/customer-churn/blob/main/eda/marital_status.png)

There are **749 "unknowns" out of 10,000** records. This is significant enough that we shouldn't just drop these records, we should try to fill these. Let's use the strategy to **impute using the most common category** (Married).
 
#### Income Category
![income](https://github.com/aisyahputami/customer-churn/blob/main/eda/income_category.png)

There are **even more "unknown" salary** (1112). For now we will **impute this simply using the "most_common" category** (Less than $40k). However income is likely to be quite linked to other features. With more time we could impute this by grouped information on some of these demographic features. For future, in case we would come back to change this imputation strategy, let's identify some features we would group by. Plot distributions vs income.

#### Education Level
![education](https://github.com/aisyahputami/customer-churn/blob/main/eda/education-level.png)

There are **15% "unknown" eduaction level** (1519). For now we will **impute this simply using the "most_common" category** (Graduate).

#### Age
![age](https://github.com/aisyahputami/customer-churn/blob/main/eda/customer-age-distribution.png)

From these statistics, we can see that **the age distribution tends to be centered around the mean value (46.33 years) and the median (46 years)**, with a relatively low standard deviation (8.02 years). This indicates that the majority of the data falls within a relatively close range from the mean, although there is significant variation in age from the minimum value (26 years) to the maximum (73 years).

#### Credit Limit
![credit](https://github.com/aisyahputami/customer-churn/blob/main/eda/credit-limit-distribution.png)

From these statistics, we can observe that **the credit limit distribution is characterized by a wide range of values**, as evidenced by the large standard deviation (9088.78). Both the mean (8631.95) and the median (4549) indicate that there is a substantial difference between the average and typical credit limits. The minimum credit limit is 1438.3, while the maximum is 34516, illustrating significant variability in credit limits among individuals. Additionally, the number of distinct values (6205) suggests that there are many unique credit limits in the dataset, further highlighting the diversity in credit limits among the population.

#### Attrition Flag
![flag](https://github.com/aisyahputami/customer-churn/blob/main/eda/attrition-flag-distribution.png)

From this statistics, we can see that the **majority of customers (84%)** in the dataset **are "Existing Customer"** while **the rest (16%) are "Attrited Customer"**.

When we train a prediction model using a dataset with such a significant class imbalance, the model tends to predict the majority class more often than the minority class. This can lead to poor model performance in identifying rare or minority cases.

Therefore, special handling is needed, such as oversampling or undersampling techniques, to balance these classes during model training. This will help prevent the model from leaning too much towards the majority class and allow the model to learn patterns from both classes more effectively.

To simplify the analysis process, we can **replace the "attrition_flag" column with a boolean column called "churn"** where **"True" represents "Attrited Customer" and "False" represents "Existing Customer"**.

![churn](https://github.com/aisyahputami/customer-churn/blob/main/eda/replace-to-churn.png)

### Multivariate Analysis
#### Income Category by Age
![income](https://github.com/aisyahputami/customer-churn/blob/main/eda/income-category-by-age-distribution.png)

In summary, the distribution indicates that **the majority of customers have medium to low incomes**, with a few having high incomes. Additionally, **income tends to increase with age but remains dominant in the middle-income category**.

1. **General Pattern**: The majority of customers have incomes less than $40K in each age range.
2. **Income Increase Trend**: There is a trend of increasing income with age. For example, in the age range [20,32), the number of customers with incomes less than $40K is 232, while in the age range [44,56), the number of customers with incomes less than $40K increases to 2302.
3. **Middle-Class Income Pattern**: Income ranges between $40K and $120K are the most common in each age range, although the numbers can vary. This indicates that most customers fall into the middle-income category.
4. **High Income**: Although fewer in number, there are customers with incomes over $120K, especially in older age ranges (above 44 years).


#### Income Category by Credit Limit
![credit](https://github.com/aisyahputami/customer-churn/blob/main/eda/income-category-by-credit-limit-distribution.png)

The distribution of "Income_Category" by "Credit_Limit" reveals how credit limits vary across different income brackets. Here are some insights we can gather from the distribution:

1. **Credit Limit Increase with Income**: Generally, there is an increasing trend in credit limits as income categories rise. For example, in the income category "Less than $40K," the majority of customers have credit limits in the range [0,7000), while in the income category "$120K +," the majority have credit limits in the higher ranges, such as [28000,35000).
2. **Middle-Income Credit Limits**: In the middle-income categories ($40K - $120K), the distribution of credit limits is relatively balanced across different ranges, indicating that customers in these income brackets have varying credit needs.
3. **Low Credit Limits for Lower Incomes**: Customers in the "Less than $40K" income category predominantly have lower credit limits, with the majority falling in the range [0,7000).
4. **High Credit Limits for Higher Incomes**: Conversely, customers in higher income categories tend to have higher credit limits, with a significant portion falling into the higher credit limit ranges ([14000,21000) and above).

In summary, the distribution suggests that **credit limits tend to increase with income**, with **lower-income individuals having lower credit limits and higher-income individuals having higher credit limits**. There is also variability within middle-income categories, reflecting diverse credit needs among customers with similar income levels.

#### Correlation Matrix
![metrix](https://github.com/aisyahputami/customer-churn/blob/main/eda/correlation-matrix.png)

**Summary of correlations:**
- Strong correlation coeffs observed between:
  - months_on_book vs customer_age
  - total_trans_ct vs total_trans_amt
  - total_trans_amt vs total_relationship_count
  - total_trans_ct vs total_relationship_count
  - avg_open_to_buy vs credit_limit 
    - **Very co-linear**, makes sense as customers with high credit-limit are probably the ones who are quire open to having even more credit.*
  - avg_utilisation_ratio vs credit_limit
  - avg_utilisation_ratio vs total_revolving_bal
  - avg_utilisation_ratio vs avg_open_to_buy
  - total_ct_chng_q4_q1 vs total_amt_chng_q4_q1
    - These 2 features are inter-related, so makes sense to be highly positive correlation.
  - total_transaction_ct vs total_trans_amt
    - Again these 2 features are inter-related, so makes sense to be highly positive correlation.

**Top few correlations vs churn (sorted by coefficient):**
- total_trans_ct (-ve correlation)
- total_ct_chng_q4_q1 (-ve correlation)
- total_revolving_bal (-ve correlation)
- contacts_count_12_mon (+ve correlation)
- avg_utilization_ratio (-ve correlation)
- months_inactive_12_mon (+ve correlation)
- **Summary**
  - Negative correlations with features that represent "usage of credit".
  - Positive correlations with features that represent "lack of usage of credit".

    
For clarity, print out a list of correlation coeffs vs the dependent variable "churn"


#### Customer Churn by Card Category
![card](https://github.com/aisyahputami/customer-churn/blob/main/eda/customer-churn-by-card-category-distribution.png)

The distribution of customer churn by card category provides an overview of the number of customers who churned in each card category. Here is a summary of the distribution:
1. **Blue card category**: has a churn rate of 15.94%
2. **Silver Card Category**: has a churn rate of 14.04%
3. **Gold Card Category**: has a churn rate of 18.02%
4. **Platinum Card Category**: has a churn rate of 25%

From the data, we can conclude that **the churn rate tends to increase with the higher level of exclusivity or card status**, with the highest churn rate occurring in the Platinum card category (25%). Although this contradicts the common assumption that cards with higher status will have lower churn rates, in this context, customers with Platinum cards appear to have a higher churn rate.

This may be due to several factors, such as higher expectations from customers with higher status, higher fee requirements, or changes in customer preferences regarding the benefits offered by the card.

#### Customer Churn by Education Level
![education](https://github.com/aisyahputami/customer-churn/blob/main/eda/customer-churn-by-education-level-distribution.png)

From the data, we can conclude that there is variation in churn rates among different education levels. Although no specific education level consistently has a higher or lower churn rate than others, there are some interesting findings:

1. "**Doctorate**" education level has the highest churn rate at 20.72%.
2. "**Post-Graduate**" education level has a churn rate of 17.70%.
3. "**Uneducated**" education level has a churn rate of 15.85%.
4. "**Graduate**" education level has a churn rate of 15.82%.
5. "**College**" education level has a relatively low churn rate at 15.04%.
6. "**High School**" education level has a churn rate of 14.92%.

Although there is no clear pattern where higher education levels always result in lower churn rates, there are indications that customers with certain education levels may have a higher tendency to churn.

## Modelling
### Setup Analysis
First, let's create a "New Analysis" with the dataset used, namely **BankChurners_prepared**. This dataset has undergone transformation processes according to the needs outlined during the EDA process. Let's name it **ML Development**, then click Create Analysis.

![setup](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-1.png)

Next, create a "New Model Task" and select **AutoML Prediction**.
![setup](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-2.png)

Select the "churn" column as the target column and choose **Quick Prototypes**. Then, click Create.

![setup](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-3.png)

Before training the model, review the model design first.
![setup](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-4.png)

### Basic
#### Target
In the target section, make sure that the selected target column is "churn" and the Prediction Type is **Two-class Classification**.

![target](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-5.png)

It seems that the "churn" column has an imbalanced distribution, with "Yes" accounting for 16% and "No" for 84%. This imbalance can affect the model's performance. However, based on the information provided in the [Dataiku documentation](https://doc.dataiku.com/dss/latest/machine-learning/supervised/settings.html?_gl=1*3rkwkw*_ga*MTkxNDg5MjQ2Ni4xNzEyNTE0NTYy*_ga_B3YXRYMY48*MTcxNDk5MzMyNi40LjEuMTcxNDk5MzUyNy40OS4wLjA.#setting-weighting-strategy), the **class weights** feature can address the imbalance issue in the data.

![imbalance](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-6.png)

#### Train / Test Set
In this section, select "Split the dataset". Don't forget to **activate K-fold cross-test** and choose **the number of folds as 4**. The number 4 means the dataset will be divided into 4 equally sized parts and will undergo 4 iterations where each part is used as the testing data once. This helps in obtaining a more stable estimate of our model's performance.

Next, input **1337 as the splitting random seed**. This means that 1337 is the seed value used in the process of dividing the data into subsets for training and testing. This seed is used to control the randomness of the data split. So, if we use the same seed value (in this case "1337") in the data splitting process, we will get the same split each time we run the process.

![train](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-7.png)

Also, don't forget to **activate Stratified**. In the context of data splitting or model testing, stratified aims to maintain the same class proportions in each fold when performing cross-validation. This is important especially when you have class imbalance in your data.

In cross-validation without stratification, there's a chance that one or some folds may have significantly different class proportions than others. This can lead to inconsistent and even biased model evaluations, especially if you have important minority classes in your dataset.

#### Metrics
![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-8.png)

The **use of Recall as a metric for finding the best model during hyperparameter optimization and KFold cross-validation** serves several purposes:

1. **Addressing class imbalance**: When the dataset has an imbalance between positive and negative classes, as in this case, metrics like accuracy alone may not provide an accurate picture of the model's performance. Recall provides information about the model's ability to identify all instances of the positive class (true positive rate), thus helping to assess the overall model performance, especially if the minority class is considered important.
2. **Focus on detecting positive cases**: In this case, we focus on the model's ability to detect positive cases (true positives). This is because **accurately detecting customers who will churn allows the marketing team to create business strategies to prevent it**. Recall provides a measure of how well the model can find all positive cases in the dataset.
3. **Avoiding false negatives**: False negatives (positive cases incorrectly classified as negative) can have serious consequences. In this case, customers who churn but are predicted not to will result in losses for the company. This is because **the cost of acquiring customers is higher than the cost of retaining** them. Using recall as an evaluation metric helps reduce these errors because models with high recall are more likely to avoid false negatives.

So, in the context of hyperparameter optimization and KFold cross-validation, the use of recall helps evaluate the model's performance more holistically, especially in cases of class imbalance and when focusing on detecting important positive cases.

After that, **using Accuracy as a metric to find the best threshold after building a model primarily** aims to evaluate how well the model can correctly classify both classes, positive and negative, at a single time.

### Features
#### Features Handling
![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-9.png)

In this section, we can choose which columns will be used as features. In this case, **we will use all columns except for the columns CLIENTNUM, Attrition_Flag, Naive_Bayes_Classifier_Attrition_Flag_Card_Category_Contacts_Count_12_mon_Dependent_count_Education_Level_Months_Inactive_12_mon_1, and Naive_Bayes_Classifier_Attrition_Flag_Card_Category_Contacts_Count_12_mon_Dependent_count_Education_Level_Months_Inactive_12_mon_2**. 

### Modelling
#### Algorithms
![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling-10.png)

By using a combination of Algorithm such as Random Forest, Logistic Regression, LightGBM, and XGBoost, we can benefit from model diversity and maximize the potential for accurate predictions and good generalization.

#### Save and Train
Click on Save and then proceed to run the Train process. This step typically takes around 3-5 minutes to complete.

![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/modelling_11.png)

## Result
The **best-performing model obtained is Random Forest** with a **Recall value of 90.5%**.
![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/result.png)

In addition, let's examine the performance of each model based on the confusion matrix values. As mentioned earlier, the focus of the model performance we expect is to **minimize the false negatives and maximize the positive cases**. Based on the four models used, it is evident that **Random Forest is the best-performing model that fulfills the research assumptions**.

![metrics](https://github.com/aisyahputami/customer-churn/blob/main/modelling/model.png)
