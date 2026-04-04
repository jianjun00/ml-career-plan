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

---

## Practice (8h)

- Compare SGD vs. Adam vs. AdamW on the same task; plot loss curves
- Implement dropout from scratch; verify expected behavior
- Implement batch normalization from scratch; compare to `torch.nn.BatchNorm1d`
- Practical: train a model that overfits, then fix it with dropout + weight decay

---

## Deliverable

`week10_optimization_regularization.ipynb` — optimizer comparison plots + BatchNorm from scratch

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
