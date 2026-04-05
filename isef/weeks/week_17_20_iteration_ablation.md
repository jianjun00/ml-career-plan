# Weeks 17–20: Iteration, Ablation, and Robustness
## Phase 4 — Core Research (Weeks 17–20 of 36)

---

## Objectives
- Deepen your best result with targeted improvements from error analysis
- Complete comprehensive ablation study
- Test robustness and generalization
- Prepare a "story" of your research that a judge can follow

---

## The Research Story Arc

By Week 20, your project must have a coherent narrative that answers these questions in order:
1. Why does this problem matter? (motivation)
2. What has been done? (prior work)
3. What doesn't work about current approaches? (gap)
4. What did you try? (your approach)
5. What did you learn from experiments that didn't work? (honest reporting)
6. What worked and by how much? (results)
7. Why does it work? (analysis/interpretation)
8. What are the limits? (limitations)
9. What would you do next? (future work)

If you can answer all 9 questions fluently, you'll handle any judge interview.

---

## Week 17: Targeted Improvement from Error Analysis

Based on your Week 11–16 error analysis, identify the largest failure mode and design one targeted fix.

**Common failure modes → targeted fixes**:

| Failure mode | Targeted fix |
|-------------|-------------|
| Class imbalance causing low recall on minority class | Class-weighted loss, oversampling (SMOTE), focal loss |
| Overfitting (train >> val accuracy) | Dropout, weight decay, data augmentation, early stopping |
| Poor performance on out-of-distribution examples | Domain adaptation, data augmentation with harder examples, test-time augmentation |
| Low confidence calibration (model over/under-confident) | Temperature scaling, label smoothing |
| High variance between runs (± > 2%) | Ensemble methods (average 3–5 models), better regularization |

Implement the fix, run the full evaluation pipeline, compare.

---

## Week 18: Comprehensive Ablation Study

Expand your ablation to cover everything in your model. This week's output should be a table you can point to in your poster and during the judge interview.

**Ablation study best practices**:
- Run each configuration N=5 times with different random seeds
- Report mean ± std, not just single runs
- Ablate one component at a time (control for everything else)
- Include a "sanity ablation": randomly initialized weights (should perform near random chance)

```python
import pandas as pd

results = {
    "Configuration": [
        "Random baseline",
        "Pretrained backbone only",
        "+ Your component A",
        "+ Your component B",
        "+ Your component C",
        "Full model (A+B+C)",
        "State-of-the-art (Paper X)",  # compare to best published
    ],
    "Val Accuracy": [...],
    "Std": [...],
    "Test Accuracy": [...],
    "Params (M)": [...],  # model complexity matters
    "Inference time (ms)": [...],  # practical efficiency matters
}

df = pd.DataFrame(results)
print(df.to_markdown())  # export for paper/poster
```

---

## Week 19: Generalization Testing

A strong ISEF project shows the approach generalizes, not just works on one split of one dataset.

### Test 1: K-Fold Cross-Validation
```python
from sklearn.model_selection import StratifiedKFold

kf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
fold_results = []

for fold, (train_idx, val_idx) in enumerate(kf.split(X, y)):
    # Train model on train_idx, evaluate on val_idx
    # Record val_acc, val_f1, val_auc
    fold_results.append({"fold": fold, "acc": acc, "f1": f1})

df = pd.DataFrame(fold_results)
print(f"Mean: {df['acc'].mean():.4f} ± {df['acc'].std():.4f}")
```

### Test 2: External Dataset (if available)
Train on Dataset A, test on Dataset B with similar but not identical distribution.
- If performance holds: strong generalization evidence
- If performance drops: identify why (domain shift? different demographics?)

### Test 3: Data Efficiency Curve
How much data do you need for your approach to work?
```python
# Train with increasing fractions of data
for frac in [0.1, 0.25, 0.5, 0.75, 1.0]:
    X_sub, _, y_sub, _ = train_test_split(X_train, y_train, train_size=frac, stratify=y_train)
    # Train and evaluate
    # Plot: frac → val_accuracy for baseline and your model
```

This shows whether your approach is especially beneficial in low-data regimes (important and impressive to judges).

---

## Week 20: Interpretability Analysis

ISEF judges in ML projects often ask: "But do you understand what your model is learning?" Prepare an interpretability analysis.

### For image models: Grad-CAM
```python
# pip install grad-cam
from pytorch_grad_cam import GradCAM
from pytorch_grad_cam.utils.image import show_cam_on_image

cam = GradCAM(model=model, target_layers=[model.layer4[-1]])
grayscale_cam = cam(input_tensor=input_tensor, targets=targets)
# Visualize: shows what regions the model focuses on
```

### For tabular/clinical models: SHAP
```python
import shap

explainer = shap.DeepExplainer(model, X_train_background)
shap_values = explainer.shap_values(X_test)
shap.summary_plot(shap_values, X_test, feature_names=feature_names)
```

### For NLP models: Attention Visualization
```python
# For transformer models, visualize attention weights
# BertViz: https://github.com/jessevig/bertviz
from bertviz import head_view
head_view(attention, tokens)
```

**Why this matters for ISEF**: Interpretability analysis shows judges you understand what the model is doing, not just that it achieves a number. It's also evidence of impact — a clinician won't trust a model they can't interpret.

---

## Weeks 17–20 Deliverables
- [ ] Targeted improvement from error analysis implemented and evaluated
- [ ] Comprehensive ablation table (all components, N=5 runs each, mean ± std)
- [ ] 5-fold cross-validation completed
- [ ] Data efficiency curve plotted
- [ ] Interpretability analysis (Grad-CAM / SHAP / attention visualization)
- [ ] Final best model checkpoint saved
- [ ] Complete results table ready for paper

## Final Results Summary
| Model | Accuracy | F1 | AUC | Params | Inference |
|-------|----------|----|-----|--------|-----------|
| Baseline | | | | | |
| Full model | | | | | |
| SOTA (paper ref) | | | | | |

**Final answer to the research question**:
> [Write your conclusion here: Did your approach work? By how much? Under what conditions?]
