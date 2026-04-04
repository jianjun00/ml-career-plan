# Week 7 — Model Evaluation, Validation, and Feature Engineering
## Phase 1: Foundations (Week 7 of 8)

---

## Theory (4h)

- **Evaluation metrics**: accuracy, precision, recall, F1, AUC-ROC, MSE, RMSE, MAE
- **When to use which**: class imbalance → AUC; equal error cost → accuracy; ordinal → RMSE
- **Cross-validation**: k-fold, stratified k-fold, time-series CV
- **Overfitting diagnosis**: learning curves, bias-variance tradeoff
- **Feature engineering**: polynomial features, interaction terms, target encoding, frequency encoding

---

## Metric Decision Guide

| Task | Primary Metric | When |
|------|---------------|------|
| Binary classification (balanced) | Accuracy or F1 | Equal class sizes |
| Binary classification (imbalanced) | AUC-ROC | Rare positive class |
| Multi-class | Macro-F1 | Treat all classes equally |
| Regression | RMSE | Penalize large errors heavily |
| Regression (robust) | MAE | Outlier-heavy targets |
| Ranking | NDCG | Order matters |

---

## Key Implementations

### AUC-ROC from scratch
```python
def auc_roc(y_true, y_score):
    # Sort by score descending
    sorted_idx = np.argsort(y_score)[::-1]
    y_true_sorted = y_true[sorted_idx]
    # Count TP and FP at each threshold
    tp = np.cumsum(y_true_sorted)
    fp = np.cumsum(1 - y_true_sorted)
    tpr = tp / tp[-1]
    fpr = fp / fp[-1]
    # Trapezoid rule
    return np.trapz(tpr, fpr)
```

### Full pipeline template (IOAI-ready)
```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import StratifiedKFold, cross_val_score

pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('model', xgb.XGBClassifier(n_estimators=300, random_state=42))
])
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(pipeline, X, y, cv=cv, scoring='roc_auc')
```

### Target encoding (for high-cardinality categoricals)
```python
# Compute mean target per category; use out-of-fold to avoid leakage
from category_encoders import TargetEncoder
enc = TargetEncoder(cols=['high_card_col'])
# WRONG: enc.fit_transform(X_full, y_full) — leaks test target statistics
# CORRECT: fit on train fold only, transform both train and test folds
enc.fit(X_train, y_train)
X_train_enc = enc.transform(X_train)
X_test_enc  = enc.transform(X_test)
```

## Data Leakage Checklist

Data leakage is the most common cause of "model works in CV but fails on leaderboard" at IOAI. Run through this checklist before every submission:

```
[ ] Scaler/encoder fitted on TRAIN ONLY, then applied to test
    WRONG: StandardScaler().fit_transform(X_full)
    RIGHT: scaler.fit(X_train); scaler.transform(X_train); scaler.transform(X_test)

[ ] Target encoding uses out-of-fold predictions, not full-data mean

[ ] PCA/dimensionality reduction fitted on TRAIN ONLY (see Week 4)

[ ] Feature engineering using only past data for time-series
    (no using t+1 information to predict t)

[ ] Cross-validation: fit_transform inside each fold, not outside
    WRONG: X_scaled = scaler.fit_transform(X); cross_val_score(model, X_scaled, y)
    RIGHT: Pipeline([('scaler', StandardScaler()), ('model', model)])
           cross_val_score(pipeline, X, y)  # scaler refits each fold

[ ] No test labels ever seen during training (obvious but easy to accidentally include)
```

**Telco churn benchmark** (so you know where you stand):
- Naive majority class predictor: AUC ~0.50
- Logistic regression + basic features: AUC ~0.80
- XGBoost + feature engineering: AUC ~0.85–0.87
- XGBoost + Optuna + stacking: AUC ~0.88–0.89

**Target: AUC > 0.85** = solid Silver-level execution. Above 0.87 = approaching Gold-level feature engineering.

---

## Practice (8h)

- Implement AUC-ROC from scratch (sort by score, compute trapezoid area)
- Full ML pipeline: feature engineering → cross-validation → model selection → submission
- Target: [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) — achieve AUC > 0.85

---

## Deliverable

`week07_evaluation_pipeline.ipynb` — AUC implementation + full pipeline + churn model

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
