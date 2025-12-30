# Telecom-Churn-Prediction

This project analyzes customer behavior data from an Indian telecommunications company to predict customer attrition (Churn). [cite_start]Using a dataset of **104,143 records**, this repository documents the full data science workflow in **R**: from data cleaning and Exploratory Data Analysis (EDA) to feature selection (PCA & Entropy) and Logistic Regression modeling[cite: 28, 191].

## 🎯 Project Objective
The main goal is to develop a binary classification model to:
1.  Identify key variables influencing a customer's decision to leave.
2.  [cite_start]Predict churn probability (`is_churned`) to enable preventative actions[cite: 29].

## 🛠️ Tech Stack
* **Language:** R
* **Key Libraries:** `tidyverse`, `caret`, `ggplot2`, `pROC`, `FSelectorRcpp`, `corrplot`.
* **Techniques:** PCA (Principal Component Analysis), IGA (Information Gain Analysis), Logistic Regression, Threshold Tuning.

## 📊 Dataset Description
[cite_start]The dataset contains 18 variables (15 numeric, 2 categorical, 1 logical)[cite: 191].
* **Target Variable:** `is_churned` (0: Retained, 1: Churned).
* [cite_start]**Class Imbalance:** 71.3% of users are retained, while 28.7% churned[cite: 333].

## ⚙️ Methodology

### 1. Cleaning & Preprocessing
* [cite_start]**Imputation:** Null values in numeric variables were replaced with the **median** due to skewed distributions[cite: 211, 218].
* [cite_start]**Transformation:** Binary and categorical variables were converted to factors; the `user_id` column was dropped[cite: 243, 246].

### 2. Exploratory Data Analysis (EDA)
Key insights discovered during analysis:
* [cite_start]**Referrals:** Counter-intuitively, users who were referred (`is_referral = TRUE`) had a **higher churn rate (34.6%)** compared to non-referred users (24.5%)[cite: 1783].
* [cite_start]**Permissions:** Users who denied permissions (`given_permission_1`) showed higher abandonment rates (37.5%)[cite: 1784].
* [cite_start]**Age:** Churned users tended to be slightly older than retained users[cite: 822].

### 3. Feature Selection
Since direct correlations were weak, two statistical approaches were used to select variables:
* [cite_start]**PCA (Principal Component Analysis):** Selected 8 components explaining **81% of the variance**[cite: 1788].
* [cite_start]**Entropy (Information Gain):** Identified transactional variables (payments and rewards) as the most information-rich features[cite: 1277].

### 4. Modeling (Logistic Regression)
Two models were trained and compared:
* [cite_start]**Model 1 (PCA-based):** Features selected based on their impact on principal components (e.g., `is_churned`, `reward_purchase`, `first_payment`, `age`, `is_referral`, etc.)[cite: 1793].
* [cite_start]**Model 2 (IGA-based):** Features selected based on Information Gain[cite: 1804].

## 📈 Results & Performance

**Threshold Tuning:** A standard threshold of 0.5 resulted in the model predicting "No Churn" for almost everyone. [cite_start]The decision threshold was lowered to **0.30** to improve the detection of the minority class (Churn)[cite: 1794, 1795].

**Selected Model: Model 1**
[cite_start]Model 1 was chosen over Model 2 because it offered a better balance between Sensitivity and Specificity, avoiding extreme bias toward the majority class[cite: 1817].

| Metric (Test Set) | Value | Interpretation |
| :--- | :--- | :--- |
| **Accuracy** | 61.96% | [cite_start]Overall correctness of the model[cite: 1802]. |
| **Sensitivity** | 62.30% | [cite_start]Ability to correctly identify churners[cite: 1802]. |
| **Specificity** | 61.11% | [cite_start]Ability to correctly identify retained users[cite: 1802]. |
| **AUC** | 0.662 | [cite_start]Area Under the ROC Curve[cite: 1802]. |

## 📝 Conclusion
[cite_start]While Model 2 achieved higher specificity (79%), it failed to detect churners effectively (Sensitivity ~48%)[cite: 1815]. Model 1 is the superior choice for business application because identifying potential churners is the priority. [cite_start]The analysis highlights that transactional behavior (first payments, rewards) and referral status are critical indicators of customer retention[cite: 1819].

## 🚀 How to Run
1.  Clone this repository.
2.  Open the script in RStudio.
3.  Install required packages:
    ```r
    install.packages(c("tidyverse", "caret", "pROC", "corrplot", "FSelectorRcpp"))
    ```
4.  Run the script to reproduce the cleaning, visualization, and modeling steps.
