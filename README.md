# Telecom-Churn-Prediction

This project analyzes customer behavior data from an Indian telecommunications company to predict customer attrition (Churn). Using a dataset of **104,143 records**, this repository documents the full data science workflow in **R**: from data cleaning and Exploratory Data Analysis (EDA) to feature selection (PCA & Entropy) and Logistic Regression modeling.

## Project Objective
The main goal is to develop a binary classification model to:
1.  Identify key variables influencing a customer's decision to leave.
2.  Predict churn probability (`is_churned`) to enable preventative actions.

## Tech Stack
* **Language:** R
* **Key Libraries:** `tidyverse`, `caret`, `ggplot2`, `pROC`, `FSelectorRcpp`, `corrplot`.
* **Techniques:** PCA (Principal Component Analysis), IGA (Information Gain Analysis), Logistic Regression, Threshold Tuning.

## Dataset Description
The dataset contains 18 variables (15 numeric, 2 categorical, 1 logical).
* **Target Variable:** `is_churned` (0: Retained, 1: Churned).
* **Class Imbalance:** 71.3% of users are retained, while 28.7% churned.

## Methodology

### 1. Cleaning & Preprocessing
* **Imputation:** Null values in numeric variables were replaced with the **median** due to skewed distributions.
* **Transformation:** Binary and categorical variables were converted to factors.

### 2. Exploratory Data Analysis (EDA)
Key insights discovered during analysis:
* **Referrals:** Counter-intuitively, users who were referred (`is_referral = TRUE`) had a **higher churn rate (34.6%)** compared to non-referred users (24.5%).
* **Permissions:** Users who denied permissions (`given_permission_1`) showed higher abandonment rates (37.5%).
* **Age:** Churned users tended to be slightly older than retained users.

### 3. Feature Selection
Since direct correlations were weak, two statistical approaches were used to select variables:
* **PCA (Principal Component Analysis):** Selected 8 components explaining **81% of the variance**.
* **Entropy (Information Gain):** Identified transactional variables (payments and rewards) as the most information-rich features.

### 4. Modeling (Logistic Regression)
Two models were trained and compared:
* **Model 1 (PCA-based):** Features selected based on their impact on principal components (e.g., `is_churned`, `reward_purchase`, `first_payment`, `age`, `is_referral`, etc.).
* **Model 2 (IGA-based):** Features selected based on Information Gain.

## Results & Performance

**Threshold Tuning:** A standard threshold of 0.5 resulted in the model predicting "No Churn" for almost everyone. The decision threshold was lowered to **0.30** to improve the detection of the minority class (Churn).

**Selected Model: Model 1**
Model 1 was chosen over Model 2 because it offered a better balance between Sensitivity and Specificity, avoiding extreme bias toward the majority class.

| Metric (Test Set) | Value | Interpretation |
| :--- | :--- | :--- |
| **Accuracy** | 61.96% | Overall correctness of the model. |
| **Sensitivity** | 62.30% | Ability to correctly identify churners. |
| **Specificity** | 61.11% | Ability to correctly identify retained users. |
| **AUC** | 0.662 | Area Under the ROC Curve. |

## Conclusion
While Model 2 achieved higher specificity (79%), it failed to detect churners effectively (Sensitivity ~48%). Model 1 is the superior choice for business application because identifying potential churners is the priority. The analysis highlights that transactional behavior (first payments, rewards) and referral status are critical indicators of customer retention.

## Limitations

## Future Improvements


