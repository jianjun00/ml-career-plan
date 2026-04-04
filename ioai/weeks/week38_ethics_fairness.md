# Week 38 — AI Safety, Fairness, and Ethics (Deep Dive)
## Phase 4: Pre-Competition Peak (Week 38 of 40)

---

## Theory (6h)

- **Algorithmic fairness**: case studies (COMPAS recidivism, hiring algorithms)
- **Fairness metrics**: demographic parity, equalized odds, calibration — why they conflict
- **AI safety**: alignment problem, reward hacking, specification gaming
- **Privacy**: differential privacy, federated learning basics
- **Explainability**: SHAP values, LIME, gradient-based attribution

---

## Fairness Metrics

### Demographic Parity
```
P(Ŷ=1 | A=0) = P(Ŷ=1 | A=1)
Positive prediction rate is equal across groups A=0 and A=1
```

### Equalized Odds (stronger)
```
P(Ŷ=1 | Y=y, A=0) = P(Ŷ=1 | Y=y, A=1)  for y ∈ {0,1}
Both TPR and FPR are equal across groups
```

### Why they conflict: Impossibility theorem
If base rates differ (P(Y=1|A=0) ≠ P(Y=1|A=1)), you cannot simultaneously have:
- Calibration
- Equal TPR
- Equal FPR
One must be sacrificed.

---

## SHAP Values (explain any model)
```python
import shap

# For tree models (fast):
explainer = shap.TreeExplainer(xgb_model)
shap_values = explainer.shap_values(X_test)

# Summary plot: global feature importance
shap.summary_plot(shap_values, X_test, feature_names=feature_names)

# Force plot: single prediction explanation
shap.force_plot(explainer.expected_value, shap_values[0], X_test.iloc[0])
```

## LIME (Local Interpretable Model-agnostic Explanations)
```python
from lime import lime_tabular

explainer = lime_tabular.LimeTabularExplainer(X_train, feature_names=feature_names, class_names=['0','1'])
exp = explainer.explain_instance(X_test[0], model.predict_proba, num_features=10)
exp.show_in_notebook()
```

---

## Practice (6h)

- Compute SHAP values for an XGBoost model; interpret feature importance plot
- Implement a fairness-aware classifier with demographic parity constraint
- Read: [Fairness and ML textbook](https://fairmlbook.org/) — Chapters 1–2

---

## Deliverable

`week38_ethics_fairness.ipynb` — SHAP analysis + fairness metric computation + constrained fair classifier

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
