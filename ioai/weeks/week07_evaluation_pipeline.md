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
X_encoded = enc.fit_transform(X, y)
```

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
