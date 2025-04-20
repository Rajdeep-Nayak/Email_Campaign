#  Email Campaign Effectiveness Prediction  
###  A Machine Learning Approach to Boost Click-Through Rate (CTR)

---

## Project Overview

Email campaigns are one of the most common forms of digital marketing, but not all campaigns are equally effective. In this project, we aim to **evaluate the performance** of a marketing email campaign run by an e-commerce company and build a **machine learning model** that can help improve future campaigns.

The **goal** of the campaign was simple:
> If a user clicks the link inside the email, it's considered a success.

### Objectives:
- Understand how many users opened and clicked on the email.
- Analyze behavior patterns among different user segments.
- Build a predictive model to **maximize future email click-through rates (CTR)**.

---

## Dataset Description

We were provided with **three structured CSV datasets**:

### 1. `email_table`  
Metadata for each sent email:
- `email_id`: Unique identifier
- `email_text`: Either short or long
- `email_version`: Either generic or personalized
- `hour`, `weekday`: When the email was sent
- `user_country`: Country of the user
- `user_past_purchases`: Number of previous purchases

### 2. `email_opened_table`  
- Contains `email_id` if the email was opened.

### 3. `link_clicked_table`  
- Contains `email_id` if the user clicked the link inside the email.

From these, we derived two target variables:
- `opened`: 1 if the email was opened, 0 otherwise
- `clicked`: 1 if the link was clicked, 0 otherwise

---

## Case Description

The e-commerce site's marketing team launched an email campaign to inform users about a new feature. A random sample of users received the email. A campaign is **considered successful** if the user clicks the link inside the email.

### Business Questions:
1. What percentage of users opened and clicked the email?
2. Can we build a model to improve future targeting and maximize clicks?
3. How much improvement in Click-Through Rate (CTR) can we expect using the model?
4. Are there noticeable patterns in user segments or behaviors?

---

##  Step-by-Step Workflow

---

### 1️⃣ Data Preparation & Feature Engineering

- Merged all three tables using `email_id`
- Created binary labels: `opened`, `clicked`
- Handled categorical variables using **One-Hot Encoding**
- All features converted to numeric format

---

###  Class Imbalance Handling

We observed that **only ~3–5% of users clicked** the link, making it a **highly imbalanced classification problem**.
![image](https://github.com/user-attachments/assets/30f709de-9dd1-42b8-a54a-4fa53885f379)

#### Resampling Techniques Used:
- **Random Under Sampling**: Reduced majority class to balance data
- **SMOTE (Synthetic Minority Over-sampling Technique)**: Generated synthetic minority samples to avoid information loss

---

### 2️⃣ Model Training & Evaluation

We experimented with several classification models:

- Logistic Regression
- Decision Tree
- Random Forest
- K-Nearest Neighbors (KNN)
- XGBoost
- LightGBM

All models were trained using both original and resampled data.

#### Evaluation Metrics:
- Accuracy
- Precision
- Recall
- F1 Score
- ROC AUC

---
![image](https://github.com/user-attachments/assets/ab305ca9-5d92-4e11-a337-c3ec479e46df)

##  Model Comparison Summary

| Model        | Sampling | Test F1 Score | ROC AUC | Notes |
|--------------|----------|----------------|---------|-------|
| KNN          | SMOTE    | 84.42%         | 83.26%  | Overfit: Very high recall on training, drops on test |
| XGBoost      | SMOTE    | 86.47%         | 84.21%  | Strong and consistent |
| **LightGBM** | **SMOTE**| **86.57%**     | **84.30%** | ✅ Best generalization and performance |

---

## ✅ Why We Chose LightGBM + SMOTE

- **Highest F1 Score on test set**: 86.57%
- **Best ROC AUC**: 84.30%
- **Minimal Overfitting**: Train/test performance nearly identical
- **Efficient Training**: Especially on large datasets
- **Supports Categorical Features**: (natively in LightGBM)

By contrast, **KNN + SMOTE** showed signs of overfitting:
- Recall on training = 99%
- Recall on testing = 84%

---
Estimated Uplift in Click-Through Rate
While a formal uplift simulation or A/B test was not part of this project's scope, the model's strong predictive performance indicates significant potential for improving campaign outcomes through intelligent targeting.

Baseline CTR (Random Emailing)
Historically observed Click-Through Rate: ~3–5%

Model-Based Targeting Potential
Based on the LightGBM + SMOTE model's robust performance:

Test F1 Score: 86.57%

ROC AUC: 84.30%

We can reasonably expect that prioritizing users with the highest predicted probability of clicking would result in a significant uplift in CTR, potentially doubling or even tripling the current baseline when applied strategically.

Suggested Real-world Validation (Next Steps)
To measure the real-world impact of model-driven targeting, the following A/B testing approach is recommended:

Group A (Control): Random selection of users (current strategy)

Group B (Treatment): Top N% users ranked by model probability

Compare CTR across groups to evaluate the actual uplift and business value of the model.


##  Key Insights from EDA

| Factor              | Insight |
|---------------------|---------|
| **Email Text**      | Short emails had higher engagement |
| **Email Version**   | Personalized emails outperformed generic ones |
| **Send Time**       | Emails sent between 8–11 AM had higher open rates |
| **User Country**    | Some countries performed significantly better |
| **Past Purchases**  | Users with more previous purchases had higher click rates |
