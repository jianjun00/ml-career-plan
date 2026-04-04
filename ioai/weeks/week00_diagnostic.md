# Week 0 — Diagnostic Baseline
## Phase 0: Before Starting

Before beginning the 52-week program, assess your current level to identify which phases to accelerate or slow down.

---

## Diagnostic Tasks

- [ ] Run through IOAI 2024 Day 1 Task 1 cold (no prep). Score it.
- [ ] Complete a Kaggle Titanic-style problem end to end in under 90 minutes.
- [ ] Implement a 3-layer MLP in pure PyTorch from scratch (no tutorials).
- [ ] Derive backpropagation for a 2-layer network on paper.

---

## How to Interpret Results

### Scoring Your IOAI 2024 Task 1 Attempt

Run your solution against the ground truth from [IOAI 2024 GitHub](https://github.com/IOAI-official/IOAI-2024). Check the task README for the metric used (AUC, F1, or custom).

| Your Score vs. Known Thresholds | Starting Point |
|--------------------------------|----------------|
| Above gold threshold | Start at Week 19 (skip Phases 1–2, review Weeks 9–18 in 2 weeks) |
| Between silver and gold | Start at Week 9; accelerate Weeks 9–18 to 5 weeks |
| Between bronze and silver | Start at Week 1; complete Phase 1 at full pace |
| Below bronze | Start at Week 1; extend Phase 1 to 10 weeks |

### Scoring the Other Three Diagnostics

| Diagnostic | Passing Criterion | If You Pass | If You Fail |
|-----------|------------------|-------------|-------------|
| Titanic end-to-end in <90 min, Kaggle top 30% | Feature engineering + sklearn solid | Skim Week 2 (1 day) | Full Week 2 |
| 3-layer MLP in PyTorch correct, <30 min | PyTorch fundamentals solid | Skim Weeks 9–10 | Full Weeks 9–10 |
| Backprop derivation on paper for 2-layer network | Math foundations solid | Skim Weeks 3–4 | Full Weeks 3–4 |

### Prerequisite Dependencies (must complete in order)

```
Week 1 (NumPy) → Week 5 (Regression) → Week 9 (MLP/PyTorch) → Week 13 (Transformers)
Week 3 (Probability) → Week 7 (Evaluation) → Week 17 (Unsupervised)
Week 4 (Linear Algebra) → Week 4 (PCA) → Week 17 (Clustering)
Week 9 (PyTorch) → Week 10 (Optimization) → Week 11 (CNNs) → Week 12 (Segmentation)
Week 11 (CNNs) → Week 13 (Attention) → Week 14 (BERT/ViT)
```

Do not skip prerequisite weeks even if you feel confident — the later weeks reference exact code patterns from earlier ones.

---

## Environment Setup (Do This First)

Pin versions to avoid breaking API changes during your training year:

```bash
pip install torch==2.4.0 torchvision==0.19.0 numpy==1.26.4 pandas==2.2.2 \
    scikit-learn==1.5.1 matplotlib seaborn xgboost==2.1.0 lightgbm==4.5.0 \
    optuna==3.6.1 transformers==4.44.0 datasets peft accelerate \
    umap-learn librosa scipy pillow
```

Verify GPU and key packages:
```python
import torch
print(torch.__version__)          # 2.4.0
print(torch.cuda.is_available())  # True if GPU present
print(torch.cuda.get_device_name(0) if torch.cuda.is_available() else "CPU only")

import sklearn, xgboost, transformers
print(sklearn.__version__)        # 1.5.1
print(xgboost.__version__)        # 2.1.0
print(transformers.__version__)   # 4.44.0
```

**CPU-only**: All weeks can be completed on CPU. Training will be slower — reduce batch sizes and epochs during practice. At IOAI, you will have GPU access. Use Google Colab (free T4 GPU) for weeks requiring GPU (Weeks 11–22).

---

## IOAI Competition Environment Specs

Know your competition environment before you compete in it:

**Platform**: Bohrium (China-based scientific computing platform) or Kaggle Notebooks (varies by year).

**Software environment** (IOAI 2025):
```
Python 3.12
PyTorch 2.1.x (TensorFlow BLOCKED)
Scikit-learn 1.3.x
NumPy 1.26.x
Pandas 2.1.x
Transformers 4.35.x (HuggingFace)
Pillow, OpenCV, librosa, scipy
```

**Hardware** (IOAI 2025): Single GPU per participant (A100 40GB or similar).

**Internet**: BLOCKED except whitelisted documentation:
- https://pytorch.org/docs/
- https://scikit-learn.org/stable/
- https://numpy.org/doc/
- https://pandas.pydata.org/docs/
- https://huggingface.co/docs/transformers/
- https://docs.python.org/3/

**AI assistant**: Token-limited access to GPT-4o-mini (~10 queries per 6-hour session). Use these for: algorithm questions you genuinely can't answer from memory, not for code generation.

**Evaluation**: Submit predictions file or call an evaluation API. Results returned in ~5–10 minutes. Budget for this lag in your time plan.

**What this means for preparation**:
- Practice writing PyTorch models without referencing documentation
- Know your key sklearn/HuggingFace APIs from memory
- 10 GPT queries = 1 query per ~36 minutes. Plan carefully.

---

*Part of the IOAI Gold Medal 52-Week Training Program*
