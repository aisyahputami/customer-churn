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



![replace](https://github.com/aisyahputami/customer-churn/blob/main/eda/replace-marital-status.png)

![new](https://github.com/aisyahputami/customer-churn/blob/main/eda/new_marital-status.png)

![replace](https://github.com/aisyahputami/customer-churn/blob/main/eda/replace-income-category.png)

![new](https://github.com/aisyahputami/customer-churn/blob/main/eda/new_income-category.png)

### Multivariate Analysis
