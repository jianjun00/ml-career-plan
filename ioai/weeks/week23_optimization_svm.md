# Week 23 — Constrained Optimization and Mathematical ML
## Phase 3: Advanced Topics + Competition Hardening (Week 23 of 30)

**IOAI relevance**: "Pixel Parsimony" (IOAI 2025) was a constrained optimization problem.

---

## Theory (6h)

- **Convex optimization**: gradient descent convergence, convexity conditions
- **Constrained optimization**: Lagrange multipliers (SVM dual derivation)
- **SVM**: hard margin, soft margin, kernel trick (RBF, polynomial), dual problem
- **Combinatorial optimization basics**: Hungarian algorithm for matching

---

## SVM Dual Derivation (key for IOAI theory questions)
```
Primal: min 1/2 ||w||² s.t. yᵢ(wᵀxᵢ + b) ≥ 1 ∀i

Lagrangian: L = 1/2||w||² - Σαᵢ(yᵢ(wᵀxᵢ+b) - 1)

KKT conditions:
  ∂L/∂w = 0 → w = Σαᵢyᵢxᵢ
  ∂L/∂b = 0 → Σαᵢyᵢ = 0

Dual: max Σαᵢ - 1/2 ΣΣαᵢαⱼyᵢyⱼ(xᵢᵀxⱼ) s.t. αᵢ≥0, Σαᵢyᵢ=0

Kernel trick: replace xᵢᵀxⱼ with K(xᵢ,xⱼ) — never compute high-dim features explicitly
```

## scipy.optimize for constrained problems
```python
from scipy.optimize import minimize

# Minimize f(x) subject to g(x) = 0 and h(x) >= 0
result = minimize(
    fun=lambda x: x[0]**2 + x[1]**2,          # objective: min x² + y²
    x0=[1.0, 1.0],
    method='SLSQP',
    constraints=[
        {'type': 'eq', 'fun': lambda x: x[0] + x[1] - 1},  # x + y = 1
        {'type': 'ineq', 'fun': lambda x: x[0]},            # x >= 0
    ],
    bounds=[(0, None), (0, None)]
)
print(result.x)   # constrained minimum
```

## Hungarian Algorithm (for assignment problems)
```python
from scipy.optimize import linear_sum_assignment

# Cost matrix: cost[i][j] = cost of assigning worker i to task j
cost_matrix = np.array([[4,1,3],[2,0,5],[3,2,2]])
row_ind, col_ind = linear_sum_assignment(cost_matrix)
print(f"Optimal assignment: {list(zip(row_ind, col_ind))}, cost: {cost_matrix[row_ind, col_ind].sum()}")
```

---

## Practice (6h)

- Derive the SVM dual problem from scratch on paper
- Practice [IOAI 2025 "Pixel Parsimony/Efficiency" task](https://github.com/IOAI-official/IOAI-2025)
- Implement a constrained optimization solver using `scipy.optimize`

---

## Deliverable

`week23_optimization_svm.ipynb` — SVM derivation notes + constrained optimizer + Hungarian example

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
