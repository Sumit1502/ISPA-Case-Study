## Telco Customer Churn Prediction Analysis

This document summarizes the analysis of the Telco Customer Churn Dataset, covering data preparation, rule induction, model comparison, and deployment concepts.

---

### Project Goal

The primary goal of this project was to predict customer churn, extract actionable decision rules, and compare the predictive performance of different machine learning models (Decision Tree/CHAID-like and Logistic Regression).

---

### Methodology and Key Findings

#### 1. Data Preparation and EDA
* **Cleaning:** The initial dataset was cleaned by removing 11 rows with missing values in the `TotalCharges` column.
* **Target:** The target variable, **Churn**, showed an imbalance, with approximately **26.6%** of customers leaving the company.
* **Feature Engineering:** Features were preprocessed using **One-Hot Encoding** for categorical variables and **Standard Scaling** for numerical variables (`tenure`, `MonthlyCharges`, `TotalCharges`).

#### 2. Key Churn Factors
The analysis consistently identified the following as the strongest predictors of churn:
* **Contract Type:** Customers on **Month-to-month contracts** are at the highest risk.
* **Internet Service:** Customers using **Fiber Optic** internet service show a significantly elevated churn rate.
* **Tenure:** **New customers (low tenure)** on flexible contracts are the most vulnerable segment.

#### 3. Rule Induction (CHAID-like)
A Decision Tree model (acting as a CHAID-like algorithm) was used to extract clear business rules. The top-level split confirms that **Contract Type** is the single most important factor.

**Example Actionable Rule:** Target all customers who are **Month-to-month**, use **Fiber Optic**, and have very **Low Tenure**. This specific segment has a high likelihood of churning and requires immediate intervention.

#### 4. Model Performance Comparison
Two models were compared: Logistic Regression and Decision Tree.

| Metric | Decision Tree | Logistic Regression |
| :--- | :--- | :--- |
| **ROC-AUC** | 0.8220 | **0.8378** |

**Conclusion:** The **Logistic Regression** model offered better overall predictive performance (higher ROC-AUC) and is the preferred scoring model. The Decision Tree's value lies in its high interpretability for defining specific, targeted marketing segments.

---

### Deployment and MLOps Concepts

* **Deployment:** The final model, including all preprocessing steps, must be saved as a single **pipeline** using **Joblib**. This serialized object is then loaded into the production scoring environment (e.g., an API) to ensure consistent, accurate predictions on new, raw customer data.
* **Model Updating:** To counter **Concept Drift** (changes in customer behavior over time), the model's performance must be continuously **monitored**. Retraining should be automated via an **MLOps pipeline** whenever performance degrades or on a regular schedule (e.g., quarterly), using the latest customer data.
