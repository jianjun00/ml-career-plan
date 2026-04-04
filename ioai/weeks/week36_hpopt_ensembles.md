# Week 36 — Hyperparameter Optimization & AutoML Techniques
## Phase 4: Pre-Competition Peak (Week 36 of 40)

---

## Theory (4h)

- **Grid search vs. random search vs. Bayesian optimization** (Optuna)
- **Early stopping**: patience-based, ReduceLROnPlateau
- **Ensemble methods**: voting, stacking, blending — when to use each
- **Post-processing tricks**: calibration (Platt scaling, isotonic regression), threshold tuning

---

## Optuna: Bayesian Hyperparameter Search
```python
import optuna
from sklearn.model_selection import cross_val_score
import xgboost as xgb

def objective(trial):
    params = {
        'n_estimators': trial.suggest_int('n_estimators', 100, 2000),
        'max_depth': trial.suggest_int('max_depth', 3, 10),
        'learning_rate': trial.suggest_float('lr', 0.005, 0.3, log=True),
        'subsample': trial.suggest_float('subsample', 0.5, 1.0),
        'colsample_bytree': trial.suggest_float('colsample', 0.5, 1.0),
        'min_child_weight': trial.suggest_int('min_child_weight', 1, 10),
    }
    model = xgb.XGBClassifier(**params, eval_metric='auc', random_state=42)
    return cross_val_score(model, X_train, y_train, cv=5, scoring='roc_auc').mean()

study = optuna.create_study(direction='maximize')
study.optimize(objective, n_trials=100, n_jobs=-1)
best_params = study.best_params
```

## Stacking Ensemble
```python
from sklearn.model_selection import cross_val_predict

# Level 0: base models — get out-of-fold predictions
oof_xgb = cross_val_predict(xgb_model, X_train, y_train, cv=5, method='predict_proba')[:,1]
oof_rf = cross_val_predict(rf_model, X_train, y_train, cv=5, method='predict_proba')[:,1]
oof_lgb = cross_val_predict(lgb_model, X_train, y_train, cv=5, method='predict_proba')[:,1]

# Level 1: meta-learner trained on OOF predictions
meta_X = np.column_stack([oof_xgb, oof_rf, oof_lgb])
meta_model = LogisticRegression()
meta_model.fit(meta_X, y_train)

# Test: train base models on full train, predict test
test_preds = np.column_stack([xgb_model.predict_proba(X_test)[:,1],
                               rf_model.predict_proba(X_test)[:,1],
                               lgb_model.predict_proba(X_test)[:,1]])
final_pred = meta_model.predict_proba(test_preds)[:,1]
```

## F1 Threshold Optimization
```python
from sklearn.metrics import f1_score
# Find optimal threshold for F1 (instead of default 0.5)
thresholds = np.linspace(0, 1, 200)
f1_scores = [f1_score(y_val, (val_probs > t).astype(int)) for t in thresholds]
best_threshold = thresholds[np.argmax(f1_scores)]
```

---

## Practice (8h)

- Set up Optuna for automated hyperparameter tuning on 3 tasks; compare to manual tuning
- Implement a stacking ensemble (meta-learner on out-of-fold predictions)
- Implement threshold optimization for F1 score

---

## Deliverable

`week36_hpopt_ensembles.ipynb` — Optuna tuning + stacking results + F1 threshold optimization

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
