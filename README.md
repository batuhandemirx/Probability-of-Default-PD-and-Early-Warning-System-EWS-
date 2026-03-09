# Credit Risk Modeling & Early Warning System (EWS)

## Project Overview

This project develops a **credit risk prediction model and an Early Warning System (EWS)** to identify customers who are likely to default on their loans. The objective is to improve the bank’s ability to detect potential credit deterioration earlier than traditional credit scoring systems.

The analysis includes:

- Data cleaning and preprocessing
- Feature engineering
- Credit risk modeling using machine learning
- Model evaluation using industry-standard metrics
- Early Warning System (EWS) development
- Business value assessment for banking risk management

The model performance is compared against an existing **benchmark Probability of Default score used by the bank**.

---

# Dataset Description

The dataset contains **18,420 customer observations** with financial, demographic, and credit-related attributes.

Key variables include:

- Debt
- Initial credit limit
- Income
- Age
- Loan term
- Credit history rating
- Industry
- Living area
- Settlement (city)
- Scoring mark
- Family status
- Number of underage children
- Probability of Default (existing bank score)

The target variable was constructed from **Overdue Days**.

A borrower is classified as **default (OVERDUE = 1)** if:


Overdue Days ≥ 90


Otherwise:


OVERDUE = 0


The dataset shows significant **class imbalance**:

- Non-default customers: 17,665
- Default customers: 755

This imbalance required specific modeling techniques.

---

# Data Cleaning

Several preprocessing steps were performed to ensure model stability and data quality.

### 1. Handling Inconsistent Categorical Values

Some categorical variables contained inconsistent naming conventions (e.g., region names in different formats or languages). These were standardized using mapping dictionaries.

Example:


'Гомельская область' → 'Gomel Region'
'ГРОДНЕНСКАЯ ОБЛАСТЬ' → 'Grodno Region'


This step ensured consistent geographic categories.

---

### 2. Handling Missing Values

Some features contained missing values such as:

- Credit history rating
- Living area
- Settlement name
- Scoring mark

XGBoost can handle missing values directly. Therefore:

- Missing values were preserved as **NaN**
- Infinite values were converted to NaN before training


df_model = df_model.replace([np.inf, -np.inf], np.nan)


---

### 3. Encoding Categorical Variables

Categorical variables were encoded using **Label Encoding**:


Sex
Education
Living_Area
Settlement_Name
Industry_Name
Credit_History_Rating


This conversion enabled the machine learning algorithm to process categorical attributes.

---

# Feature Engineering

Several additional features were engineered to improve predictive power.

### 1. Loan Utilization Rate

Measures how much of the credit limit is used.


Utilization Rate = Debt / Initial Limit


This is an important indicator of financial stress.

---

### 2. Debt-to-Income Ratio (DTI)

Measures financial burden relative to income.


DTI Ratio = Debt / Income


Higher values indicate higher credit risk.

---

### 3. Multiple Loan Indicator

Customers with multiple loans were identified using the client ID frequency.


Took_Multiple_Loan = 1 if customer has more than one loan


Approximately **5,707 customers had multiple loans**.

---

### 4. Demographic and Socioeconomic Features

Additional explanatory variables included:

- Family status
- Number of underage children
- Education level
- Industry sector
- Settlement location

These variables capture behavioral and socioeconomic risk patterns.

---

# Model Development

The model was built using **XGBoost (Extreme Gradient Boosting)**, a powerful ensemble machine learning algorithm commonly used in credit risk modeling.

### Model Configuration

Key hyperparameters:


n_estimators = 100
learning_rate = 0.1
max_depth = 5
scale_pos_weight = imbalance_ratio
eval_metric = 'auc'


Class imbalance was addressed using the **scale_pos_weight parameter**, which adjusts the penalty for misclassifying minority class observations.

The dataset was split into:


Training set: 80%
Test set: 20%


Stratified sampling ensured class balance across splits.

---

# Model Performance

The model was evaluated using multiple performance metrics commonly used in credit risk analytics.

---

# ROC Curve (Receiver Operating Characteristic)

The ROC curve evaluates the model's ability to distinguish between defaulters and non-defaulters.

The **Area Under the Curve (AUC)** measures discriminatory power.

Higher AUC indicates stronger predictive capability.

---

# Precision-Recall Curve

The Precision-Recall curve is particularly useful for **imbalanced datasets**, where default events are rare.

- **Precision** measures prediction accuracy for positive cases.
- **Recall** measures the model’s ability to capture actual defaulters.

This metric is especially important for risk detection.

---

# Classification Report

Model results:

| Metric | Value |
|------|------|
| Accuracy | 0.96 |
| Precision (Default) | 0.50 |
| Recall (Default) | 0.75 |
| F1 Score (Default) | 0.60 |

The model successfully captures **75% of default events**, which is strong for an imbalanced credit dataset.

---

# Gini Coefficient Comparison

The Gini coefficient is widely used in the banking industry to measure credit model performance.

It is calculated as:


Gini = 2 × AUC − 1


### Results

| Model | Gini |
|------|------|
| Proposed Model | **0.8962** |
| Existing Bank Score | **-0.2261** |

The proposed model significantly outperforms the benchmark scoring system.

This indicates that the engineered features provide substantial predictive value.

---

# Early Warning System (EWS)

An additional model was developed to detect **customers at risk before default occurs**.

Instead of predicting 90+ day defaults, the EWS identifies customers who are **31–90 days overdue**.

Customers already defaulted were excluded from this analysis.

Remaining dataset:

- Customers analyzed: 17,665
- Early warning signals: 426

---

# EWS Model Performance

Results:

| Metric | Value |
|------|------|
| Accuracy | 0.83 |
| Recall (Risk Cases) | 0.75 |
| Precision (Risk Cases) | 0.10 |
| AUC | 0.8592 |

The high recall indicates that the system successfully identifies a large proportion of emerging risk cases.

Although precision is relatively low, this is expected in early warning systems where **missing a risky client is more costly than issuing a false alert**.

---

# Business Value for Banks

The Early Warning System provides several operational benefits:

### 1. Early Risk Detection

The system detects risk signals **before customers reach default status**, allowing proactive intervention.

### 2. Reduced Credit Losses

Banks can apply preventive actions such as:

- restructuring loans
- reducing credit limits
- initiating collection processes earlier

### 3. Improved Portfolio Monitoring

Risk managers gain visibility into **emerging portfolio deterioration**.

### 4. Better Capital Allocation

Stronger credit risk models enable banks to:

- improve regulatory capital calculations
- optimize risk-adjusted returns

---

# Conclusion

This project demonstrates that machine learning techniques combined with thoughtful feature engineering can significantly improve credit risk prediction.

Key achievements include:

- A strong **Gini score of 0.896**
- A high **default recall rate (75%)**
- An effective **Early Warning System with AUC 0.86**

The proposed approach substantially outperforms the benchmark credit scoring system and provides meaningful operational value for banking risk management.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-Learn
- XGBoost
- Matplotlib
- Seaborn
