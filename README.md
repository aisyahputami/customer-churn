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
