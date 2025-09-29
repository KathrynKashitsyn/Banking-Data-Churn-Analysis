# Customer Churn Analysis & Prediction

## Project Overview

This project analyzes customer churn for a European bank which has operations in France, Germany and Spain 
The goal is to identify key factors driving customer churn and to build a predictive model that can flag customers at high risk of leaving.

Customer churn is a major challenge in subscription-based businesses. We define it as follows: customer churn occurs when a client who was using our services stops doing business with the bank. 

The ultimate business objective is to reduce churn rate by enabling proactive retention campaigns.

## Key Questions this analysis aims to answer:

- Which customer segments have the highest churn rate and risk?
- What are the most significant predictors of customer churn?
- What is the expected performance and business value of the churn prediction model?

## Workflow

In this project, I analyzed a customer dataset to uncover patterns behind churn and build predictive models to identify customers at risk of leaving.  The project includes:

- Data cleaning & preprocessing
- Exploratory data analysis (EDA)
- Feature engineering
- Machine learning modeling & evaluation
- Communicating actionable business insights

## Dataset and Metadata
The dataset contains:

**10,000 rows** – each row is a unique customer of the bank

**14 columns:**
| **Column name** | **Type** | **Description** |
| --- | --- | --- |
| RowNumber | int | Row numbers from 1 to 10,000 |
| CustomerId | int | Customer’s unique ID assigned by bank |
| Surname | str | Customer’s last name |
| CreditScore | int | Customer’s credit score. This number can range from 300 to 850. |
| Geography | str | Customer’s country of residence |
| Gender* | str | Categorical indicator |
| Age | int | Customer’s age (years) |
| Tenure | int | Number of years customer has been with bank |
| Balance | float | Customer’s bank balance (Euros) |
| NumOfProducts | int | Number of products the customer has with the bank |
| HasCrCard | bool | Indicates whether the customer has a credit card with the bank |
| IsActiveMember | bool | Indicates whether the customer is considered active |
| EstimatedSalary | float | Customer’s estimated annual salary (Euros) |
| Exited | bool | Indicates whether the customer churned (left the bank) |

## Numerical Data

<img width="1785" height="1025" alt="image" src="https://github.com/user-attachments/assets/eb373d9f-fa6c-4147-ab10-b3af2f161250" />
- **CreditScore** - Bell-shaped, slightly left-skewed distribution centered around 650-700 with a small spike around 900
- **Age** - Right-skewed distribution with median around customers aged 30-45
- **Tenure** - Nearly uniform distribution (1-9 years) with lower frequencies at 0 and 10 years
- **Balance** - Massive peak at zero balance (3,617 customers)
- **Number of Products** - Heavy concentration at 1-2 products with sharp drops at 3-4 products 
- **Salary** - Uniform distribution across all ranges

## Churned customers
<img width="1489" height="975" alt="image" src="https://github.com/user-attachments/assets/696095da-dee0-4742-b472-db19e9d55499" />

Since we are interested in churn, the numeric data for 'Exited' category reveals:
- **Credit Score** Very similar distributions between churned and retained customers
- **Age** - Churned customers are significantly older (median ~45) than retained customers (median ~35)
- **Tenure** -  Nearly identical distributions (both medians ~5 years).
- **Balance** - Churned customers have higher median balance (~110,000) than retained customers (~95,000), retained customers have a substantial zero-balance number shown in Q1.
- **NumOfProducts** - Similar distributions (both medians = 1 product). 
- **EstimatedSalary** - Virtually identical distributions (both medians ~100,000). 
