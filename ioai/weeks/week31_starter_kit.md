# Week 31 — Competition Playbook: Baselines for Every Task Type
## Phase 4: Pre-Competition Peak (Week 31 of 40)

**Goal**: Build and memorize your personal "starter kit" — code you can write from memory in under 15 minutes for each task type.

---

## The 7 Baseline Templates

### 1. Tabular Classification/Regression
```python
import pandas as pd, numpy as np, xgboost as xgb
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.preprocessing import OrdinalEncoder
import warnings; warnings.filterwarnings('ignore')

df_train = pd.read_csv('train.csv')
df_test  = pd.read_csv('test.csv')

# Safe categorical encoding: OrdinalEncoder handles unseen categories at test time
# LabelEncoder.transform() raises ValueError on unseen values — do NOT use for train/test splits
cat_cols = df_train.select_dtypes('object').columns.tolist()
if 'target' in cat_cols: cat_cols.remove('target')

enc = OrdinalEncoder(handle_unknown='use_encoded_value', unknown_value=-1)
df_train[cat_cols] = enc.fit_transform(df_train[cat_cols].astype(str))  # fit on TRAIN
df_test[cat_cols]  = enc.transform(df_test[cat_cols].astype(str))        # apply to TEST

X = df_train.drop('target', axis=1).fillna(-999).values
y = df_train['target'].values
X_test = df_test.fillna(-999).values

model = xgb.XGBClassifier(n_estimators=500, max_depth=6, learning_rate=0.05,
                           subsample=0.8, colsample_bytree=0.8, random_state=42, n_jobs=-1)
cv = StratifiedKFold(5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=cv, scoring='roc_auc')
print(f"CV AUC: {scores.mean():.4f} ± {scores.std():.4f}")
model.fit(X, y)
```

### 2. CV Classification
```python
import torchvision.models as models
from torchvision.models import ResNet50_Weights

# Use weights= instead of deprecated pretrained=True (torchvision 0.13+)
model = models.resnet50(weights=ResNet50_Weights.DEFAULT)
model.fc = nn.Linear(model.fc.in_features, num_classes)

# Phase 1: train only the head (backbone frozen)
for p in model.parameters(): p.requires_grad = False
for p in model.fc.parameters(): p.requires_grad = True
optimizer = torch.optim.AdamW(model.fc.parameters(), lr=1e-3, weight_decay=1e-2)

# Phase 2 (after ~5 epochs): unfreeze last block + head with lower LR
for p in model.layer4.parameters(): p.requires_grad = True
optimizer = torch.optim.AdamW([
    {'params': model.layer4.parameters(), 'lr': 1e-5},
    {'params': model.fc.parameters(),     'lr': 1e-4},
], weight_decay=1e-2)
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=15)
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
