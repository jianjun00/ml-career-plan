# Week 19 — Advanced PyTorch: Custom Training Loops & Efficiency
## Phase 3: Advanced Topics + Competition Hardening (Week 19 of 30)

---

## Theory (4h)

- **Mixed precision training**: float16 vs. bfloat16, `torch.cuda.amp.autocast`
- **Gradient accumulation**: simulate larger batch sizes on limited GPU memory
- **Custom loss functions**: when to subclass `nn.Module`
- **DataLoader optimization**: num_workers, pin_memory, prefetching
- **Profiling**: `torch.profiler`, identifying bottlenecks

---

## Mixed Precision Training
```python
from torch.cuda.amp import GradScaler, autocast

scaler = GradScaler()
for X, y in loader:
    optimizer.zero_grad()
    with autocast():                          # runs forward in float16
        loss = criterion(model(X), y)
    scaler.scale(loss).backward()             # scale loss to avoid underflow
    scaler.step(optimizer)
    scaler.update()
# Typically 1.5–2x speedup, ~50% memory reduction
```

## Gradient Accumulation
```python
accumulation_steps = 4   # simulate batch_size * 4 effective batch

for i, (X, y) in enumerate(loader):
    with autocast():
        loss = criterion(model(X), y) / accumulation_steps
    scaler.scale(loss).backward()

    if (i + 1) % accumulation_steps == 0:
        scaler.step(optimizer)
        scaler.update()
        optimizer.zero_grad()
```

## Custom Loss Functions
```python
class FocalLoss(nn.Module):
    def __init__(self, alpha=0.25, gamma=2.0):
        super().__init__()
        self.alpha, self.gamma = alpha, gamma
    def forward(self, pred, target):
        bce = F.binary_cross_entropy_with_logits(pred, target, reduction='none')
        pt = torch.exp(-bce)
        return (self.alpha * (1 - pt) ** self.gamma * bce).mean()

class DiceLoss(nn.Module):
    def forward(self, pred, target, smooth=1):
        pred = torch.sigmoid(pred)
        intersection = (pred * target).sum()
        return 1 - (2*intersection + smooth) / (pred.sum() + target.sum() + smooth)

class ContrastiveLoss(nn.Module):
    def __init__(self, margin=1.0):
        super().__init__()
        self.margin = margin
    def forward(self, d, label):
        # d = L2 distance, label = 1 if same class, 0 if different
        return (label * d**2 + (1-label) * torch.clamp(self.margin - d, min=0)**2).mean()
```

---

## Practice (8h)

- Rewrite a previous model with mixed precision; measure speed/memory improvement
- Implement gradient accumulation for fine-tuning BERT on a small GPU
- Implement focal loss, contrastive loss, dice loss as `nn.Module` subclasses
- Benchmark ResNet-50 training; optimize data loading until GPU utilization >85%

---

## Deliverable

`week19_pytorch_efficiency.ipynb` — speed comparison table (baseline vs. AMP vs. AMP+GA) + 3 custom losses

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
