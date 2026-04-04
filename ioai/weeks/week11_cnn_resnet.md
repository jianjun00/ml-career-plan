# Week 11 — Convolutional Neural Networks
## Phase 2: Deep Learning Core (Week 11 of 18)

---

## Theory (6h)

- **Convolution**: stride, padding, dilation — compute output dimensions manually
- **Pooling**: max, average, global average pooling
- **CNN architectures**: LeNet → AlexNet → VGG → ResNet (residual connections, why they work)
- **Transfer learning**: feature extraction vs. fine-tuning; when to unfreeze layers

**Resources**: CS231n Lecture 5–9

---

## Output dimension formula
```
out = floor((in + 2*padding - dilation*(kernel-1) - 1) / stride + 1)
# Standard conv (stride=1, pad=1, kernel=3): out = in (same padding)
# Stride=2 conv: out = in/2 (downsampling)
```

## ResNet block
```python
class ResBlock(nn.Module):
    def __init__(self, channels):
        super().__init__()
        self.conv1 = nn.Conv2d(channels, channels, 3, padding=1, bias=False)
        self.bn1   = nn.BatchNorm2d(channels)
        self.conv2 = nn.Conv2d(channels, channels, 3, padding=1, bias=False)
        self.bn2   = nn.BatchNorm2d(channels)

    def forward(self, x):
        residual = x
        out = torch.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        return torch.relu(out + residual)  # skip connection
```

## Transfer learning template
```python
import torchvision.models as models

model = models.resnet18(pretrained=True)

# Feature extraction: freeze all but last layer
for param in model.parameters():
    param.requires_grad = False
model.fc = nn.Linear(model.fc.in_features, num_classes)  # replace head

# Fine-tuning: unfreeze last 2 layers
for param in model.layer4.parameters():
    param.requires_grad = True
for param in model.fc.parameters():
    param.requires_grad = True

optimizer = torch.optim.Adam(filter(lambda p: p.requires_grad, model.parameters()), lr=1e-4)
```

---

## Practice (6h)

- Implement a minimal ResNet-like network in PyTorch; train on CIFAR-10 (target: >90% accuracy)
- Fine-tune pretrained ResNet-18 on a small custom dataset
- Data augmentation: implement RandomCrop, RandomFlip, ColorJitter; measure impact on test accuracy

---

## Deliverable

`week11_cnn_resnet.ipynb` — ResBlock implementation + CIFAR-10 >90% + augmentation ablation

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
