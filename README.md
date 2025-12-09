# Fraud Detection with XGBoost

In this project, I built a simple and clear fraud detection model using **XGBoost** and **SHAP**.
The goal is to identify fraudulent financial transactions using realistic, simulated banking data.

I created the dataset myself based on typical fraud behaviours seen in real banks and fintech companies.




## Project Goals

* Create a realistic synthetic fraud dataset
* Preprocess the data and encode categorical features
* Train an XGBoost model on imbalanced data
* Evaluate the model (precision, recall, confusion matrix, ROC-AUC)
* Explain the model using SHAP values
* Build a transparent and understandable fraud model




## Dataset Overview

The dataset includes typical transaction features:

| Feature          | Description                             |
| ---------------- | --------------------------------------- |
| amount           | Transaction amount                      |
| oldbalanceOrg    | Sender’s balance before transaction     |
| newbalanceOrig   | Sender’s balance after transaction      |
| oldbalanceDest   | Receiver’s balance before transaction   |
| newbalanceDest   | Receiver’s balance after transaction    |
| transaction_type | TRANSFER or CASH_OUT                    |
| isFraud          | Target variable (0 = normal, 1 = fraud) |


### Fraud patterns I included:

* Very high transaction amounts
* TRANSFER transactions more likely to be fraud
* Sender balance often too low for the transaction
* Receiver balance jumps unusually high
* Fraud ratio around **20%** to make modeling easier




## Preprocessing

* One-hot encoding for transaction_type
* Train-test split using stratification
* Handling class imbalance with **scale_pos_weight**
* No scaling needed (tree-based model)



## Model: XGBoost

I chose XGBoost because:

* It performs extremely well on tabular financial data
* It handles imbalanced datasets
* It is more stable than single decision trees
* It works perfectly with SHAP for explainability

### Main parameters used:

```python
model = xgb.XGBClassifier(
    n_estimators=300,
    learning_rate=0.1,
    max_depth=5,
    subsample=0.8,
    colsample_bytree=0.8,
    scale_pos_weight=scale_pos_weight,
    eval_metric="logloss",
    random_state=42
)
```



## Model Evaluation

I evaluated the model using:

* Precision
* Recall
* F1-score
* Confusion matrix
* ROC-AUC

### Why recall is important:

In fraud detection, missing a fraud (false negative) is very costly for banks.
So recall for the fraud class (1) matters the most.




## Explainability (SHAP)

I used SHAP to understand **why** the model predicts a transaction as fraud or normal.

SHAP provides:

### Global explanation

Which features are most important overall.

### Local explanation

Why one single transaction was flagged as fraud.

### Insights I found:

* Higher transaction amounts (↑ fraud probability)
* TRANSFER transactions more risky
* Receiver balances jumping very high look suspicious
* Stable sender balances usually indicate normal behaviour




## Key Learnings

* XGBoost works very well on fraud data
* Handling class imbalance is essential
* SHAP makes the model explainable and regulator-friendly
* Fraud patterns based on real behaviour improve model accuracy
* This workflow is similar to what many banks use




## Future Improvements

* SMOTE / ADASYN oversampling
* Hyperparameter tuning with Optuna
* Model comparison (LightGBM, CatBoost)
* API deployment with FastAPI
* Streamlit dashboard for transaction monitoring



## Author

Emine Ceran

Financial Data Science Journey


GitHub: [https://github.com/ebceran](https://github.com/ebceran)

LinkedIn: [https://www.linkedin.com/in/emine-ceran-03b26159/](https://www.linkedin.com/in/emine-ceran-03b26159/)




