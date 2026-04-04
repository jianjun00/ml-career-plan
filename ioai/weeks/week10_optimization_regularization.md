# Week 10 — Optimization and Regularization
## Phase 2: Deep Learning Core (Week 10 of 18)

---

## Theory (4h)

- **Optimizers**: SGD with momentum, RMSProp, Adam — derive Adam update rules
- **Learning rate schedules**: cosine annealing, warmup, step decay
- **Regularization**: L1/L2 weight decay, dropout (derive expectation argument), batch normalization
- **Gradient issues**: vanishing/exploding gradients; gradient clipping

---

## Adam Update Rules (derive these)
```
m_t = β₁ m_{t-1} + (1-β₁) g_t          # first moment (momentum)
v_t = β₂ v_{t-1} + (1-β₂) g_t²         # second moment (RMS)
m̂_t = m_t / (1-β₁ᵗ)                    # bias correction
v̂_t = v_t / (1-β₂ᵗ)
θ_t = θ_{t-1} - α m̂_t / (√v̂_t + ε)    # update
# Defaults: β₁=0.9, β₂=0.999, ε=1e-8, α=1e-3
```

### LR Schedule implementations
```python
# Cosine annealing
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epochs)

# Linear warmup + cosine decay
from torch.optim.lr_scheduler import OneCycleLR
scheduler = OneCycleLR(optimizer, max_lr=1e-3, steps_per_epoch=len(train_loader), epochs=20)

# Step at each epoch end:
for epoch in range(epochs):
    train(...)
    scheduler.step()
```

### Dropout (derive the expectation)
```python
# During training: each neuron active with probability p
# Expected output = p * activation (so scale by 1/p at test time, or scale at train time)
# PyTorch's nn.Dropout(p) zeros activations with probability p, scales by 1/(1-p) at train time

class ModelWithDropout(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(256, 256)
        self.drop = nn.Dropout(0.3)   # 30% dropout
        self.fc2 = nn.Linear(256, 10)
    def forward(self, x):
        return self.fc2(self.drop(torch.relu(self.fc1(x))))
```

## Batch Normalization from Scratch (complete implementation)

BatchNorm has **learnable parameters** γ (scale) and β (shift) — many from-scratch implementations omit these and produce incorrect output.

```python
class BatchNorm1dScratch(nn.Module):
    def __init__(self, num_features, eps=1e-5, momentum=0.1):
        super().__init__()
        self.eps = eps
        self.momentum = momentum
        # Learnable parameters — REQUIRED for correctness
        self.gamma = nn.Parameter(torch.ones(num_features))   # scale
        self.beta  = nn.Parameter(torch.zeros(num_features))  # shift
        # Running stats for inference
        self.register_buffer('running_mean', torch.zeros(num_features))
        self.register_buffer('running_var',  torch.ones(num_features))

    def forward(self, x):
        if self.training:
            mean = x.mean(dim=0)
            var  = x.var(dim=0, unbiased=False)
            # Update running stats (used at inference time)
            self.running_mean = (1 - self.momentum) * self.running_mean + self.momentum * mean.detach()
            self.running_var  = (1 - self.momentum) * self.running_var  + self.momentum * var.detach()
        else:
            mean = self.running_mean
            var  = self.running_var
        x_norm = (x - mean) / torch.sqrt(var + self.eps)
        return self.gamma * x_norm + self.beta   # apply learnable scale and shift

# Verify: output should match nn.BatchNorm1d within 1e-5
bn_scratch = BatchNorm1dScratch(16)
bn_torch   = nn.BatchNorm1d(16)
# Copy weights: bn_torch.weight = bn_scratch.gamma, bn_torch.bias = bn_scratch.beta
x = torch.randn(32, 16)
assert torch.allclose(bn_scratch(x), bn_torch(x), atol=1e-5)
```

**Why gamma and beta matter**: Without them, BatchNorm forces every layer's output to zero-mean unit-variance, which destroys the representational capacity built up during training (e.g., a layer tuned to output large positive values). The learnable parameters let the network recover any distribution it needs.

---

## Practice (8h)

- Compare SGD vs. Adam vs. AdamW on the same task; plot loss curves. Record: which converges fastest? which generalizes best?
- Implement dropout from scratch; verify that at inference time the expected output equals training-time output (disable dropout, verify numerically)
- Implement `BatchNorm1dScratch` above; run the `assert torch.allclose` check — it must pass
- Practical: train a model that overfits (val loss diverges), then fix it with dropout + weight decay. Show before/after learning curves.

---

## Deliverable

`week10_optimization_regularization.ipynb` — optimizer comparison plots + BatchNorm from scratch

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
