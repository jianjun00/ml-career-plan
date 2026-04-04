# Week 5 — Classical ML: Linear & Logistic Regression, KNN
## Phase 1: Foundations (Week 5 of 8)

---

## Theory (6h)

- **Linear regression**: normal equation derivation, OLS, gradient descent form
- **Logistic regression**: sigmoid, cross-entropy loss, gradient derivation
- **KNN**: curse of dimensionality, distance metrics (Euclidean, Manhattan, cosine)

**Resources**: CS229 Lecture Notes 1–3

---

## Key Implementations

### Linear Regression (both methods)
```python
# Normal equation: θ = (XᵀX)⁻¹Xᵀy
theta_normal = np.linalg.pinv(X.T @ X) @ X.T @ y

# Gradient descent
def linear_regression_gd(X, y, lr=0.01, epochs=1000):
    m, n = X.shape
    theta = np.zeros(n)
    for _ in range(epochs):
        grad = X.T @ (X @ theta - y) / m
        theta -= lr * grad
    return theta
```

### Logistic Regression (NumPy only)
```python
def sigmoid(z): return 1 / (1 + np.exp(-z))

def logistic_regression(X, y, lr=0.1, epochs=1000):
    m, n = X.shape
    w = np.zeros(n)
    for _ in range(epochs):
        pred = sigmoid(X @ w)
        grad = X.T @ (pred - y) / m
        w -= lr * grad
    return w

# Gradient derivation: ∂L/∂w = Xᵀ(σ(Xw) - y) / m
# This follows from cross-entropy loss: L = -[y log σ + (1-y) log(1-σ)]
```

---

## Practice (6h)

- Implement both methods for linear regression; compare on a synthetic dataset
- Implement logistic regression in NumPy (no Scikit-learn); test on a 2D dataset
- Scikit-learn: fit all three models on 3 different Kaggle datasets; tune with cross-validation

---

## Deliverable

`week05_regression_knn.ipynb` — all three implementations with comparison plots

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
