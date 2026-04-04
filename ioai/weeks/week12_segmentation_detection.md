# Week 12 — Computer Vision: Advanced Topics (Segmentation & Detection)
## Phase 2: Deep Learning Core (Week 12 of 18)

---

## Theory (4h)

- **Semantic segmentation**: FCN, UNet architecture (encoder-decoder + skip connections)
- **Object detection overview**: YOLO architecture (grid cells, anchor boxes, IoU, NMS)
- **Evaluation metrics**: mAP, IoU threshold, pixel accuracy
- **Image augmentation for dense prediction**: flipping must transform labels too

---

## UNet Architecture
```python
class DoubleConv(nn.Module):
    def __init__(self, in_c, out_c):
        super().__init__()
        self.net = nn.Sequential(
            nn.Conv2d(in_c, out_c, 3, padding=1), nn.BatchNorm2d(out_c), nn.ReLU(),
            nn.Conv2d(out_c, out_c, 3, padding=1), nn.BatchNorm2d(out_c), nn.ReLU()
        )
    def forward(self, x): return self.net(x)

class UNet(nn.Module):
    def __init__(self, in_c=3, out_c=1, features=[64,128,256,512]):
        super().__init__()
        self.downs = nn.ModuleList([DoubleConv(in_c if i==0 else features[i-1], features[i]) for i in range(len(features))])
        self.pool = nn.MaxPool2d(2)
        self.bottleneck = DoubleConv(features[-1], features[-1]*2)
        self.ups = nn.ModuleList([nn.ConvTranspose2d(features[i]*2, features[i], 2, 2) for i in range(len(features)-1,-1,-1)])
        self.up_convs = nn.ModuleList([DoubleConv(features[i]*2, features[i]) for i in range(len(features)-1,-1,-1)])
        self.final = nn.Conv2d(features[0], out_c, 1)

    def forward(self, x):
        skip = []
        for down in self.downs:
            x = down(x); skip.append(x); x = self.pool(x)
        x = self.bottleneck(x)
        for up, conv, s in zip(self.ups, self.up_convs, skip[::-1]):
            x = up(x)
            if x.shape != s.shape: x = F.interpolate(x, size=s.shape[2:])
            x = conv(torch.cat([s, x], dim=1))
        return self.final(x)
```

### Dice Loss (better than BCE for segmentation)
```python
def dice_loss(pred, target, smooth=1):
    pred = torch.sigmoid(pred)
    intersection = (pred * target).sum()
    return 1 - (2 * intersection + smooth) / (pred.sum() + target.sum() + smooth)
```

---

## Practice (8h)

- Implement UNet from scratch in PyTorch; train on Oxford Pets segmentation dataset
- Reproduce Stefan Asandei's IOAI 2025 chicken counting approach: UNet with MSE on density maps
- Practice IOAI 2024 "Lab Repository" CV task from [IOAI 2024 GitHub](https://github.com/IOAI-official/IOAI-2024)

---

## Deliverable

`week12_segmentation_detection.ipynb` — UNet implementation + segmentation training + density map counting

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
