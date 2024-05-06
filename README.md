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
