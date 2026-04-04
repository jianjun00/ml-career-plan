# Week 21 — Computer Vision: Counting, Matching, and Metric Learning
## Phase 3: Advanced Topics + Competition Hardening (Week 21 of 30)

**IOAI relevance**: "Chicken Counting" (IOAI 2025) and "Restroom Icon Matching" (IOAI 2025).

---

## Theory (4h)

- **Density estimation for counting**: predict density maps, sum for count
- **Image matching**: feature descriptors (ORB, SIFT conceptually), learned descriptors
- **Metric learning**: contrastive loss, triplet loss, cosine similarity in embedding space
- **SimCLR, DINO**: self-supervised visual representations

---

## Density Map Approach for Counting
```python
# Key idea: for each object, place a Gaussian kernel at its center
# Ground truth is a density map; sum = total count
# UNet predicts density map; MSE loss against ground truth

def create_density_map(points, img_h, img_w, sigma=5):
    dm = np.zeros((img_h, img_w))
    for (x, y) in points:
        if 0 <= x < img_w and 0 <= y < img_h:
            dm[int(y), int(x)] += 1
    from scipy.ndimage import gaussian_filter
    return gaussian_filter(dm, sigma=sigma)

# Model: UNet → density map prediction
# Loss: F.mse_loss(pred_map, gt_map)
# Inference count: pred_map.sum().item()
```

## Siamese Network for Matching
```python
class SiameseNet(nn.Module):
    def __init__(self, backbone):
        super().__init__()
        self.backbone = backbone  # e.g., ResNet-18 without fc

    def forward(self, x1, x2):
        f1 = self.backbone(x1)  # (B, embed_dim)
        f2 = self.backbone(x2)
        return F.normalize(f1, dim=-1), F.normalize(f2, dim=-1)

# Cosine similarity matching
def match_images(query_feats, gallery_feats):
    # query_feats: (Q, D), gallery_feats: (G, D)
    scores = query_feats @ gallery_feats.T   # (Q, G)
    return scores.argmax(dim=-1)

# Triplet loss for training
def triplet_loss(anchor, pos, neg, margin=0.3):
    d_ap = (anchor - pos).pow(2).sum(1)
    d_an = (anchor - neg).pow(2).sum(1)
    return F.relu(d_ap - d_an + margin).mean()
```

---

## Practice (8h)

- Implement density map approach for object counting (key for "Chicken Counting" IOAI 2025)
- Practice [IOAI 2025 "Restroom Icon Matching" task](https://github.com/IOAI-official/IOAI-2025)
- Build a Siamese network for image similarity using triplet loss

---

## Deliverable

`week21_counting_matching.ipynb` — density map counter + Siamese network + IOAI task solution

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
