📧 Email Campaign Effectiveness Prediction
🧠 A Machine Learning Approach to Boost Click-Through Rate (CTR)
📌 Project Overview
This project dives into analyzing and improving an email marketing campaign run by an e-commerce company. The campaign's success is defined as the user clicking a link in the email. Using historical data, we aim to:

Evaluate campaign performance.

Discover user behavior patterns.

Build a machine learning model to optimize future email targeting and maximize the Click-Through Rate (CTR).

📁 Dataset Description
We were given three structured CSV files representing different parts of the email campaign:

1. email_table
Contains metadata for each sent email:

email_id

email_text (short/long)

email_version (generic/personalized)

hour, weekday, user_country

user_past_purchases

2. email_opened_table
Records email_id if the email was opened.

3. link_clicked_table
Records email_id if the user clicked the link inside.

Using these, we created two target columns:

opened (binary: 1 if opened)

clicked (binary: 1 if clicked)

**Case Description**

The marketing team of an e-commerce site has launched an email campaign. 
This site has email addresses from all the users who created an account in 
the past. 
They have chosen a random sample of users and emailed them. The email 
lets the user know about a new feature implemented on the site. From the 
marketing team perspective, success is if the user clicks on the link inside of 
the email. This link takes the user to the company site. 
You are in charge of figuring out how the email campaign performed and were 
asked the following questions: 
● What percentage of users opened the email and what percentage 
clicked on the link within the email? 
● The VP of marketing thinks that it is stupid to send emails in a random 
way. Based on all the information you have about the emails that were 
sent, can you build a model to optimize in future how to send emails to 
maximize the probability of users clicking on the link inside the email? 
● By how much do you think your model would improve click through rate 
(defined as # of users who click on the link/total users who receive the 
email). How would you test that? 
● Did you find any interesting pattern on how the email campaign 
performed for different segments of users? Explain.

🔍 Step-by-Step Workflow
1. 🧹 Data Preparation
Merged all three tables using email_id.

Engineered opened and clicked labels.

Handled categorical columns via One-Hot Encoding.

Observed heavy class imbalance in clicked column (~3-5% clicked), so we applied:
![image](https://github.com/user-attachments/assets/30f709de-9dd1-42b8-a54a-4fa53885f379)


👉 Resampling Techniques:
RandomUnderSampling: Remove majority class samples.

SMOTE: Generate synthetic minority class samples (for better learning without information loss).

2. 🧪 Model Building
We trained the following models with both resampling strategies:

Logistic Regression

Decision Tree

Random Forest

XGBoost

LightGBM

K-Nearest Neighbors (KNN)

Each model was evaluated using: Accuracy,Precision,Recall,F1 Score,ROC AUC

![image](https://github.com/user-attachments/assets/ab305ca9-5d92-4e11-a337-c3ec479e46df)




📊 Model Comparison Summary
Here's what we found (focused on F1 Score & ROC AUC on test set):


Model	Sampling	Test F1	ROC AUC	Comment
KNN	SMOTE	84.42%	83.26%	High variance between recall in train/test → Overfitting
XGBoost	SMOTE	86.47%	84.21%	Strong performance, good balance
LightGBM	SMOTE	86.57%	84.30%	✅ Best overall balance, minimal overfitting
✅ Why We Chose LightGBM + SMOTE
Highest Test F1 Score: 86.57%

High ROC AUC: 84.30% → good separation power

Generalization: Very little performance drop from training to test

Efficient: Fast training and supports categorical handling (internally, in native form)

Compared to KNN, which overfitted heavily under SMOTE (Recall = 99% on train vs 84% on test), LightGBM showed the best realistic and deployable performance.
