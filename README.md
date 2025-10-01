# Customer Churn Analysis & Prediction

## Project Overview

This project analyzes customer churn for a European bank which has operations in France, Germany and Spain 
The goal is to identify key factors driving customer churn and to build a predictive model that can flag customers at high risk of leaving.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7438f028-d19a-4af8-b576-46dff1b99cb9" />


Customer churn is a major challenge in many businesses. We define it as follows: customer churn occurs when a client who was using our services stops doing business with the bank. 

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

## Tools & Libraries Used

1. Core Data Handling & Math

pandas → for data manipulation (DataFrame, .copy(), .describe(), .value_counts(), .qcut(), etc.)

numpy → for arrays, math functions (np.abs, .tolist() conversions, etc.)

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

## Modeling with Logistic Regression

Using 10 features for modeling: CreditScore, Geography, Gender, Age, Tenure, Balance, NumOfProducts, HasCrCard, IsActiveMember, EstimatedSalary

The selection covers all key customer dimensions: 
 Demographics (Age, Gender, Geography) 
 Financials (Credit, Balance, Salary) 
 Behavior (Tenure, Products, Activity) 
 Relationship (Engagement, Product holdings)
 
<img width="509" height="349" alt="image" src="https://github.com/user-attachments/assets/1a657261-c1d7-41fe-bdaf-3cd3721a633e" />

Accuracy: 0.8092 (81%)
→ Looks good at first, but it’s misleading here because most customers do not churn (class imbalance).

Precision: 0.5988 (60%)
→ When the model predicts “Churn,” it’s correct ~60% of the time.
→ So, more than half of flagged customers are indeed churners.

Recall: 0.1906 (19%)
→ The model only catches ~19% of actual churners.
→ This is very low: most churners are being missed.

F1-Score: 0.2891 (29%)
→ Low because recall drags it down.

ROC AUC: 0.7836 (78%)
→ Model is reasonably good at ranking churners higher than non-churners, but thresholding at 0.5 makes recall weak.

<img width="519" height="391" alt="image" src="https://github.com/user-attachments/assets/79f7036e-cabf-42e1-ae0e-bcfe52723ec9" />

** Confusion Matrix reveals:**

True Negatives (Stayed correctly predicted): 1926

False Positives (Predicted churn but stayed): 65

False Negatives (Missed churners): 412

True Positives (Correctly predicted churners): 97

👉 Out of 509 actual churners, the model only caught 97 (≈19%).


## Random Forest

<img width="331" height="430" alt="image" src="https://github.com/user-attachments/assets/091b958f-c136-407d-9db9-7ca77f000c96" />

<img width="683" height="500" alt="image" src="https://github.com/user-attachments/assets/70f5ad8a-c0da-4dd0-ac57-7870660b8d17" />

<img width="1489" height="490" alt="image" src="https://github.com/user-attachments/assets/41ecfbee-8be6-44ce-898b-002079ca377c" />


##  What drives churn?

<img width="446" height="338" alt="image" src="https://github.com/user-attachments/assets/4ea99d2d-f300-4454-8daa-84229ef83c1b" />

## Summary

Our analysis reveals significant revenue erosion due to customer churn, with an annual impact of $2.0M. Key risk segments have been identified, enabling targeted retention strategies with substantial ROI potential.

• Total Customer Base: 10,000
   • Current Churn Rate: 20.4%
   • Annual Revenue Loss: $2,037,000

**KEY INSIGHTS:**

   1. Mature customers (46-60) have 51% churn rate vs 20% overall
   2. German customers churn at 32% vs 16% for France/Spain
   3. Inactive members are 3x more likely to churn
   4. Customers with 1 product churn more than those with 2+ products


**Strategic Recommendations:**

Priority 1: High-Impact Retention Initiatives (Immediate)
- Segment-Specific Campaigns: Develop targeted programs for German customers and mature demographic (46-60 years)
- Proactive Reactivation: Implement immediate outreach to inactive customer segments
- Predictive Modeling: Deploy churn prediction system with 70% probability threshold for interventions

Priority 2: Medium-Term Enhancements (30-60 Days)
- Product Portfolio Strategy: Create age-specific bundled offerings
- Market-Specific Programs: Develop German market retention initiatives
- Monitoring Infrastructure: Establish real-time customer activity tracking

**Success Metrics**

Primary KPIs:

- Reduction in overall churn rate
- Increase in customer lifetime value (CLV)
- Model accuracy in production environment
- Return on investment for retention initiatives

Secondary Indicators:

- Improvement in customer engagement scores
- Growth in multi-product adoption
- Reduction in high-risk segment churn rates

