# Predicting 28-Day Mortality in Sepsis-Associated Liver Injury (SALI) Patients

## 📌 TL;DR (Project Summary)
- Built and validated 8 machine learning models using MIMIC-IV and MIMIC-III datasets (1,192 ICU patients).
- Best internal AUC: **0.916 (SVM)**; Best external AUC: **0.932 (Random Forest)**.
- Feature selection via permutation importance and SHAP; top predictors include **heart rate, AST, PTT, lactate, and urine output**.
- Outperformed SOFA, qSOFA, and APACHE II traditional severity scores.
- SHAP analysis provided clinical interpretability, highlighting biologically plausible predictors.

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Data Collection](#data-collection)
4. [Data Preprocessing](#data-preprocessing)
5. [Modeling and Results](#modeling-and-results)
6. [SHAP Value Analysis](#shap-value-analysis)
7. [Conclusion](#conclusion)

---

## Project Overview
This project aims to develop and validate interpretable machine learning models to predict 28-day mortality in patients with Sepsis-Associated Liver Injury (SALI). Using clinical data extracted from the MIMIC-IV and MIMIC-III databases, multiple classifiers were trained and evaluated, including SVM, XGBoost, Random Forest, Logistic Regression, and more. The study also incorporated feature importance analysis using permutation importance and SHAP to ensure transparency in model predictions.

---

## Dataset
Data was collected from:
- **MIMIC-IV (v2.2)**: Records from 2008 to 2019, used for model training and internal validation.
- **MIMIC-III (v1.4)**: Records from 2001 to 2012, used for external validation.

A total of 1,192 adult ICU patients diagnosed with SALI were included. SALI was defined based on bilirubin > 2 mg/dL and INR > 1.5. Exclusion criteria included pregnancy, HIV, other liver diseases, and absence of required tests within 24 hours.

---

## Data Collection
Structured Query Language (SQL), MySQL, and Python were used to extract 69 clinical features from the first 24 hours of ICU admission. These were categorized into:
- **Demographics**: Age, gender, BMI, race, admission type.
- **Vital Signs**: Heart rate, respiratory rate, blood pressure, temperature, oxygen saturation.
- **Laboratory Indicators**: Bilirubin, PTT, AST, lactate, albumin, hemoglobin, WBC, and more.
- **Comorbidities**: Hypertension, diabetes, congestive heart failure, renal disease.
- **Interventions**: Vasopressor use, mechanical ventilation, antibiotics.
- **Clinical Measurements**: Urine output, GFR.
- **Severity Scores**: SOFA, qSOFA, APACHE II, GCS.

---

## Data Preprocessing
- **Missing Values**: Features with >20% missing were removed. Remaining missing values were imputed using the median from the training set.
- **Categorical Encoding**: Binary variables were label-encoded; multiclass variables were one-hot encoded.
- **Outliers & Skew**: Winsorization was applied, and log transformation was used for skewed features.
- **Multicollinearity**: Highly correlated features (r > 0.85) were removed; VIF analysis further reduced redundancy.
- **Scaling**: All features were standardized.

<p align="center">
  <img src="https://github.com/user-attachments/assets/72a3a9e5-aecf-4ec6-9af2-510dd4614bb9" alt="Patient Selection Flowchart" width="500"/>
  <br>
  <em>Figure 1. Patient Selection Flowchart for MIMIC-III and MIMIC-IV</em>
</p>




Permutation importance was used to identify the top 30 features most predictive of mortality, including:
- Minimum heart rate, PTT, AST, lactate
- Total urine output
- Diastolic BP, systolic BP
- Glucose, BUN, albumin

<p align="center">
  <img src="https://github.com/user-attachments/assets/11c4a917-2bba-44d8-89db-d1486180d16e" alt="Patient Selection Flowchart" width="500"/>
  <br>
  <em>Figure 2. Bar chart of Important features using Permutation Importance </em>
</p>


---

## Modeling and Results
Eight machine learning models were developed and compared:
- Logistic Regression
- ElasticNet-regularized Logistic Regression (CV_glmnet)
- Random Forest
- Support Vector Machine (SVM)
- K-Nearest Neighbors (KNN)
- Decision Tree
- Linear Discriminant Analysis (LDA)
- XGBoost

### Internal Validation (MIMIC-IV):
- **Best model**: SVM with AUC of 0.9159
- XGBoost: AUC 0.9127
- Logistic Regression: AUC 0.8852
- Random Forest: AUC 0.8874

<p align="center">
  <img src="https://github.com/user-attachments/assets/051c4d7b-03cd-4666-ba04-beb2dbcfe6a6" alt="Patient Selection Flowchart" width="500"/>
  <br>
  <em>Figure 3. ROC Curve plot for Internal Validation </em>
</p>


### External Validation (MIMIC-III):
- **Best model**: Random Forest with AUC of 0.9317
- XGBoost and SVM: AUC 0.9191

All models demonstrated high recall and precision, significantly outperforming traditional severity scores such as SOFA and APACHE II.

<p align="center">
  <img src="https://github.com/user-attachments/assets/86244d85-9699-48d6-9084-32e0632380d8" alt="Patient Selection Flowchart" width="500"/>
  <br>
  <em>Figure 4. ROC curve plot for external validation </em>
</p>


## SHAP Value Analysis
To interpret the XGBoost model, SHAP (Shapley Additive Explanations) was used:
- **Top SHAP features**:
  - Minimum heart rate
  - Minimum PTT
  - Minimum AST
  - Minimum lactate
  - Systolic Blood Pressure
  - Total urine output

SHAP plots revealed how individual feature values influenced the mortality prediction. For instance, lower heart rate and lower albumin were associated with higher mortality risk.

<p align="center">
  <img src="https://github.com/user-attachments/assets/59894cef-9586-4a94-a028-793c300d13a4" alt="Patient Selection Flowchart" width="500"/>
  <br>
  <em>Figure 5. SHAP summary chart.
Feature Impact on SVM Predictions (SHAP Values) </em>
</p>

---

## Conclusion
This project successfully developed machine learning models that accurately predict 28-day mortality in SALI patients, using early ICU data. SVM and Random Forest models showed excellent performance, and interpretability was enhanced using SHAP values. The models can assist clinicians in early identification of high-risk patients and improve ICU decision-making. External validation confirms the models' generalizability, paving the way for future clinical applications.


