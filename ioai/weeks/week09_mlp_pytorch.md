# Week 9 — Neural Networks from Scratch + PyTorch Basics
## Phase 2: Deep Learning Core (Week 9 of 18)

---

## Theory (6h)

- **Forward pass**: activation functions (ReLU, sigmoid, tanh, GELU) — pros/cons
- **Backpropagation**: chain rule derivation for a 3-layer network
- **Computational graphs**: how PyTorch autograd builds them

**Resources**: Goodfellow Chapter 6; [Andrej Karpathy's micrograd](https://github.com/karpathy/micrograd)

---

## Key Concepts

### Activation function comparison

| Activation | Range | Vanishing gradient? | Use case |
|-----------|-------|---------------------|---------|
| Sigmoid | (0,1) | Yes (saturates) | Binary output only |
| Tanh | (-1,1) | Yes (saturates) | Hidden layers (rarely) |
| ReLU | [0,∞) | No (for x>0) | Default for hidden layers |
| GELU | ~ReLU | No | Transformers |
| LeakyReLU | (-∞,∞) | No | When dying ReLU is problem |

### PyTorch training loop template
```python
import torch
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, in_dim, hidden, out_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, hidden), nn.ReLU(),
            nn.Linear(hidden, hidden), nn.ReLU(),
            nn.Linear(hidden, out_dim)
        )
    def forward(self, x): return self.net(x)

model = MLP(784, 256, 10)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()

for epoch in range(20):
    for X_batch, y_batch in train_loader:
        optimizer.zero_grad()
        loss = criterion(model(X_batch), y_batch)
        loss.backward()
        optimizer.step()
```

---

## Practice (6h)

- Clone and study Karpathy's micrograd — understand every line
- Build a 3-layer MLP in pure PyTorch (`torch.nn.Module`); train on MNIST
- Achieve >98% accuracy on MNIST with your custom MLP
- Time it: can you write the full training loop from scratch in <20 minutes?

---

## Deliverable

`week09_mlp_pytorch.ipynb` — micrograd study notes + MNIST MLP achieving >98% accuracy

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
