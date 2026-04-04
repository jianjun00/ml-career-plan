# Week 0 — Diagnostic Baseline
## Phase 0: Before Starting

Before beginning the 52-week program, assess your current level to identify which phases to accelerate or slow down.

---

## Diagnostic Tasks

- [ ] Run through IOAI 2024 Day 1 Task 1 cold (no prep). Score it.
- [ ] Complete a Kaggle Titanic-style problem end to end in under 90 minutes.
- [ ] Implement a 3-layer MLP in pure PyTorch from scratch (no tutorials).
- [ ] Derive backpropagation for a 2-layer network on paper.

---

## How to Interpret Results

| Result | Meaning |
|--------|---------|
| IOAI task scored above bronze baseline | Phase 1 can be accelerated — spend 4 weeks instead of 8 |
| Titanic done in <90 min, top 25% | Pandas/EDA foundations solid; skip Week 2 deep dives |
| MLP in PyTorch correct, <30 min | Phase 2 Weeks 9–10 can be condensed |
| Backprop derivation correct from memory | Strong math foundations; Week 3–4 can be review-only |

---

## Environment Setup (Do This First)

```bash
pip install torch torchvision numpy pandas scikit-learn matplotlib seaborn xgboost lightgbm optuna transformers datasets
```

Verify:
```python
import torch; print(torch.__version__)   # should be 2.x
import sklearn; print(sklearn.__version__)
```

---

*Part of the IOAI Gold Medal 52-Week Training Program*
