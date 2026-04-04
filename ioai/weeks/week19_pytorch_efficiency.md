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

## Mixed Precision Training (PyTorch 2.0+ native API)

> **Deprecation note**: `from torch.cuda.amp import GradScaler, autocast` is the old API (still works but emits deprecation warnings in PyTorch 2.0+). Use `torch.amp.autocast` and `torch.amp.GradScaler` instead.

```python
import torch

# PyTorch 2.0+ native AMP (use this)
scaler = torch.amp.GradScaler('cuda')

for X, y in loader:
    X, y = X.cuda(), y.cuda()
    optimizer.zero_grad()
    with torch.amp.autocast('cuda'):           # forward in float16 on GPU, bfloat16 on newer GPUs
        loss = criterion(model(X), y)
    scaler.scale(loss).backward()
    scaler.step(optimizer)
    scaler.update()

# bfloat16 preference on Ampere/Hopper (A100, H100): more numerically stable than float16
# device_type='cpu' also works with bfloat16 on modern CPUs
with torch.amp.autocast('cuda', dtype=torch.bfloat16):
    output = model(X)
# Typically 1.5–2x speedup, ~50% memory reduction vs. float32
```

## Gradient Accumulation
```python
accumulation_steps = 4   # simulate batch_size * 4 effective batch

for i, (X, y) in enumerate(loader):
    X, y = X.cuda(), y.cuda()
    with torch.amp.autocast('cuda'):
        loss = criterion(model(X), y) / accumulation_steps
    scaler.scale(loss).backward()

    if (i + 1) % accumulation_steps == 0:
        scaler.step(optimizer)
        scaler.update()
        optimizer.zero_grad()
```

## GPU Memory Management

Critical patterns for IOAI's memory-constrained environment:

```python
# Check available memory before loading a large model
print(torch.cuda.memory_allocated() / 1e9, "GB allocated")
print(torch.cuda.memory_reserved()  / 1e9, "GB reserved")

# Free memory after inference (e.g., between tasks)
del model
torch.cuda.empty_cache()

# Gradient checkpointing: trade compute for memory (use for large models)
from torch.utils.checkpoint import checkpoint
# Instead of: out = layer(x)
# Use:        out = checkpoint(layer, x)
# Recomputes activations during backward instead of storing them — ~50% memory savings

# Half-precision inference (no training): model.half() converts weights to float16
model = model.half().cuda()
with torch.no_grad(), torch.amp.autocast('cuda'):
    output = model(input.half())

# Find maximum batch size that fits in GPU:
def find_max_batch_size(model, input_shape, start=256):
    batch = start
    while batch >= 1:
        try:
            _ = model(torch.randn(batch, *input_shape).cuda())
            torch.cuda.empty_cache()
            return batch
        except torch.cuda.OutOfMemoryError:
            torch.cuda.empty_cache()
            batch //= 2
    return 1
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
