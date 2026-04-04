# Week 29 — Theoretical Depth: Goodfellow Chapters
## Phase 3: Advanced Topics + Competition Hardening (Week 29 of 30)

---

## Realistic Time Budget

The Goodfellow book chapters assigned here are ~150 pages total. Reading for understanding (not skimming) at 10–15 pages/hour = **10–15 hours of reading**, plus derivation practice. This week's total is **18 hours** across 7 days.

**Day-by-day plan:**

| Day | Chapter | Hours | Focus |
|-----|---------|-------|-------|
| Mon | Ch.5: ML Basics | 3h | Bias-variance, capacity, regularization concepts |
| Tue | Ch.6: Feedforward Networks | 3h | Activation functions, backprop derivation |
| Wed | Derivations drill 1 | 2h | Derive backprop + bias-variance on paper |
| Thu | Ch.7: Regularization | 3h | L1/L2 sparsity, dropout expectation |
| Fri | Ch.9 + Ch.10 | 3h | CNNs equivariance, LSTM gates |
| Sat | Derivations drill 2 | 2h | Derive dropout + LSTM equations |
| Sun | Summaries + review | 2h | Write summaries, identify gaps |

---

## Theory Reading Guide

Work through [Deep Learning book](https://www.deeplearningbook.org/) — the sections most tested at IOAI:

### Chapter 5: Machine Learning Basics (pages 96–161, ~30 pages core)
**Read**: Sections 5.1–5.4, 5.7–5.8. Skip 5.5–5.6 (maximum likelihood theory optional).
- Capacity, underfitting, overfitting — draw the capacity vs. error curves
- Bias-variance decomposition: `E[(y - ŷ)²] = Bias² + Variance + Noise`
- Regularization as weight decay: `L_reg = L + λ||θ||²`
- Hyperparameter selection: understand why test set must never influence model selection

### Chapter 6: Deep Feedforward Networks (pages 164–223, ~25 pages core)
**Read**: Sections 6.1–6.5. Skip 6.6 (historical).
- Activation functions: derive ReLU gradient (0 or 1), sigmoid gradient (σ(x)(1-σ(x))), tanh
- Backpropagation: write out the full forward and backward pass for a 2-layer network
- Universal approximation: conceptual understanding only — a 1-hidden-layer net can approximate any function given enough neurons

### Chapter 7: Regularization (pages 224–284, ~25 pages core)
**Read**: Sections 7.1–7.3, 7.5, 7.8, 7.12.
- L1 → sparsity (subgradient at 0 pulls weights to exactly 0)
- L2 → small weights (gradient always points toward 0, proportionally)
- Dropout: derive E[output_dropout] = p × output → equivalent to geometric mean of many networks

### Chapter 9: Convolutional Networks (pages 326–366, ~20 pages core)
**Read**: Sections 9.1–9.3, 9.7.
- Equivariance vs. invariance: learn the difference
- Pooling and receptive field growth

### Chapter 10: Sequence Modeling (pages 367–415, ~15 pages core)
**Read**: Sections 10.1–10.2, 10.7, 10.10. Read conceptually, not for implementation.
- RNN vanishing gradient: understand WHY (product of Jacobians < 1 over many steps)
- LSTM gates: learn the 4 gate equations; understand what each gate does

---

## Practice (5h — not 2h)

Derive these on paper from scratch. No notes. Allocate ~45 minutes each:

1. **Bias-variance decomposition**: expand `E[(f(x) - ŷ)²]` into Bias² + Variance + irreducible noise
2. **Backprop for 2-layer network**: given L = cross-entropy(softmax(W₂·ReLU(W₁·x))), derive ∂L/∂W₁ and ∂L/∂W₂
3. **Why L1 produces sparsity**: show that the subgradient of |w| at w=0 is [-1, +1], so gradient descent drives small weights to exactly 0 while L2 only drives them toward 0
4. **Dropout expectation**: if neuron i has activation aᵢ and dropout probability p, show E[ã_i] = (1-p)·aᵢ → therefore scale by 1/(1-p) at train time (inverted dropout)
5. **LSTM forward pass**: write all 4 equations from memory: fₜ (forget), iₜ (input), c̃ₜ (candidate), oₜ (output), then cₜ and hₜ

Write 1 paragraph per chapter summarizing the **single most important concept** in your own words.

---

## Deliverable

`week29_theory_depth.md` — 5 written derivations + 5 chapter summaries (1 paragraph each) + 3 IOAI-style theory questions you could now answer confidently

**Success criteria**: All 5 derivations completed without referencing the book. Chapter summaries written in your own words, not copied from the text.

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
