# Week 1 — Python for ML & NumPy
## Phase 1: Foundations (Week 1 of 8)

---

## Theory (6h)

- NumPy broadcasting rules: understand axes, reshape, einsum
- Vectorized operations vs. loops: performance implications
- Resource: NumPy docs + [CS231n Python/NumPy tutorial](http://cs231n.github.io/python-numpy-tutorial/)

---

## Practice (6h)

- Implement matrix multiplication, softmax, cross-entropy loss in NumPy only (no libraries)
- Solve 10 NumPy exercises: [100 NumPy exercises](https://github.com/rougier/numpy-100)
- Time yourself: aim to implement softmax + cross-entropy in <15 minutes

### Key implementations to nail

```python
# Softmax (numerically stable)
def softmax(x):
    x = x - x.max(axis=-1, keepdims=True)  # subtract max for numerical stability
    e = np.exp(x)
    return e / e.sum(axis=-1, keepdims=True)

# Cross-entropy loss
def cross_entropy(probs, labels):
    n = labels.shape[0]
    return -np.log(probs[np.arange(n), labels] + 1e-9).mean()

# Matrix multiply (verify against np.dot)
def matmul(A, B):
    return np.einsum('ij,jk->ik', A, B)
```

---

## Deliverable

`week01_numpy_fundamentals.ipynb` — all three implementations + 10 NumPy exercises

---

## Time Target

- Softmax + cross-entropy from scratch: **< 15 minutes**
- Matrix multiply using einsum: **< 5 minutes**

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
