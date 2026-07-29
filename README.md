# LoanGenie
**Loan Approval Predictor** | **Logistic Regression** | **Streamlit** | **Docker**

## Overview
LoanGenie predicts loan approval eligibility based on applicant financial profile. Built on 4,269 real loan records, it uses a Logistic Regression model with engineered features achieving 98.24% test accuracy and ROC-AUC of 0.993.

## Tech Stack
| Category | Tool | Purpose |
| :--- | :--- | :--- |
| **Modelling** | scikit-learn | Logistic Regression, GridSearchCV |
| **Data** | pandas, numpy | EDA, Feature Engineering |
| **Scaling** | StandardScaler | Feature normalisation |
| **App** | Streamlit | Interactive UI |
| **Deployment**| Docker | Containerised inference |

## Dataset
* 4,269 loan records with 13 features including CIBIL score, income, assets, loan amount and term.
* Target: Approved (62%) / Rejected (38%) — mild imbalance, handled via Stratified Split.
* No missing values; asset columns aggregated from 4 sub-categories into a single `assets` feature.

## Feature Engineering
Six techniques applied to expose non-linear CIBIL behaviour to the linear model:
* **CIBIL Ordinal Bins:** 9 ordered buckets mirroring real bank lending policy thresholds.
* **Threshold Flags:** binary indicators for CIBIL ≥ 700, ≥ 750, < 500.
* **Quadratic Term:** `cibil_score²` to capture curved approval response.
* **Financial Ratios:** loan-to-income, loan-to-assets, income-per-dependent.
* **Interaction Feature:** CIBIL × income to jointly encode creditworthiness + capacity.
* **Log Transforms:** `log1p` applied to income, loan amount and assets to reduce right-skew.

## Model & Evaluation
* **Algorithm:** Logistic Regression (solver: lbfgs, L2 regularization)
* **Tuning:** GridSearchCV over C ∈ {0.001 → 200} with 5-fold StratifiedKFold
* **Best C=50:** CV Accuracy: 97.28% | Test Accuracy: 98.24%
* **ROC-AUC:** 0.993 | F1-Score: 0.98 on both classes | Precision ≈ Recall (FP ≈ FN)
* **No overfitting:** train/test gap < 0.5%; confirmed via Learning Curve and Confusion Matrix

## Project Structure
```text
LoanGenie/
├── loan_approval_app.py      # Streamlit app
├── model.pkl                 # Trained LR model
├── scaler.pkl                # Fitted StandardScaler
├── Loan_Approval.ipynb       # Full notebook
├── Dockerfile                
├── requirements.txt          
└── loan_approval_dataset.csv 
```

## Run Locally

### Without Docker
```bash
pip install -r requirements.txt
streamlit run loan_approval_app.py
```

### With Docker
```bash
docker build -t loangenie .
docker run -p 8501:8501 loangenie
```
Open `http://localhost:8501` in your browser.

---
*Developed by **Prakhar Shukla**, IIT (BHU) Varanasi | [GitHub](https://github.com/Prakh4r-S)*
