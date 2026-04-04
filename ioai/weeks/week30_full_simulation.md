# Week 30 — Full IOAI 2025 Simulation (Day 1 + Day 2)
## Phase 3: Advanced Topics + Competition Hardening (Week 30 of 30) — Capstone

---

## Overview

This is the Phase 3 capstone. Simulate both days of IOAI 2025 under exact competition conditions.

**Total time**: 14 hours over 2 days

---

## Day 1 Simulation (6 hours)

### IOAI 2025 Day 1 Tasks

| Task | Domain | Key Technique |
|------|--------|---------------|
| Radar | Signal/anomaly detection | Fourier features + Isolation Forest / Autoencoder |
| Chicken Counting | Computer Vision | UNet + density map estimation |
| Concepts | NLP with LLM API | Prompting + BERT fine-tune |

**Rules**:
- No personal notes
- Whitelisted docs only: PyTorch, Scikit-learn, NumPy, HuggingFace, Python stdlib
- Maximum 10 GPT-4o-mini queries per day
- Time each task; record first submission time

---

## Day 2 Simulation (6 hours)

### IOAI 2025 Day 2 Tasks (harder)

| Task | Domain | Key Technique |
|------|--------|---------------|
| Restroom Icon Matching | Image matching | Siamese network + cosine similarity |
| Antique Painting Authentication | Clustering | PCA → t-SNE/UMAP → K-means |
| Pixel Parsimony | Constrained optimization | scipy.optimize + custom loss |

---

## Scoring

After both days:
1. Score each task using the provided ground truth from [IOAI 2025 GitHub](https://github.com/IOAI-official/IOAI-2025)
2. Compare against published gold medalist scores from [ioai-writeup.github.io](https://ioai-writeup.github.io/)
3. Identify the exact point gap to gold threshold

---

## Post-Simulation Gap Analysis (2h)

For each task where you didn't reach gold level:
- Was it algorithm choice? → Review the gold solution, understand their approach
- Was it time management? → You ran out of time before implementing the right idea
- Was it implementation bugs? → Identify and fix
- Was it not knowing the technique? → Add to Phase 4 drill list

---

## Deliverable

`week30_full_simulation/` — 6 solution notebooks + `gap_analysis.md`:
- Score per task vs. gold threshold
- Root cause for each gap
- Priority list for Phase 4 targeted fixes

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
