# Week 4 — Linear Algebra for ML
## Phase 1: Foundations (Week 4 of 8)

---

## Theory (8h)

- **Matrix operations**: multiplication, inversion, transpose, determinant
- **Eigenvalues/eigenvectors**: geometric interpretation, power iteration
- **SVD**: how it factors any matrix A = UΣVᵀ, connection to PCA
- **Norms**: L1, L2, Frobenius — when each matters

**Resources**: [3Blue1Brown Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) (all videos), Goodfellow Chapter 2

---

## Key Concepts

### PCA from SVD

**Data leakage warning**: Always fit PCA (compute mean and eigenvectors) on the **training set only**, then apply that same transform to the test set. Fitting on the full dataset leaks test-set distribution information into your model.

```python
# PCA from scratch — NumPy only (correct train/test split)
class PCAFromScratch:
    def fit(self, X_train):
        self.mean = X_train.mean(axis=0)          # fit mean on TRAIN ONLY
        X_c = X_train - self.mean
        cov = X_c.T @ X_c / (len(X_c) - 1)
        eigenvalues, eigenvectors = np.linalg.eigh(cov)
        idx = eigenvalues.argsort()[::-1]
        self.components = eigenvectors[:, idx]    # store principal components
        self.explained_variance = eigenvalues[idx]
        return self

    def transform(self, X, k):
        return (X - self.mean) @ self.components[:, :k]   # apply SAME mean

# Correct usage:
pca = PCAFromScratch()
pca.fit(X_train)                      # fit on train only
X_train_pca = pca.transform(X_train, k=2)
X_test_pca  = pca.transform(X_test,  k=2)   # reuse train mean — no leakage

# WRONG (leakage): pca.fit(np.vstack([X_train, X_test]))

# Equivalently via SVD (same leakage rule applies):
X_train_c = X_train - X_train.mean(axis=0)   # center on train
U, S, Vt = np.linalg.svd(X_train_c, full_matrices=False)
# To transform test: (X_test - X_train.mean(axis=0)) @ Vt[:k].T
```

### Why SVD matters for ML
- PCA: top-k eigenvectors = top-k right singular vectors
- Dimensionality reduction: project data onto top-k components
- Low-rank approximation: Aₖ = Σᵢ₌₁ᵏ σᵢ uᵢ vᵢᵀ (best rank-k approximation by Eckart-Young theorem)

---

## Practice (4h)

- Implement `PCAFromScratch` class above; verify `.transform(X_train)` matches `sklearn.decomposition.PCA` on Iris
- Deliberately introduce leakage (fit on full data) and show the numeric difference vs. proper train-only fit — understand why it matters
- Gold medalist exercise: apply PCA to a dataset, visualize 2D projection, identify cluster structure (IOAI 2025 "Antique Painting" core technique)

**Success criteria**: Your PCA projection matches sklearn's within 1e-5 absolute error (up to sign flip of components).

---

## Deliverable

`week04_pca_from_scratch.ipynb` — PCA implementation + Iris verification + cluster visualization

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
