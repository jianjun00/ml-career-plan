# Week 4: Problem Framing
## Phase 1 — Foundation (Week 4 of 36)

---

## Objectives
- Narrow from 3–5 gap candidates to exactly 1 research question
- Frame the question precisely: hypothesis, independent variable, dependent variable, metric
- Validate feasibility: data exists, compute is available, you can finish in time
- Write a one-paragraph project pitch

---

## The Research Question Formula

A strong ISEF research question has this structure:

> **"Does [your proposed approach] improve [specific metric] on [specific task] compared to [specific baseline], for [specific population/domain]?"**

Example (weak): "Can AI detect cancer better?"
Example (strong): "Does a multi-scale CNN incorporating both spatial and frequency-domain features improve sensitivity for early-stage pancreatic cancer detection on the PanCancer Atlas CT dataset compared to ResNet-50, particularly for tumors < 2cm in diameter?"

The strong version tells judges:
- Exactly what you're proposing (multi-scale CNN + frequency features)
- Exactly what you're measuring (sensitivity)
- Exactly what you're comparing against (ResNet-50 baseline)
- Exactly what dataset (PanCancer Atlas CT)
- Exactly why it matters (small tumors are hard to detect)

---

## Feasibility Checklist

Before committing to a research question, verify all of these:

### Data
- [ ] A public dataset exists for your task (or you can collect one without IRB issues)
- [ ] Dataset size is sufficient for ML (>1,000 labeled examples for deep learning minimum; >10,000 is better)
- [ ] Dataset is downloadable today — no access restrictions you can't meet
- [ ] You understand the dataset's limitations (collection bias, class imbalance, labeling errors)

**Public dataset resources**:
- Kaggle Datasets: https://www.kaggle.com/datasets
- UCI ML Repository: https://archive.ics.uci.edu/
- Hugging Face Datasets: https://huggingface.co/datasets
- PhysioNet (medical): https://physionet.org/
- NCBI/GEO (genomics): https://www.ncbi.nlm.nih.gov/geo/
- Zenodo (scientific): https://zenodo.org/
- Papers With Code Datasets: https://paperswithcode.com/datasets

### Compute
- [ ] Google Colab (free T4 GPU) is sufficient for your proposed model size
- [ ] Or Kaggle Notebooks (free P100 GPU, 30h/week)
- [ ] Or you have local GPU access
- [ ] Training time per experiment < 4 hours (otherwise iteration is too slow)

**Compute reality check**: A ResNet-50 on ImageNet needs ~4 days on a V100. An MNIST MLP takes 2 minutes. For ISEF projects, aim for models that train in <1 hour on Colab so you can run many experiments.

### Time
- [ ] Core experiments completable in 12–14 weeks of part-time work (~10h/week)
- [ ] Buffer exists for failed experiments (50% of your time budget should be buffer)
- [ ] You can define "done": a clear success criterion exists

### Uniqueness
- [ ] A search of Google Scholar for the exact combination of your approach + dataset + metric shows no paper that already does exactly what you're proposing
- [ ] Papers With Code doesn't show your method already implemented with your dataset

---

## How to Write the Project Pitch (One Paragraph)

Template:
> [Domain] is important because [why it matters to society]. The current state of the art [best known approach] achieves [current best result] but fails in [specific limitation]. I propose [your approach], which addresses this limitation by [key innovation]. I will evaluate my approach on [dataset] using [metric] and compare against [baseline]. If successful, this would [real-world impact].

Write this pitch. If you can't write it clearly in one paragraph, the project isn't well-defined enough yet. Iterate until it's clear.

---

## Evaluating Three Research Questions Side by Side

If you still have multiple candidates, compare them:

| Question | Dataset available? | Baseline available? | Novel? | Impact? | Time to implement | Your excitement (1-5) |
|----------|------------------|-------------------|--------|---------|------------------|----------------------|
| Option A | | | | | | |
| Option B | | | | | | |
| Option C | | | | | | |

**Choose the one with the best combination of**: dataset availability + novelty + your genuine excitement. Excitement matters because you'll spend 6+ months on this.

---

## Common Pitfalls When Framing ISEF Projects

### Pitfall 1: Too Broad
"Using AI to improve healthcare" — this isn't a research question. Narrow it to one specific condition, one specific imaging modality, one specific task.

### Pitfall 2: Already Solved
Before committing, search arXiv for your exact combination of method + dataset. The field moves fast. If a paper from 3 months ago does exactly what you planned, you need to either (a) address a limitation of that paper or (b) switch questions.

### Pitfall 3: No Baseline
"My model gets 89% accuracy" means nothing without a baseline. You must have a prior method to compare against. If no baseline exists, it's a sign the problem is either too new (good) or doesn't make sense (bad).

### Pitfall 4: Requires Resources You Don't Have
Wet lab, proprietary data, enterprise cloud credits, or access to a specific rare dataset that isn't publicly available. Verify every dependency is accessible NOW.

### Pitfall 5: Human Subjects Without IRB Plan
If your project needs to collect data from humans (surveys, recordings, biometric data), you MUST get IRB/SRC approval before collecting ANY data. Factor this into your Week 7-8 timeline.

---

## Week 4 Deliverables
- [ ] Narrow to exactly 1 research question
- [ ] Write the one-paragraph project pitch
- [ ] Verify the feasibility checklist (all items checked)
- [ ] Download or confirm access to your dataset
- [ ] Reproduce one baseline result from a paper in your domain (sanity check that you can work with the data/code)

## Final Research Question
> [Write it here in the precise format above]

## Project Pitch
> [One paragraph]

## Success Criterion
> My project succeeds when: [specific, measurable outcome]
