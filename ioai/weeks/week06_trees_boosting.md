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
```python
import xgboost as xgb
from sklearn.model_selection import cross_val_score

model = xgb.XGBClassifier(
    n_estimators=500,
    max_depth=6,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    use_label_encoder=False,
    eval_metric='logloss',
    random_state=42
)
scores = cross_val_score(model, X, y, cv=5, scoring='roc_auc')
print(f"AUC: {scores.mean():.4f} ± {scores.std():.4f}")
```

### Optuna hyperparameter search
```python
import optuna

def objective(trial):
    params = {
        'n_estimators': trial.suggest_int('n_estimators', 100, 1000),
        'max_depth': trial.suggest_int('max_depth', 3, 10),
        'learning_rate': trial.suggest_float('learning_rate', 0.01, 0.3, log=True),
        'subsample': trial.suggest_float('subsample', 0.6, 1.0),
    }
    model = xgb.XGBClassifier(**params, eval_metric='logloss', random_state=42)
    return cross_val_score(model, X, y, cv=5, scoring='roc_auc').mean()

study = optuna.create_study(direction='maximize')
study.optimize(objective, n_trials=50)
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
