# Week 3 — Probability, Statistics, and Information Theory
## Phase 1: Foundations (Week 3 of 8)

---

## Theory (8h)

- **Probability**: conditional probability, Bayes theorem, law of total probability
- **Distributions**: Normal, Binomial, Poisson, Bernoulli — PDFs, CDFs, moments
- **MLE** (Maximum Likelihood Estimation): derive for Gaussian and Bernoulli
- **Information theory**: entropy, KL divergence, cross-entropy — derive the connections

**Resources**: Stanford CS229 notes (Lecture 1–3), [Goodfellow Chapter 3](https://www.deeplearningbook.org/)

---

## Key Derivations (do these on paper)

### MLE for Gaussian
Given i.i.d. samples x₁, …, xₙ from N(μ, σ²):
```
log L(μ,σ) = -n/2 log(2πσ²) - Σ(xᵢ-μ)²/(2σ²)
∂log L/∂μ = 0 → μ_MLE = (1/n)Σxᵢ
∂log L/∂σ² = 0 → σ²_MLE = (1/n)Σ(xᵢ-μ)²
```

### Cross-entropy = Negative Log-Likelihood
```
H(p,q) = -Σ p(x) log q(x)
NLL = -log P(data|θ) = -Σ log q(xᵢ)
When p = empirical distribution: H(p,q) = NLL → same objective
```

### KL Divergence
```
KL(P||Q) = Σ P(x) log(P(x)/Q(x)) = H(P,Q) - H(P) ≥ 0
Minimizing cross-entropy = minimizing KL divergence from data distribution
```

---

## Practice (4h)

- Implement MLE estimation for a Gaussian from scratch (NumPy only)
- Show mathematically why cross-entropy loss = negative log-likelihood
- Solve 5 probability problems from AMC/AIME level as warm-up

---

## Deliverable

Written derivations in `week03_probability_stats.pdf` or LaTeX — at minimum: MLE for Gaussian, cross-entropy = NLL proof, Bayes theorem applied to a classification example

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
