# Week 31 — Competition Playbook: Baselines for Every Task Type
## Phase 4: Pre-Competition Peak (Week 31 of 40)

**Goal**: Build and memorize your personal "starter kit" — code you can write from memory in under 15 minutes for each task type.

---

## The 7 Baseline Templates

### 1. Tabular Classification/Regression
```python
import pandas as pd, numpy as np, xgboost as xgb
from sklearn.model_selection import StratifiedKFold
from sklearn.preprocessing import LabelEncoder

df = pd.read_csv('train.csv')
# Encode categoricals
for col in df.select_dtypes('object').columns:
    df[col] = LabelEncoder().fit_transform(df[col].astype(str))
X, y = df.drop('target', axis=1).values, df['target'].values

model = xgb.XGBClassifier(n_estimators=500, max_depth=6, learning_rate=0.05,
                           subsample=0.8, colsample_bytree=0.8, random_state=42)
cv = StratifiedKFold(5, shuffle=True, random_state=42)
# OOF predictions + CV score
```

### 2. CV Classification
```python
import torchvision.models as models
model = models.resnet50(pretrained=True)
model.fc = nn.Linear(model.fc.in_features, num_classes)
# Freeze backbone initially, train head for 5 epochs, then unfreeze all
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-4, weight_decay=1e-2)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=20)
```

### 3. CV Segmentation
```python
# UNet from week12 + Dice loss
# Adam, lr=1e-4, cosine scheduler, train 30 epochs
criterion = lambda pred, tgt: 0.5*F.binary_cross_entropy_with_logits(pred,tgt) + 0.5*dice_loss(pred,tgt)
```

### 4. NLP Classification
```python
from transformers import AutoModelForSequenceClassification, AutoTokenizer, Trainer, TrainingArguments
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=n)
# tokenize, TrainingArguments(lr=2e-5, epochs=3, batch=16), Trainer.train()
```

### 5. Counting / Density Estimation
```python
# UNet → predict density map → sum for count
# MSE loss on density map, not count directly
# Ground truth: gaussian_filter(point_map, sigma=5)
```

### 6. Anomaly Detection
```python
from sklearn.ensemble import IsolationForest
iso = IsolationForest(contamination=0.05, random_state=42)
scores = -iso.score_samples(X)   # higher = more anomalous
# Threshold at percentile; report AUC
```

### 7. Clustering
```python
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans, DBSCAN
X_pca = PCA(n_components=2).fit_transform(X)
# Visualize first; then cluster
km = KMeans(n_clusters=k, random_state=42)
labels = km.fit_predict(X_pca)
```

---

## Memorization Practice (12h)

Write each baseline from **memory** without referencing previous notebooks:
- [ ] Tabular template — < 10 min
- [ ] CV classification template — < 15 min
- [ ] CV segmentation template — < 15 min
- [ ] NLP classification template — < 15 min
- [ ] Density map baseline — < 15 min
- [ ] Anomaly detection baseline — < 10 min
- [ ] Clustering baseline — < 10 min

Keep repeating any that take longer than target until they're automatic.

---

## Deliverable

`week31_starter_kit/` — 7 clean template notebooks, each running end-to-end

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
