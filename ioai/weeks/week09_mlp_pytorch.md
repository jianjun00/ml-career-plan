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

### PyTorch training loop template (with reproducibility and GPU support)
```python
import torch
import torch.nn as nn
import numpy as np
import random

# --- Reproducibility: set ALL seeds ---
SEED = 42
torch.manual_seed(SEED)
torch.cuda.manual_seed_all(SEED)
np.random.seed(SEED)
random.seed(SEED)
torch.backends.cudnn.deterministic = True
torch.backends.cudnn.benchmark = False   # set True for speed if input sizes are fixed

DEVICE = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print(f"Using device: {DEVICE}")

class MLP(nn.Module):
    def __init__(self, in_dim, hidden, out_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, hidden), nn.ReLU(),
            nn.Linear(hidden, hidden), nn.ReLU(),
            nn.Linear(hidden, out_dim)
        )
    def forward(self, x): return self.net(x)

model = MLP(784, 256, 10).to(DEVICE)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
criterion = nn.CrossEntropyLoss()

best_val_acc, best_state = 0, None
for epoch in range(20):
    model.train()
    for X_batch, y_batch in train_loader:
        X_batch, y_batch = X_batch.to(DEVICE), y_batch.to(DEVICE)
        optimizer.zero_grad()
        loss = criterion(model(X_batch), y_batch)
        loss.backward()
        optimizer.step()

    # Validation
    model.eval()
    with torch.no_grad():
        correct = sum((model(X.to(DEVICE)).argmax(1) == y.to(DEVICE)).sum().item()
                      for X, y in val_loader)
        val_acc = correct / len(val_loader.dataset)
    if val_acc > best_val_acc:
        best_val_acc = val_acc
        best_state = {k: v.clone() for k, v in model.state_dict().items()}
    print(f"Epoch {epoch+1}: val_acc={val_acc:.4f}")

model.load_state_dict(best_state)  # restore best checkpoint
```

**Why set all seeds**: PyTorch, NumPy, and Python's random module use separate RNGs. Setting only one leaves the others non-deterministic. At IOAI, you want fully reproducible results between runs.

## PyTorch Debugging Guide (read this before you spend 2 hours stuck)

The most common bugs when implementing models from scratch:

```python
# 1. Shape mismatch — always print shapes during debugging
x = torch.randn(32, 784)
print(x.shape)            # add these everywhere when debugging
out = model(x)
print(out.shape)          # expected (32, 10) for 10-class classification

# 2. Device mismatch — all tensors must be on the same device
# Error: "Expected all tensors to be on the same device"
model = model.to(DEVICE)
X = X.to(DEVICE)          # move data to same device as model
y = y.to(DEVICE)

# 3. Gradient not flowing — check with requires_grad
x = torch.randn(4, 784, requires_grad=True)
loss = criterion(model(x), y)
loss.backward()
print(model.net[0].weight.grad)   # None = gradient didn't reach this layer

# 4. NaN loss — usually caused by exploding gradients or log(0)
if torch.isnan(loss):
    print("NaN loss detected!")
    # Fix: add gradient clipping
    torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)

# 5. Model stuck in training mode during eval
model.eval()    # switches Dropout/BatchNorm to inference mode
with torch.no_grad():   # disables gradient computation
    output = model(X_test)
model.train()   # switch back for next training loop

# 6. Wrong loss for your task
# Binary classification → nn.BCEWithLogitsLoss (NOT BCELoss — numerically unstable)
# Multi-class → nn.CrossEntropyLoss (applies softmax internally — don't apply softmax before it)
# Regression → nn.MSELoss or nn.L1Loss
```

---

## Practice (6h)

- Clone and study Karpathy's micrograd — understand every line; re-implement the `Value` class from memory
- Build a 3-layer MLP in pure PyTorch (`torch.nn.Module`); train on MNIST with the full template above including seeds and GPU support
- Achieve >98% accuracy on MNIST — a 3-layer MLP with ReLU achieves 98.2%; above 99% requires BatchNorm or CNN (try both)
- Time it: can you write the full training loop from scratch in <20 minutes? (The template above with no referencing is the target.)

**Success criteria** (bronze/silver/gold):
- Bronze: MLP trains, converges, reaches >97% MNIST accuracy
- Silver: >98% MNIST accuracy + seeds set + GPU-compatible + val loss tracked
- Gold: >98.5% with learning curve plotted showing no overfitting + <20 min to write from memory

---

## Deliverable

`week09_mlp_pytorch.ipynb` — micrograd study notes + MNIST MLP with full training template + learning curves

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
