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
- Statistical tests
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

## Customer segmentation and Feature Engineering

Loyalty feature represents the percentage of each customer's life that they were customers. We can do this by dividing Tenure by Age. It devides into three bins - Low, Medium and High
<img width="401" height="269" alt="image" src="https://github.com/user-attachments/assets/a691bbcc-f833-4b20-8728-d294acf47ce6" />
<img width="601" height="151" alt="image" src="https://github.com/user-attachments/assets/bf88d2bb-d980-47bc-b9a3-8d7208b30df2" />


Age is the most importand churn predictor, these are the counts for different Age Categories
<img width="281" height="132" alt="image" src="https://github.com/user-attachments/assets/cb409c88-c269-41ca-b547-79c3099e8a1e" />

**Distribution Results:**
<img width="1589" height="790" alt="image" src="https://github.com/user-attachments/assets/78f34ce8-14a8-4089-b7ab-140a249cef19" />

| Age Category        | Retained (%) | Exited (%) |
|:--------------------|-------------:|-----------:|
| Young (≤29)         |       92.44  |      7.56  |
| Middle-aged (30–45) |       84.70  |     15.30  |
| Mature (46–60)      |       48.88  |     51.12  |
| Senior (60+)        |       75.22  |     24.78  |

Churn Rates by Age Category:

Young (≤29): 7.56% churn rate

Middle-aged (30–45): 15.3% churn rate

Mature (46–60): 51.12% churn rate

Senior (60+): 24.78% churn rate


**Balance Tier Distribution:**

Balance Ranges for Each Tier:
Zero: $0.00 - $0.00
Low: $3,768.69 - $100,169.51
Very High: $139,528.23 - $250,898.09
High: $119,852.01 - $139,496.35
Medium: $100,194.44 - $119,839.69

| Balance Tier | Customer Count |
|--------------|---------------|
| Zero         | 3,617         |
| Low          | 1,596         |
| Medium       | 1,596         |
| High         | 1,595         |
| Very High    | 1,596         |

**Risk Heatmap**
<img width="916" height="699" alt="image" src="https://github.com/user-attachments/assets/e324b101-c0f2-4b11-9381-333fe3efb626" />

Mature customers (46–60) show the highest churn risk across almost all balance tiers, with values above 50%, peaking at 62.75% in the Medium balance tier.

Senior customers (60+) also have relatively high churn risk, but slightly lower than mature customers (27–32%).

Younger customers (≤29) consistently show the lowest churn rates across balance tiers (as low as 3.89% in Zero balance).

## Statistical Tests

### T-test

Pairwise T-test is suitable here because we want to check if the average value in various age categories is **significantly** different between churned vs. non-churned customers.

<img width="480" height="572" alt="image" src="https://github.com/user-attachments/assets/3386feb7-e8d7-44fc-8697-a89c997227ed" />

The results show t-statistics of -33 to -90, which are HUGE - indicating extremely strong evidence that churn rates differ between age groups.

**What it means:**

- Age strongly predicts churn - different age groups have meaningfully different churn rates

- The differences are not random - they're statistically significant based on p-value = 0.0000 and < α

- We should tailor retention strategies by age group since their churn behavior differs substantially

### Chi-squared test

Let's observe categories. We can use a chi-square test to check whether categorical variables are related to churn.

**Based on geography:**
<img width="295" height="443" alt="image" src="https://github.com/user-attachments/assets/80d5b5f2-d680-4784-ac33-1048f3d8a62c" />

**Key Findings:**


Highly Significant Association (p-value: 0.0000)

- There's a statistically significant relationship between geography and churn

- The probability this pattern occurred by random chance is essentially zero


**Germany Churn requires attention**

- Germany: 32.44% churn rate (almost 1 in 3 customers leave) - German customers are churning at DOUBLE the rate of other countries

- France and Spain have Similar Patterns, both have around 16% churn rate (industry average for banking), and no significant difference between these two markets

** Test for Loalty category**
<img width="361" height="167" alt="image" src="https://github.com/user-attachments/assets/89d0ea84-3e5d-42d8-b475-024f07dc8955" />

p-value < 0.05 → There’s a significant relationship between loyalty category and churn - inactive members leave more often.

