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
```python
# PCA from scratch — NumPy only
def pca(X, k):
    X_centered = X - X.mean(axis=0)
    cov = X_centered.T @ X_centered / (len(X) - 1)
    eigenvalues, eigenvectors = np.linalg.eigh(cov)
    # Sort descending
    idx = eigenvalues.argsort()[::-1]
    eigenvalues, eigenvectors = eigenvalues[idx], eigenvectors[:, idx]
    # Project onto top k components
    return X_centered @ eigenvectors[:, :k], eigenvectors[:, :k], eigenvalues

# Equivalently via SVD:
U, S, Vt = np.linalg.svd(X_centered, full_matrices=False)
# Principal components = Vt[:k].T; scores = U[:, :k] * S[:k]
```

### Why SVD matters for ML
- PCA: top-k eigenvectors = top-k right singular vectors
- Dimensionality reduction: project data onto top-k components
- Low-rank approximation: Aₖ = Σᵢ₌₁ᵏ σᵢ uᵢ vᵢᵀ (best rank-k approximation by Eckart-Young theorem)

---

## Practice (4h)

- Implement PCA from scratch using only NumPy (compute covariance matrix, eigendecomposition)
- Verify your PCA matches Scikit-learn's `PCA` on the Iris dataset
- Gold medalist exercise: apply PCA to a dataset, visualize 2D projection, identify cluster structure (IOAI 2025 "Antique Painting" core technique)

---

## Deliverable

`week04_pca_from_scratch.ipynb` — PCA implementation + Iris verification + cluster visualization

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
