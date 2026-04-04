# Week 18 — Phase 2 Integration + National Selection Mock
## Phase 2: Deep Learning Core (Week 18 of 18) — Integration Week

---

## Objectives

Consolidate Phase 2. Simulate full competition day. Write your personal playbook.

---

## Practice (12h)

- Timed mock: 2 IOAI 2024 on-site tasks, 3 hours each (Day 1 + Day 2 simulation)
- Complete one NEOAI 2025 Kaggle competition task
- Self-assessment: which task types are weakest? (CV segmentation? NLP? tabular?)
- Write personal "competition playbook": your go-to baselines for each task type

---

## Your Competition Playbook Template

For each task type, document:

### Tabular
```
Baseline (15 min): XGBoost, 5-fold CV, basic imputation + label encoding
Improvement 1 (20 min): LightGBM, feature engineering
Improvement 2 (15 min): Optuna tuning
Improvement 3 (10 min): Stacking or post-processing
```

### CV Classification
```
Baseline (15 min): pretrained ResNet-18, fine-tune last layer
Improvement 1 (20 min): augmentation, unfreeze more layers
Improvement 2 (15 min): try EfficientNet or ViT
```

### NLP Classification
```
Baseline (20 min): BERT fine-tune with HuggingFace
Improvement 1 (25 min): data augmentation, back-translation
Improvement 2 (15 min): try DeBERTa or RoBERTa
```

### Anomaly Detection
```
Baseline (15 min): Isolation Forest + AUC evaluation
Improvement 1 (20 min): Autoencoder-based detection
Improvement 2 (15 min): Ensemble IF + AE
```

---

## Resources

- [NEOAI 2025 Kaggle](https://www.kaggle.com/competitions/neoai-2025/overview)
- [IOAI 2025 Solution Writeups](https://ioai-writeup.github.io/)
- [Awesome IOAI Tasks](https://github.com/open-cu/awesome-ioai-tasks)

---

## Deliverable

`week18_competition_playbook.md` — filled-in playbook for all 6 task types you expect at IOAI

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
