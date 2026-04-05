# Weeks 11–16: Core Experimentation
## Phase 4 — Core Research (Weeks 11–16 of 36)

---

## Objectives
- Implement and test your novel approach
- Run systematic experiments; log every result
- Achieve a result that beats the baseline on your primary metric
- Understand WHY your approach works (or doesn't)

---

## The Research Mindset for These 6 Weeks

ISEF judges at the top level do not just want to see "my model got X% accuracy." They want to see:
1. **A systematic investigation** — you tested multiple ideas, some worked, some didn't
2. **Understanding of mechanisms** — you can explain WHY your approach works
3. **Honest reporting** — including experiments that failed and what you learned
4. **Scientific rigor** — proper controls, multiple runs, statistical significance

Failed experiments do NOT hurt your project. Judges are impressed by a logbook full of failures thoughtfully analyzed — it shows genuine research process.

---

## Week-by-Week Experiment Plan Template

### Week 11: Implement Your Core Innovation
- Implement the novel component of your approach (new layer, new loss function, new architecture, new preprocessing)
- Run one experiment: baseline + your innovation vs. baseline alone
- Goal: first evidence that your idea has merit (even if small improvement)

### Week 12: Hyperparameter Sensitivity Study
- Vary the key hyperparameters of your innovation (1 at a time)
- Example: if you added a new attention mechanism, vary number of heads (1, 2, 4, 8)
- Plot performance vs. hyperparameter value
- Goal: understand what values work and why

### Week 13: Systematic Ablation Study
- Remove/add one component at a time to understand contribution of each
- Goal: show judges you understand which part of your approach drives the improvement

### Week 14: Robustness and Generalization Tests
- Test on different data splits (5-fold cross-validation minimum)
- Test on a different but related dataset if available
- Goal: show your approach generalizes; not just lucky on one split

### Week 15: Error Analysis
- Look at what your model gets wrong — not just aggregate accuracy
- Classify errors: what types of examples fail? Is there a pattern?
- Goal: understand failure modes; may suggest further improvement

### Week 16: Final Improvement Iteration
- Based on error analysis: implement one more targeted improvement
- Re-run full evaluation pipeline
- Goal: final best result for the paper

---

## Ablation Study Template

An ablation study removes one component at a time to measure its contribution:

```python
# Example: Your model = Baseline + ComponentA + ComponentB + ComponentC
# Ablations:
experiments = {
    "baseline":                    {},  # just the baseline
    "baseline+A":                  {"component_a": True},
    "baseline+B":                  {"component_b": True},
    "baseline+C":                  {"component_c": True},
    "baseline+A+B":                {"component_a": True, "component_b": True},
    "baseline+A+C":                {"component_a": True, "component_c": True},
    "baseline+B+C":                {"component_b": True, "component_c": True},
    "full_model (A+B+C)":          {"component_a": True, "component_b": True, "component_c": True},
}
# Run each experiment N=3 times with different seeds
# Report mean ± std for each

# Present as a table:
# | Model | Accuracy | F1 | AUC |
# |-------|----------|----|-----|
# | Baseline | 84.1 ± 0.3 | ... | ... |
# | +ComponentA | 85.7 ± 0.2 | ... | ... |
# | +ComponentB | 84.9 ± 0.4 | ... | ... |
# | Full Model | 87.2 ± 0.2 | ... | ... |
```

**What this tells judges**: Which components actually matter. If removing A drops performance by 3% but removing B drops it by 0.1%, A is the key innovation.

---

## Statistical Significance

Do not report a result without knowing if it's statistically significant. A 0.5% improvement is meaningless if your standard deviation is ±1%.

```python
from scipy import stats

# Run each model 5 times with different seeds
model_a_scores = [0.847, 0.851, 0.843, 0.849, 0.846]  # baseline
model_b_scores = [0.862, 0.865, 0.858, 0.863, 0.861]  # your model

# Paired t-test
t_stat, p_value = stats.ttest_rel(model_a_scores, model_b_scores)
print(f"p-value: {p_value:.4f}")
# p < 0.05 → statistically significant improvement
# p >= 0.05 → improvement may be due to random chance

# Report as: "Our model achieves 86.2 ± 0.3% vs. baseline 84.7 ± 0.3%
# (p=0.002, paired t-test, n=5 runs)"
```

---

## Error Analysis Framework

```python
# After getting predictions on validation set:
import numpy as np
from sklearn.metrics import confusion_matrix, classification_report
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Confusion matrix
cm = confusion_matrix(y_true, y_pred)
plt.figure(figsize=(10, 8))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
plt.xlabel('Predicted'); plt.ylabel('True')
plt.savefig('confusion_matrix.png', dpi=150, bbox_inches='tight')

# 2. Per-class metrics
print(classification_report(y_true, y_pred, target_names=class_names))

# 3. Find hardest examples
# Collect probabilities and find low-confidence correct + high-confidence wrong
probs = model_probs  # shape: (N, n_classes)
confidence = probs.max(axis=1)
is_wrong = (y_pred != y_true)

# High-confidence mistakes — most informative for error analysis
hard_examples_idx = np.argsort(confidence[is_wrong])[-20:]
# Visualize or describe these examples — what do they have in common?
```

**Questions to answer during error analysis**:
- Is there a systematic pattern in errors? (e.g., always confuses class 3 and class 7)
- Are errors concentrated in a specific data subgroup?
- Is the model confident when it's wrong? (calibration problem)
- What would a human need to distinguish these cases?

---

## Keeping Your Research Log During Experiments

Every experiment session, record:
```
Date: ___
Experiment name and W&B URL: ___
Hypothesis: "I expect X because Y"
Config changes from last run: ___
Result: ___
Did it match expectation? Yes / No
If no: why might the result differ from expectation?
Next hypothesis to test: ___
```

This is what judges want to see. A logbook showing "I thought X, tested it, got Y, and concluded Z" is direct evidence of scientific thinking.

---

## Weeks 11–16 Deliverables
- [ ] Novel approach implemented and running
- [ ] Full ablation study with table (each component tested)
- [ ] 5-fold cross-validation results
- [ ] Statistical significance test for main result (p-value reported)
- [ ] Error analysis with confusion matrix and failure case analysis
- [ ] Research log updated for every experiment session
- [ ] W&B dashboard showing all experiment runs
- [ ] GitHub repo with clean code and README

## Results Summary Table
| Experiment | Val Acc (mean ± std) | Test Acc | p-value vs baseline |
|-----------|---------------------|----------|---------------------|
| Baseline | | | — |
| Your model | | | |
| Ablation A | | | |
| Ablation B | | | |
