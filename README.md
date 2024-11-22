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

Models will also be analyzed for their interpretability to align with FinTech compliance requirements.

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
