# Weeks 21–24: Validation, Benchmarking, and Final Analysis
## Phase 4 — Core Research (Weeks 21–24 of 36)

---

## Objectives
- Lock in your final results (no more hyperparameter tuning after this)
- Run final evaluation on held-out test set (once, never again)
- Complete full benchmarking against all comparison methods
- Write the results and discussion sections of your paper

---

## The Final Evaluation Rule

**You may only evaluate on your test set ONCE — at the end.**

If you evaluate on the test set, tune based on what you see, and evaluate again, you've leaked the test set into your training process. This is called "test set contamination" and invalidates your results.

**Correct sequence**:
1. All hyperparameter tuning → validation set only
2. Final model selection → validation set only
3. **One final evaluation on test set → report this number, never revisit**

At ISEF, judges will ask "How did you ensure your results aren't biased?" Be ready to explain your train/val/test split and confirm the test set was held out.

---

## Complete Benchmarking Against Prior Work

### Comparison Table Structure
Your paper and poster need a table comparing your method to all relevant prior work:

```python
# Build this table carefully — verify every number from the original papers
comparison = {
    "Method": [
        "Method A (Author et al., 2021)",
        "Method B (Author et al., 2022)",
        "Method C (Author et al., 2023)",  # best prior work
        "Our Method",
    ],
    "Accuracy (%)": [81.2, 83.5, 85.1, 87.4],
    "F1 Score": [0.79, 0.82, 0.84, 0.87],
    "AUC": [0.88, 0.91, 0.93, 0.95],
    "Params (M)": [23.5, 45.2, 67.1, 28.3],  # model efficiency
    "Notes": [
        "Trained on same dataset",
        "Different preprocessing",
        "Concurrent work",
        ""
    ]
}
```

**Ethics note**: Report prior work numbers from their papers, not from your re-implementation, unless you explicitly re-implemented with the same setup (then report both).

---

## Week 21: Lock Final Results

Run the final evaluation pipeline:
1. Load the best checkpoint (saved during Week 11–16)
2. Run on test set
3. Record ALL metrics: accuracy, precision, recall, F1 (macro and micro), AUC-ROC, AUC-PR
4. Compute confidence intervals using bootstrap:

```python
from sklearn.utils import resample
from sklearn.metrics import accuracy_score, f1_score

def bootstrap_ci(y_true, y_pred, metric_fn, n_bootstrap=1000, ci=0.95):
    scores = []
    for _ in range(n_bootstrap):
        idx = resample(range(len(y_true)), random_state=None)
        scores.append(metric_fn(y_true[idx], y_pred[idx]))
    scores = sorted(scores)
    low = scores[int((1 - ci) / 2 * n_bootstrap)]
    high = scores[int((1 + ci) / 2 * n_bootstrap)]
    return np.mean(scores), low, high

mean_acc, low, high = bootstrap_ci(y_test, y_pred, accuracy_score)
print(f"Accuracy: {mean_acc:.4f} (95% CI: [{low:.4f}, {high:.4f}])")
```

**Report with confidence intervals in your paper**: "87.4% (95% CI: 86.1–88.7%)"

---

## Week 22: Comprehensive Metric Reporting

For different task types, compute appropriate metrics:

### Classification
```python
from sklearn.metrics import (
    accuracy_score, f1_score, precision_score, recall_score,
    roc_auc_score, average_precision_score, confusion_matrix,
    classification_report
)

metrics = {
    "accuracy": accuracy_score(y_test, y_pred),
    "macro_f1": f1_score(y_test, y_pred, average='macro'),
    "weighted_f1": f1_score(y_test, y_pred, average='weighted'),
    "macro_precision": precision_score(y_test, y_pred, average='macro'),
    "macro_recall": recall_score(y_test, y_pred, average='macro'),
    "auc_roc": roc_auc_score(y_test, y_prob, multi_class='ovr'),
    "auc_pr": average_precision_score(y_test, y_prob),
}
```

### For imbalanced datasets, prioritize:
- AUC-ROC (threshold-independent; handles class imbalance)
- F1 macro (treats all classes equally regardless of size)
- Per-class recall for minority classes

### For regression:
```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
metrics = {
    "rmse": mean_squared_error(y_test, y_pred, squared=False),
    "mae": mean_absolute_error(y_test, y_pred),
    "r2": r2_score(y_test, y_pred),
    "mape": np.mean(np.abs((y_test - y_pred) / y_test)) * 100,  # % error
}
```

---

## Week 23: Write Results and Discussion Sections

### Results Section (factual, no interpretation)
Report exactly what you measured. Do not explain why here.

Structure:
1. Dataset characteristics table (class distribution, train/val/test sizes)
2. Main results table (your method vs. all baselines)
3. Ablation study table
4. Cross-validation table (mean ± std across folds)
5. Generalization results (if you tested on an external dataset)
6. Visualizations: confusion matrix, ROC curve, learning curves, qualitative examples

```
Your method achieves 87.4 ± 0.3% accuracy (95% CI: 86.1–88.7%) on the [Dataset] test set,
compared to [Baseline] at 84.1 ± 0.4% (p=0.003, paired t-test, n=5 runs).
The improvement is consistent across all 5 cross-validation folds (range: 85.9–88.9%).
```

### Discussion Section (your interpretation)
Answer these 5 questions:

1. **What do the results mean?** Interpret your numbers in the context of the problem.
2. **Why does your approach work?** Link your ablation study to a mechanistic explanation.
3. **Where does it fail?** Reference your error analysis from Week 11–16.
4. **What are the limitations?** Dataset size? Generalizability? Computational cost? Lack of IRB for prospective validation?
5. **What are the future directions?** At least 3 specific next steps.

---

## Week 24: Complete Figure Package

Every figure used in the paper and poster should be publication quality.

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

# Publication-quality figure settings
mpl.rcParams.update({
    'font.size': 12,
    'axes.labelsize': 13,
    'axes.titlesize': 14,
    'xtick.labelsize': 11,
    'ytick.labelsize': 11,
    'figure.dpi': 150,
    'savefig.dpi': 300,
    'savefig.bbox': 'tight',
    'savefig.format': 'pdf',  # vector format for poster
})

# Required figures for ISEF poster:
# 1. Architecture diagram (your model schematic)
# 2. Main results bar chart with error bars
# 3. Ablation study bar chart
# 4. Learning curves (train vs val loss/accuracy)
# 5. Confusion matrix (normalized)
# 6. ROC curve (with AUC annotation)
# 7. Qualitative examples (what does your model do well/fail on?)
# 8. Interpretability visualization (Grad-CAM / SHAP)
```

---

## Weeks 21–24 Deliverables
- [ ] Final test set evaluation run (once, results recorded, never revisited)
- [ ] Full metric report with confidence intervals
- [ ] Comparison table vs. all prior work (numbers verified from papers)
- [ ] Results section written
- [ ] Discussion section written
- [ ] All 8 figures created at publication quality
- [ ] GitHub README updated with final results

## Final Performance Numbers (fill in and never change)
- **Primary metric (test)**: _______________
- **95% CI**: _______________
- **p-value vs. baseline**: _______________
- **Best prior published result**: _______________
- **Your improvement over prior**: _______________
