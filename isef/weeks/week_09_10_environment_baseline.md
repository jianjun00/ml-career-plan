# Weeks 9–10: Environment Setup + Baseline Implementation
## Phase 3 — Environment Setup (Weeks 9–10 of 36)

---

## Objectives
- Fully working experiment pipeline: data → model → metrics → logged results
- Baseline model implemented, verified, and reproducible
- Experiment tracking set up so every run is logged
- First result you can show a judge: "Here is the baseline I'm trying to beat"

---

## Complete ML Experiment Stack

Every experiment you run from here on must be **fully reproducible**. This is both a research integrity requirement and an ISEF judging criterion.

```python
# ============================================================
# ISEF-READY EXPERIMENT TEMPLATE
# Save this as your project's main experiment file
# ============================================================

import torch
import torch.nn as nn
import numpy as np
import random
import os
import wandb  # pip install wandb
from datetime import datetime

# ── Reproducibility ──────────────────────────────────────────
SEED = 42
torch.manual_seed(SEED)
torch.cuda.manual_seed_all(SEED)
np.random.seed(SEED)
random.seed(SEED)
torch.backends.cudnn.deterministic = True

# ── Config (change these per experiment) ─────────────────────
config = {
    "project_name": "isef_[your_project]",
    "experiment_name": "baseline_resnet50",
    "seed": SEED,
    "batch_size": 32,
    "learning_rate": 1e-3,
    "epochs": 50,
    "model": "resnet50",
    "dataset": "your_dataset",
    "train_size": 0.7,
    "val_size": 0.1,
    "test_size": 0.2,
}

# ── Experiment Tracking ───────────────────────────────────────
wandb.init(
    project=config["project_name"],
    name=config["experiment_name"],
    config=config
)

# ── Data Loading ──────────────────────────────────────────────
DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f"Device: {DEVICE}")

# [Load your dataset here]
# train_loader, val_loader, test_loader = ...

# ── Model ─────────────────────────────────────────────────────
# [Define your model here]
# model = YourModel(...).to(DEVICE)

# ── Training Loop ─────────────────────────────────────────────
optimizer = torch.optim.Adam(model.parameters(), lr=config["learning_rate"])
criterion = nn.CrossEntropyLoss()
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=config["epochs"])

best_val_acc, best_state = 0, None
for epoch in range(config["epochs"]):
    # Train
    model.train()
    train_loss = 0
    for X, y in train_loader:
        X, y = X.to(DEVICE), y.to(DEVICE)
        optimizer.zero_grad()
        loss = criterion(model(X), y)
        loss.backward()
        optimizer.step()
        train_loss += loss.item()

    # Validate
    model.eval()
    correct, total = 0, 0
    val_loss = 0
    with torch.no_grad():
        for X, y in val_loader:
            X, y = X.to(DEVICE), y.to(DEVICE)
            out = model(X)
            val_loss += criterion(out, y).item()
            correct += (out.argmax(1) == y).sum().item()
            total += y.size(0)
    val_acc = correct / total

    # Log to W&B
    wandb.log({
        "epoch": epoch,
        "train_loss": train_loss / len(train_loader),
        "val_loss": val_loss / len(val_loader),
        "val_acc": val_acc,
        "lr": scheduler.get_last_lr()[0]
    })

    if val_acc > best_val_acc:
        best_val_acc = val_acc
        best_state = {k: v.clone() for k, v in model.state_dict().items()}
        torch.save(best_state, f"checkpoints/{config['experiment_name']}_best.pt")

    scheduler.step()
    print(f"Epoch {epoch+1}: val_acc={val_acc:.4f} | best={best_val_acc:.4f}")

# ── Final Evaluation on Test Set ──────────────────────────────
# ONLY run this once, at the end. Never tune hyperparameters based on test set.
model.load_state_dict(best_state)
model.eval()
# [Compute final metrics here]
# wandb.log({"test_acc": test_acc, "test_f1": test_f1, "test_auc": test_auc})
wandb.finish()
```

---

## Baseline Verification Protocol

Before building your novel approach, verify your baseline is correctly implemented.

### Verification Checklist
1. **Match reported numbers**: Run baseline on a standard split; compare to paper's reported accuracy
   - Acceptable tolerance: ±0.5% for classification, ±1% for other metrics
   - If gap > 1%: investigate before proceeding (wrong data split? wrong hyperparameters? wrong metric?)

2. **Sanity checks**:
```python
# Check 1: Loss decreases (model is learning)
assert train_loss_epoch_10 < train_loss_epoch_1, "Loss not decreasing — check optimizer/LR"

# Check 2: Better than random
n_classes = 10
random_baseline = 1.0 / n_classes  # 0.10 for 10 classes
assert val_acc > random_baseline * 1.5, "Not beating random baseline"

# Check 3: No data leakage
# Test set indices must NEVER appear in training set
assert len(set(train_idx) & set(test_idx)) == 0

# Check 4: Class distribution preserved
from collections import Counter
print(Counter(y_train))  # Should match overall class distribution
print(Counter(y_test))
```

3. **Learning curves look reasonable**:
   - Training loss decreases smoothly
   - Validation loss decreases then plateaus (not explodes)
   - Train and val accuracy converge within ~5% of each other at plateau

---

## Experiment Log Format

Every experiment in your research log should follow this format. You will show this to judges.

```
Date: 2025-09-15
Experiment: baseline_resnet50_v1
Config: LR=1e-3, batch=32, epochs=50, augmentation=none
Result: val_acc=0.847, test_acc=0.841
W&B run: [URL]

Observations:
- Loss converges well; no overfitting signs
- Class 3 has low recall (0.62) — imbalanced class
- Training time: 45 min on Colab T4

Next steps:
- Try class-weighted loss to handle imbalance
- Try data augmentation to improve recall
```

---

## Weeks 9–10 Deliverables
- [ ] Data pipeline working: can load, split, and iterate over batches
- [ ] Baseline model implemented and running
- [ ] Baseline matches published accuracy (within 0.5%)
- [ ] Weights & Biases set up; baseline run logged
- [ ] First learning curve plot saved
- [ ] 3 sanity checks passing
- [ ] GitHub repo updated with clean, commented code
- [ ] Research log updated daily

## Baseline Results Table
| Model | Dataset | Train Acc | Val Acc | Test Acc | Paper's reported |
|-------|---------|-----------|---------|----------|-----------------|
| | | | | | |
