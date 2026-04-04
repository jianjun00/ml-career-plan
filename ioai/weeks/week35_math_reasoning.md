# Week 35 — Mathematical Reasoning Under Pressure
## Phase 4: Pre-Competition Peak (Week 35 of 40)

**Gold medalists are distinguished by mathematical intuition, not just engineering skill.**

---

## Theory + Practice (12h)

### Core Derivations to Have Memorized

Derive each of these from scratch on paper (no notes). Time yourself — target < 10 minutes each.

| Derivation | Key Result |
|-----------|-----------|
| MLE for Gaussian | μ = x̄, σ² = Var(x) |
| SVM dual problem | max Σαᵢ - ½ΣΣαᵢαⱼyᵢyⱼ(xᵢᵀxⱼ) |
| Backprop for softmax + cross-entropy | δ = ŷ - y (gradient = prediction - label) |
| Attention gradient | ∂Attn/∂Q = softmax'(QKᵀ/√d)V (chain rule) |
| BatchNorm backward | involves mean and variance re-centering |
| PCA variance maximization | max wᵀΣw s.t. ||w||=1 → top eigenvector |
| Bayes optimal classifier | P(y=1|x) = P(x|y=1)P(y=1) / P(x) |

---

## Visual ML: 5-Minute Data Diagnosis

Given a scatter plot or dataset, correctly diagnose the structure in under 5 minutes:

### Pattern → Algorithm Mapping

| What you see | Algorithm |
|-------------|-----------|
| Two linearly separable blobs | Logistic regression, linear SVM |
| Concentric circles | RBF SVM, neural network |
| Spiral clusters | DBSCAN, MLP |
| Elongated ellipsoids | Gaussian Mixture Model, PCA + K-means |
| Outliers present | Isolation Forest, robust regression |
| Clear elbow in PCA variance | Choose components at elbow |
| t-SNE shows tight clusters | Clustering is viable |
| Noisy, overlapping | Ensemble methods, boosting |

### Practice Exercise
- Load 5 mystery datasets (no labels)
- Visualize in 2D (PCA + t-SNE)
- Write down your diagnosis and algorithm choice in < 5 minutes
- Verify against ground truth

---

## Practice Drills

- [ ] Derive all 7 results in the table above without looking
- [ ] For 5 mystery datasets: diagnose structure in <5 min, pick algorithm
- [ ] Given a loss curve (train/val plots), diagnose: overfitting? underfitting? data noise?
- [ ] Given a confusion matrix, identify: class imbalance? systematic errors? which class to focus on?

---

## Deliverable

`week35_math_drills.pdf` — 7 derivations + 5 dataset diagnoses with reasoning

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
