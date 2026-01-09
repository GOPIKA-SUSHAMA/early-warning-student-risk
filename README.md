# Early-Warning System for At-Risk Students

This project implements a machine-learning-based **early-warning system** to identify students at academic risk **before final examinations**, enabling timely and supportive educational interventions.


## 1. Problem Motivation

Traditional student performance models focus on predicting final grades after assessments are complete.  
Such predictions are **not actionable**.

This project reframes the task into an **early-warning classification problem**, where the objective is to detect students who are likely to underperform **early enough for intervention**.



## 2. Dataset Description
Source : Kaggle

The dataset contains academic engagement and assessment indicators for students:

### Input Features
- **Attendance (%)**
- **Internal Test 1 (out of 40)**
- **Internal Test 2 (out of 40)**
- **Assignment Score (out of 10)**
- **Daily Study Hours**

### Engineered Features
- **Internal Average Score**
- **Assignment Percentage**
- **Low Attendance Flag** (Attendance < 70%)

### Target Variable
- **Risk Label**
  - `1` → At-risk student  
  - `0` → Not at-risk  
- Defined as:

This threshold reflects an academic concern/fail boundary and is explicitly justified.

 

## 3. Problem Formulation

- **Task Type:** Binary classification  
- **Objective:** Maximise recall (sensitivity) to avoid missing at-risk students  
- **Why Recall Matters:**  
False negatives (missed at-risk students) are more costly than false positives in educational settings.

 

## 4. Methodology

### 4.1 Data Preparation
- Removal of non-predictive identifiers
- Feature engineering using domain knowledge
- Stratified train-test split (80/20)
- Explicit handling of class imbalance

 

### 4.2 Models Implemented

1. **Logistic Regression**
 - Interpretable baseline
 - Odds-ratio analysis

2. **Random Forest**
 - Captures non-linear feature interactions

3. **XGBoost (Final Model)**
 - Class imbalance handling via `scale_pos_weight`
 - Early stopping
 - Strong generalisation performance

 

## 5. Evaluation Strategy

Metrics used:
- **Recall (primary metric)**
- **ROC-AUC**
- **Confusion Matrix**
- **Precision–Recall Curve**

A custom **decision threshold** was selected to ensure:

This prioritises student safety over prediction strictness.

 

## 6. Threshold Optimisation

Rather than using the default 0.5 probability cutoff, the model threshold was tuned using the precision–recall curve.

- Selected threshold: **0.407**
- Ensures high recall across folds
- Fixed threshold applied consistently during cross-validation

 

## 7. Cross-Validation

- **5-fold Stratified Cross-Validation**
- Performance reported as **mean ± standard deviation**
- Confirms robustness of recall and AUC across data splits

 

## 8. Model Explainability

Explainability is essential in educational AI systems.

Implemented techniques:
- **Logistic Regression Odds Ratios**
- **SHAP (SHapley Additive exPlanations)** for XGBoost

Key insights:
- Attendance and internal assessments are the strongest predictors of academic risk
- Study hours provide secondary but meaningful signal

 

## 9. Inference & Deployment Demo

The project includes a reusable inference function:

It outputs:

-Risk probability

-Risk category (HIGH / LOW)

-Expected final mark (heuristic estimate for demonstration)

-Model artefacts are saved and reloaded using:

-early_warning_xgb.json

-early_warning_meta.json

-This simulates a real-world deployment pipeline.

