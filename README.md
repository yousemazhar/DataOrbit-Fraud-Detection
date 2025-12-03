

## **Project Overview**

This project implements a complete fraud-detection pipeline for Medicare/Medicaid provider claims using supervised machine learning.
The objective is to identify potentially fraudulent healthcare providers based on aggregated **beneficiary**, **inpatient**, and **outpatient** claim data.

The work follows the full end-to-end ML lifecycle:

1. **Data Exploration & Feature Engineering**
2. **Modeling & Hyperparameter Tuning**
3. **Evaluation, Error Analysis, and Cost-Based Threshold Optimization**
4. **Technical Report & Final Presentation**

The final fraud detection model is chosen based on accuracy, recall, F1-score, cost optimization, and explainability criteria aligned with CMS audit requirements.

---

## **Team Members**

| Name          | Student ID | Role |
|---------------|------------|------|
| Jana Raed     | 13002886   | Data Exploration & EDA Visualization |
| Karem Elfeal  | 13001824   | Modeling & Hyperparameter Tuning |
| Yousef Magdy  | 13007105   | Evaluation, Error Analysis & Report Writing |
| Yousef Taha   | 13001373   | Feature Engineering & Pipeline Integration |


---

## **Project Structure**

```
Project2/
│
├── data/
│   ├── Train_Beneficiarydata.csv
│   ├── Train_Inpatientdata.csv
│   ├── Train_Outpatientdata.csv
│   ├── Train_labels.csv
│
├── notebooks/
│   ├── 01_data_exploration_and_feature_engineering.ipynb
│   ├── 02_modeling.ipynb
│   ├── 03_evaluation.ipynb
│

│
├── models/
│   ├── final_random_forest_model.pkl
│   ├── feature_names.pkl
│
├── reports/
│   ├── Technical_Report.pdf
│   ├── Presentation.pdf
│
└── README.md
```

---

## **Summary of Results**

### **Final Selected Model: Random Forest (Class-Weighted)**

After testing multiple models (Logistic Regression, SVM, Decision Tree, Random Forest, XGBoost), the Random Forest model demonstrated the best balance between performance and interpretability.

#### **Performance Metrics (Test Set)**

* **Accuracy:** 94.57%
* **Precision:** 0.7013
* **Recall (TPR):** 0.7105
* **F1-Score:** 0.7059
* **False Positive Rate:** 3.13%
* **False Negative Rate:** 28.95%

#### **Confusion Matrix**

* **TP:** 54
* **TN:** 712
* **FP:** 23
* **FN:** 22

### **Cost-Based Threshold Optimization**

Using CMS-aligned cost assumptions:

* FP Cost = $15,000
* FN Cost = $100,000
* TP Cost = $5,000

**Optimal threshold: 0.30**
→ Reduces net cost by **$1.43M** compared to threshold 0.50
→ Cuts false negatives nearly in half

### **Key Insights**

* Fraud is strongly correlated with **Total_Reimbursement**, **Claim Duration**, **High-Value Inpatient Procedures**, and **Chronic Condition Complexity**
* False positives were legitimate high-volume cardiac specialty centers
* False negatives were low-volume fraudulent providers with normal-like behavior
* Random Forest provided the best interpretability while capturing non-linear fraud patterns

---

#How to Run the Project (Google Colab Version)

This project is executed using three Google Colab notebooks, each corresponding to one of the Python pipeline scripts:

01_data_exploration_and_feature_engineering.ipynb

02_modeling.ipynb

03_evaluation.ipynb

Each notebook requires specific input files and generates output files used in the next step.

1. Notebook 1 — Data Exploration & Feature Engineering

Link:
https://colab.research.google.com/drive/1SszFI5m3YxgDkOTmA2nueQqGW54wA-Xt?usp=sharing

Input Files Required

Upload all raw training data:

Train_Beneficiarydata.csv

Train_Inpatientdata.csv

Train_Outpatientdata.csv

Train_labels.csv

Outputs Generated

Notebook 1 (and the script 01_data_exploration_and_feature_engineering.py) produces:

provider_level.csv (the engineered provider-level dataset)

These files must be downloaded after Notebook 1 finishes.

2. Notebook 2 — Modeling & Training

Link:
https://colab.research.google.com/drive/1ZYwKSKCIjHzicS-IcRbTF-2Tc1E4-uui?usp=sharing

Input Files Required
provider_level.csv (the engineered provider-level dataset)

Upload the outputs from Notebook 1:


Outputs Generated

Notebook 2 (and script 02_modeling.py) will generate:

final_random_forest_model.pkl

scaler.pkl (only if scaling was applied)

model_results.json (metrics summary)

confusion_matrices.png and modeling plots (optional depending on your notebook)

Download these files for use in Notebook 3.

3. Notebook 3 — Evaluation, Error Analysis & Cost Analysis

Link:
https://colab.research.google.com/drive/1So5jHXdfkl1oLWsLqmjwUp4FVmj7YWbs?usp=sharing

Input Files Required

Upload:

final_random_forest_model.pkl

feature_names.pkl

df_provider_level.csv (from Notebook 1)

Raw original data (needed for full error analysis):

Train_Beneficiarydata.csv

Train_Inpatientdata.csv

Train_Outpatientdata.csv

Train_labels.csv

Outputs Generated

Notebook 3 (mirroring 03_evaluation.py) produces:

Confusion matrix figures

False positive / false negative case studies

Threshold optimization tables

Cost-based analysis results

Final model evaluation plots

These are the figures used in the Technical Report.

Full Pipeline Workflow

Run Notebook 1
→ Upload raw CSVs
→ Produces engineered data + train/test splits + feature names

Run Notebook 2
→ Upload .npy + .pkl files from Notebook 1
→ Produces trained Random Forest model

Run Notebook 3
→ Upload the trained model + feature_names + provider-level dataset
→ Performs evaluation, error analysis, cost analysis
