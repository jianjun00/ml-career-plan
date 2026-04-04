# Week 6 — Trees: Decision Trees, Random Forests, Gradient Boosting
## Phase 1: Foundations (Week 6 of 8)

---

## Theory (6h)

- **Decision Trees**: information gain, Gini impurity, tree depth vs. overfitting
- **Random Forests**: bagging, feature subsampling, OOB error
- **Gradient Boosting**: additive model formulation, XGBoost vs. LightGBM specifics

**Resources**: CS229 notes + [XGBoost paper](https://arxiv.org/abs/1603.02754) (skim)

---

## Key Concepts

### Gini vs. Information Gain
```
Gini(t) = 1 - Σ p(cᵢ|t)²            # lower = purer split
Info Gain = H(parent) - Σ wᵢH(child_i)  # higher = better split
# In practice: use Gini for classification trees (faster, similar results)
```

### XGBoost / LightGBM quick-start template

> **Note**: `use_label_encoder` was deprecated in XGBoost 1.6 and removed in 2.0. Do not include it. `eval_metric` is now passed separately or at `fit()` time.

```python
import xgboost as xgb
import lightgbm as lgb
from sklearn.model_selection import cross_val_score
import numpy as np

# XGBoost (current API, no deprecated params)
xgb_model = xgb.XGBClassifier(
    n_estimators=500,
    max_depth=6,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42,
    n_jobs=-1
)
scores = cross_val_score(xgb_model, X, y, cv=5, scoring='roc_auc')
print(f"XGB AUC: {scores.mean():.4f} ± {scores.std():.4f}")

# LightGBM — faster on large datasets, often slightly better than XGBoost
lgb_model = lgb.LGBMClassifier(
    n_estimators=500,
    max_depth=6,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42,
    n_jobs=-1,
    verbose=-1
)
scores_lgb = cross_val_score(lgb_model, X, y, cv=5, scoring='roc_auc')
print(f"LGB AUC: {scores_lgb.mean():.4f} ± {scores_lgb.std():.4f}")
```

### Optuna hyperparameter search
```python
import optuna
optuna.logging.set_verbosity(optuna.logging.WARNING)

def objective(trial):
    params = {
        'n_estimators': trial.suggest_int('n_estimators', 100, 1000),
        'max_depth': trial.suggest_int('max_depth', 3, 12),
        'learning_rate': trial.suggest_float('learning_rate', 0.01, 0.3, log=True),
        'subsample': trial.suggest_float('subsample', 0.6, 1.0),
        'colsample_bytree': trial.suggest_float('colsample_bytree', 0.6, 1.0),
        'min_child_weight': trial.suggest_int('min_child_weight', 1, 10),
        'random_state': 42,
        'n_jobs': -1,
    }
    model = xgb.XGBClassifier(**params)
    return cross_val_score(model, X, y, cv=5, scoring='roc_auc').mean()

study = optuna.create_study(direction='maximize')
study.optimize(objective, n_trials=50, n_jobs=1)
print(f"Best AUC: {study.best_value:.4f}")
print(f"Best params: {study.best_params}")
```

---

## Practice (6h)

- Kaggle: [Intermediate ML course](https://www.kaggle.com/learn/intermediate-machine-learning) — complete all modules
- Tune XGBoost on a tabular competition using Optuna
- Submit to [House Prices](https://www.kaggle.com/c/house-prices-advanced-regression-techniques); target top 20%

---

## Deliverable

`week06_trees_boosting.ipynb` — decision tree scratch implementation + XGBoost + Optuna tuning

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
