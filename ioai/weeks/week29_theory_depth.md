# Week 29 — Theoretical Depth: Goodfellow Chapters
## Phase 3: Advanced Topics + Competition Hardening (Week 29 of 30)

---

## Theory (10h)

Work through [Deep Learning book](https://www.deeplearningbook.org/) — the sections most tested at IOAI theoretical questions:

### Chapter 5: Machine Learning Basics
- Capacity, underfitting, overfitting
- Bias-variance decomposition: `E[(y - ŷ)²] = Bias² + Variance + Noise`
- Regularization as weight decay: `L_reg = L + λ||θ||²`
- Hyperparameter selection: train/validation/test split discipline

### Chapter 6: Deep Feedforward Networks
- Activation functions and their gradients (derive for ReLU, sigmoid, tanh)
- Universal approximation theorem (conceptual)
- Backpropagation algorithm: forward pass stores activations, backward pass computes gradients

### Chapter 7: Regularization
- L1 regularization → sparse solutions (why? gradient points toward origin for small weights)
- L2 regularization → small weights (shrinks toward zero proportionally)
- Dropout: equivalent to training exponentially many models and averaging
- Data augmentation as regularization
- Multi-task learning: shared representations improve generalization

### Chapter 9: Convolutional Networks
- Equivariance vs. invariance: convolution is equivariant to translation; pooling adds invariance
- Pooling reduces spatial dimensions while increasing receptive field
- Why CNNs work for images: local structure, translation invariance, compositionality

### Chapter 10: Sequence Modeling (Conceptual)
- RNN: hidden state carries information through time; gradient vanishing over long sequences
- LSTM: gates (forget, input, output) control information flow; solves vanishing gradient
- Why Transformers replaced RNNs: parallelism + long-range dependencies via attention

---

## Practice (2h)

- Derive 5 results from the book on paper without looking:
  1. Bias-variance decomposition
  2. Backprop chain rule for a 2-layer network
  3. Why L1 produces sparsity (subgradient argument)
  4. Dropout expectation calculation
  5. LSTM forward pass equations

- Write 1-paragraph summaries of each chapter's key insight

---

## Deliverable

`week29_theory_depth.md` — derivations + chapter summaries + 3 questions you'd ask at IOAI theory section

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
