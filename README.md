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

## Conclusions
While Model 2 achieved higher specificity (79%), it failed to detect churners effectively (Sensitivity ~48%). Model 1 is the superior choice for business application because identifying potential churners is the priority. The analysis highlights that transactional behavior (first payments, rewards) and referral status are critical indicators of customer retention.

## Strategic Recommendations

While the predictive capabilities of the model are currently limited (AUC ~0.66), the **statistical analysis and feature importance study** uncovered strong behavioral drivers. The following strategies address the root causes of churn identified in the historical data:

### 1. Optimization of the Entry Barrier ("First Payment")
**Insight:** Exploratory analysis shows a friction point where low-commitment users (paying 0-2 units) exhibit a churn rate of **53%**, compared to **25%** for those paying slightly more.
**Strategic Action:** Instead of outright eliminating entry-level tiers, we recommend recalibrating the Value Ladder:
* **Upsell Focus:** Keep the low-entry tier but aggressively incentivize an immediate small upgrade (e.g., a "Starter Pack" at 2.5 units) during sign-up to nudge users into the "safe zone" of retention.
* **Quality Filter:** If the cost of serving users is high (CAC), consider raising the minimum entry price to filter out low-value users early, prioritizing unit economics over volume.

### 2. Activation through Engagement ("The Hook")
**Insight:** Feature importance algorithms (Information Gain) identified rewards engagement as a top retention driver. Non-churners redeem almost **3x more coins** than churners on average.
**Strategic Action:** Revamp the Onboarding Journey to **encourage** early ecosystem interaction:
* **"Quick Win" Strategy:** Design the first week so that redeeming a reward is effortless (e.g., "Welcome Bonus: Redeem your first gift now").
* **Educational Nudge:** Use in-app tutorials to guide users to the rewards section, creating a habit loop within the crucial first week.

### 3. Referral Program Restructuring
**Insight:** Historical data confirms that referred users have a **34.6% churn rate** vs. 24.5% for organic users, indicating a quality issue with incentivized leads.
**Strategic Action:** Shift from "immediate payout" to "milestone-based" referral bonuses (e.g., reward the referrer only after the new user remains active for 1 month). This aligns incentives with user quality rather than quantity.


## Limitations
This project provides a baseline for churn prediction, but several constraints were identified during the analysis:

Low Predictive Power: The Logistic Regression model achieved an AUC of 0.66. This indicates that the model has limited ability to distinguish between churners and non-churners effectively, performing only slightly better than random guessing in some configurations.

Weak Linear Correlations: The correlation analysis (Spearman) revealed that none of the explanatory variables had a strong direct correlation with the target variable is_churned. This suggests the relationships in the data might be non-linear, which Logistic Regression struggles to capture.

Skewed Distributions: Most numeric variables were highly right-skewed with many outliers. While nulls were imputed, the extreme outliers may still introduce noise into linear models.

Demographic Bias: The user base is concentrated in a young demographic (IQR 28-35 years old), which may limit the model's ability to generalize to older customer segments.



## Future Improvements
To improve model performance and business utility, the following steps are recommended:

Try Non-Linear Models: Since linear correlations were weak, implementing tree-based algorithms like Random Forest, XGBoost, or LightGBM could better capture complex, non-linear patterns and interactions between variables.

Advanced Balancing Techniques: Instead of simple threshold tuning (moving from 0.5 to 0.3), implementing synthetic oversampling techniques like SMOTE (Synthetic Minority Over-sampling Technique) or ADASYN during training could help the model learn the minority class features more effectively.

Investigate the "Referral Paradox": The analysis showed that referred users have a higher churn rate (34.6%) than non-referred users. A qualitative study or A/B testing is needed to understand if the referral incentives attract low-quality users who leave once the reward is consumed.

