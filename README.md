# 🧪 Leptospirosis Case Outcome Prediction (MC853)

Machine learning project focused on predicting the outcome of leptospirosis cases (recovery vs death) using Brazilian public health data.

---

## 📌 Overview

Leptospirosis is a bacterial disease commonly transmitted through contact with contaminated animal urine, especially in areas with poor sanitation and flooding.

This project aims to:
- Analyze epidemiological data from Brazil (DataSUS)
- Build ML models to predict patient outcomes
- Evaluate fairness and ethical implications

---

## 📊 Dataset

- **Source:** DataSUS (Brazilian public health system) :contentReference[oaicite:0]{index=0}  
- **Years:** 2023–2024  
- **Samples:** ~9,153 cases  
- **Target variable:** `EVOLUCAO`
  - Cure
  - Death (leptospirosis)

---

## 🧹 Data Preprocessing

### Feature Selection

Removed:
- IDs and administrative fields
- Dates (non-informative)
- Rare or irrelevant symptoms
- Lab tests (mostly missing)
- Socioeconomic variables (to reduce bias)

> ⚠️ Note: Removing socioeconomic features may reduce performance but avoids reinforcing societal bias.

---

### Selected Features

**Demographics**
- Age
- Sex
- Region (encoded)

**Symptoms (binary)**
- Fever, myalgia, headache
- Vomiting, diarrhea
- Renal, respiratory, cardiac symptoms

---

### Data Cleaning

- Removed missing or invalid labels
- Converted age to years
- Encoded:
  - Sex → binary
  - Symptoms → binary
  - Region → one-hot encoding
- Filled missing symptom values using mode

---

### Train/Test Split

- **Train:** All regions except Southeast  
- **Test:** Southeast region (geographical split)

---

### Class Balancing

Original issue:
- Deaths ≈ 2–5%

Adjusted to:
- 75% cure
- 25% death

Techniques used:
- SMOTE (oversampling minority)
- NearMiss (undersampling majority)

---

## 🤖 Models

- K-Nearest Neighbors (KNN)
- Random Forest
- Logistic Regression

---

## ⚙️ Training Strategy

- Stratified K-Fold (k=5)
- Grid Search for hyperparameter tuning

---

## 📈 Evaluation

### Metric Choice

Initially used **Recall**, but it caused:
- Overprediction of death cases

Final choice:
- **F1-score** (balance between precision & recall)

---

### Best Model

- Logistic Regression  
- Parameters: `class_weight={0:1, 1:2}`  
- **F1-score:** ~0.545  

---

### Test Performance

- Balanced Accuracy: ~70.1%  
- Precision: ~39%  
- Recall: ~81.8%  

---

### Key Insight

The model:
- Detects most severe cases (high recall)
- Produces many false positives

> In healthcare, false positives may be preferable to missing critical cases.

---

## ⚖️ Fairness Analysis

### Sensitive Attribute
- Sex (male vs female)

---

### Metrics Evaluated

- Demographic Parity
- Equal Opportunity
- **Equalized Odds (chosen)**

---

### Findings (Before Adjustment)

- Significant precision gap (~12%)
- Similar recall across groups

---

### Fairness Strategy

Applied SMOTE oversampling to female data:
- 50%, 75%, 99% balancing scenarios

---

### Results

- No meaningful fairness improvement
- Performance decreased due to noise

---

## ⚠️ Ethical Considerations

- Model must **not** be used for real medical decisions  
- Predictions are **support tools only**  
- Human expertise is essential  

### Design Decision

Removed socioeconomic features to:
- Avoid bias reinforcement
- Promote fairness

> These factors affect real outcomes but may encode inequality into the model.

---

## 📌 Limitations

- Small and imbalanced dataset  
- Noisy/incomplete data  
- Limited generalization  
- Fairness constraints reduced performance  

---

## 🚀 Future Work

- Collect more diverse data  
- Explore better fairness techniques  
- Improve feature engineering  
- Test additional models  

---

## 📚 Conclusion

This project highlights:
- The complexity of medical prediction tasks  
- Trade-offs between accuracy and fairness  
- The importance of ethical ML  

Despite moderate performance, it provides a solid foundation for further research.
