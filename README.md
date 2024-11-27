# **Credit Card Fraud Transactions**

## **Objective**
This project aims to build a model to predict whether a transaction is fraudulent. We will evaluate and compare various models to determine which performs best on the dataset.

---

## **Contents**
1. [Introduction](#1-introduction)
2. [Dataset](#2-dataset)
3. [Analysis](#3-analysis)
4. [Models](#4-models)
5. [Results](#5-results)
6. [Conclusion](#6-conclusion)
7. [References](#7-references)

---

## **1. Introduction**
Worldwide credit card usage reached 2.8 billion users in 2019, with 70% owning at least one card. However, credit card fraud has been on the rise:
- Reports of fraud in the U.S. increased by **44.7%** from 271,927 in 2019 to 393,207 in 2020.
- Two major types of fraud:
  - **Identity Theft**: Creating credit card accounts in someone else’s name (48% increase from 2019 to 2020).
  - **Account Takeover**: Misusing an existing account (9% increase from 2019 to 2020).

This project aims to detect fraudulent transactions using machine learning. It will evaluate multiple models, focusing on their accuracy and interpretability. 

As a Data Scientist with FinTech experience, I’ve observed that fraud detection is a challenging problem due to fraudsters' ability to adapt. Moreover, FinTech regulatory requirements often favor interpretable models over black-box algorithms to maintain compliance and facilitate debugging. For this reason, this project focuses on interpretable models like:
- **K-Nearest Neighbors (KNN)**
- **Support Vector Machines (SVM)**
- **Logistic Regression**

Advanced models like Random Forest and Gradient Boosting may also be explored for comparison.

---

### **Why Fraud Transaction Detection Is Necessary**

Financial organizations and customers face **massive financial losses** on a regular basis due to fraudulent transactions. Even small fraudulent amounts per transaction can accumulate into substantial losses over time. To combat this, banks and financial institutions have implemented various measures, such as:
- **Real-Time Monitoring**: Using automated systems to flag suspicious transactions.
- **Two-Factor Authentication (2FA)**: Requiring additional verification steps for transactions.
- **Transaction Alerts**: Notifying customers of transactions in real time to ensure their validity.
- **Spending Limits**: Setting daily or per-transaction limits to reduce the impact of potential fraud.

Fraud detection is critical to:
1. **Protect Customers**: Prevent customers from being wrongly charged.
2. **Minimize Losses**: Reduce financial losses for both banks and individuals.
3. **Enhance Trust**: Build trust between financial organizations and their customers.

This project is an essential step toward understanding the **patterns of fraudulent transactions** and developing a **basic model** to detect them. The goal is to compare multiple models and determine which provides the **highest accuracy** for this use case.

---

## **2. Dataset**
- **Source**: [Kaggle Dataset]
- **Description**: Simulated dataset containing legitimate and fraudulent transactions from January 1, 2019, to December 31, 2020.
- **Scope**:
  - Transactions from **1,000 customers** at **800 merchants**.
- **Columns**:
  - Full List: ['Unnamed: 0', 'trans_date_trans_time', 'cc_num', 'merchant', 'category', 'amt', 'first', 'last', 'gender', 'street', 'city', 'state', 'zip', 'lat', 'long', 'city_pop', 'job', 'dob', 'trans_num', 'unix_time', 'merch_lat', 'merch_long', 'is_fraud']
  - Relevant Columns: ['trans_date_trans_time', 'amt', 'merchant', 'category', 'is_fraud']
  
### **Dataset Preparation**
Data preparation is crucial for any machine learning task. While this dataset is already cleaned and divided into training and testing sets, key considerations for future work include:
- Handling **missing values**.
- Removing **irrelevant data**.
- Addressing **imbalanced data**.
- **Scaling and transforming** features appropriately.

If starting from scratch, these steps must be implemented meticulously to ensure accurate model performance. As a Data Scientist, I believe that data preparation and feature engineering form the foundation of a successful model.

---

## **3. Analysis**
- **Total Transactions**: 1,842,743 (legitimate) + 9,651 (fraudulent)
- **Fraudulent Transactions Percentage**: **0.52%**
- The dataset is **highly imbalanced**, which necessitates the use of specialized techniques like oversampling or cost-sensitive learning to train models effectively.

### **Merchant Names**
- **Key Observations**:
  - All merchant names in the dataset start with `'fraud_'`.
  - Fraud transaction percentages range between **0.0% and 2.18%**.

- **Feature Engineering**:
  - **`merchant_with_multiple_fraud_transactions`**:
    - A new feature that flags merchants associated with multiple fraudulent transactions.
    - This feature improves the model's sensitivity to recurring fraud patterns.

### **Category Analysis**
- **Number of Unique Categories**: 14
- **Most Frequent Categories**: `grocery_pos` and `shopping_net`
- **Additional Observations**:
  - Consumer spending patterns can provide deeper insights into behavior.
  - For marketing or customer profiling purposes, one could analyze spending trends across categories. However, this aspect is outside the scope of fraud detection and does not influence model performance directly.
 
### **Descriptive Statistics for 'amt' in Fraud Transactions**

| Statistic | Value       |
|-----------|-------------|
| **Count** | 9,651       |
| **Mean**  | 530.66      |
| **Std**   | 391.03      |
| **Min**   | 1.06        |
| **25%**   | 240.08      |
| **50%**   | 390.00      |
| **75%**   | 902.37      |
| **Max**   | 1,376.04    |

#### Key Observations:
- The average transaction amount for fraudulent transactions is **$530.66**.
- Most fraud transactions fall between **$240.08 (25th percentile)** and **$902.37 (75th percentile)**.
- The smallest fraudulent transaction amount is **$1.06**, and the largest is **$1,376.04**.

This distribution indicates that fraudulent transactions are not confined to large amounts but can occur at a wide range of transaction values.

### **Multiple Fraud Transactions Per Person**

There is a noticeable section of individuals who have **100% fraudulent transactions**. These cases are worth flagging for further investigation or inclusion in a **fraudulent list**. While it is possible for two individuals to share the same name or for a fraudster to use someone else's fake identity, financial institutions typically have measures in place to handle such cases.

To enhance the model:
- **Flag Names or Credit Card Numbers**: Any individual with more than **40% of transactions flagged as fraudulent** is added to a **watchlist**.
- **Use This Feature in the Model**: This flag is introduced as a feature to improve the model's predictive capabilities.
- **Continuous Monitoring**: Maintain and update this list over time to track recurring fraudulent behavior.

By tracking these flagged names or credit card numbers, banks can implement preventive measures to minimize losses and proactively detect fraud.

### **Observation on Gender**

| Gender | Non-Fraud Transactions | Fraud Transactions | Fraud Percentage |
|--------|-------------------------|--------------------|------------------|
| **F**  | 1,009,850              | 4,899             | 0.48%           |
| **M**  | 832,893                | 4,752             | 0.57%           |

#### Key Insights:
- There is **no significant pattern** in fraudulent transactions between genders.
- Both males and females exhibit similar fraud percentages, with **Females** at **0.48%** and **Males** at **0.57%**.
- This feature does not seem to have a strong predictive power and **can be avoided** as a feature during model training.

#### Conclusion:
Gender does not significantly impact the likelihood of fraudulent transactions and is not a valuable feature for this specific dataset.

### **State-Wise Analysis of Fraudulent Transactions**

| State | Non-Fraud Transactions | Fraud Transactions | Fraud Percentage |
|-------|-------------------------|--------------------|------------------|
| **AK** | 2,913                | 50                | 1.69%           |
| **AL** | 58,243              | 278               | 0.48%           |
| **AR** | 44,416              | 195               | 0.44%           |
| **CA** | 80,093              | 402               | 0.50%           |
| **DE** | 0                   | 9                 | **100.00%**     |
| **FL** | 60,441              | 334               | 0.55%           |
| **NY** | 118,689             | 730               | 0.61%           |

#### Key Observations:
1. **High Fraudulent Percentage States**:
   - **Delaware (DE)**: 100% fraudulent transactions.
   - **Rhode Island (RI)**: 2.01%.
2. **Large Transaction Volume States**:
   - **New York (NY)** and **Texas (TX)** have the highest total transactions but maintain a lower fraud percentage.
3. **Outliers**:
   - States with no non-fraudulent transactions but high fraud percentages should be flagged for further investigation.

---

### **City-Wise Analysis of Fraudulent Transactions**

| City          | Non-Fraud Transactions | Fraud Transactions | Fraud Percentage |
|---------------|-------------------------|--------------------|------------------|
| **Angwin**    | 0.0                   | 10.0              | **100.00%**     |
| **Nanuet**    | 0.0                   | 10.0              | **100.00%**     |
| **Orange Park** | 0.0                 | 10.0              | **100.00%**     |
| **Oakton**    | 0.0                   | 9.0               | **100.00%**     |
| **Las Vegas** | 0.0                   | 8.0               | **100.00%**     |

#### Key Observations:
1. **Cities with 100% Fraudulent Transactions**:
   - Cities like **Angwin**, **Nanuet**, and **Orange Park** have only fraudulent transactions.
   - These cities should be flagged for detailed investigation.
2. **Pattern**:
   - Cities with no non-fraudulent transactions appear as suspicious outliers and may indicate a localized issue with fraud prevention or reporting.

---

### **Correlation Between Fraudulent Transactions and City Population**

#### Correlation Results:
| Variable 1               | Variable 2               | Correlation Coefficient |
|---------------------------|--------------------------|--------------------------|
| **is_fraud**             | **city_pop**            | **0.000325**            |
| **city_pop**             | **is_fraud**            | **0.000325**            |

#### Key Observations:
1. The correlation coefficient between **fraudulent transactions (`is_fraud`)** and **city population (`city_pop`)** is extremely low (**0.000325**).
2. This indicates **no significant linear relationship** between the population size of a city and the likelihood of fraudulent transactions.

#### Analysis:
- The data suggests that fraud does not disproportionately occur in cities with larger populations.
- While city population may still be a useful descriptive feature, it likely does not contribute significantly to fraud prediction models.

### Observation on Jobs

- Out of **497 Jobs**, **22 jobs** have high fraudulent activities.
- All these 22 jobs have **100% fraudulent activities**.
- This might just be a mere coincidence, but it is important to consider this feature in the modeling process.

---

## **4. Models**
The following models will be implemented and compared based on their interpretability, accuracy, and suitability for imbalanced datasets:
1. **K-Nearest Neighbors (KNN)**
2. **Support Vector Machines (SVM)**
3. **Logistic Regression**
4. Advanced models like **Random Forest** and **Gradient Boosting** for comparison.

Key evaluation metrics:
- Precision
- Recall
- F1 Score
- ROC-AUC

### ** Model Evaluation:**
The Models in evaluated on the bases of Accuracy and Speed of Performance

|Serial Number| Model    | Accuracy   | Speed    | Overal Ranking |
|-------------|----------|------------|----------|-----------------|

---

## **5. Results**
The results section will document:
1. Model performance on the test dataset.
2. Comparative analysis of models using the evaluation metrics.
3. Insights into which model works best for this dataset and why.

---

## **6. Conclusion**
This section will summarize:
- The best-performing model for fraud detection.
- The trade-offs between interpretability and accuracy.
- Recommendations for future work, such as testing on real-world datasets or exploring advanced ensemble methods.

---

## **7. References**
- Daly, J. (2021). *Credit Card Fraud Statistics 2021.*
- [Kaggle Dataset Link](https://www.kaggle.com/)
