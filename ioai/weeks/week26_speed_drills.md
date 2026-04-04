# Week 26 — Speed Training: The 90-Minute Task Challenge
## Phase 3: Advanced Topics + Competition Hardening (Week 26 of 30)

**Focus**: Competitive time management — the most important meta-skill at IOAI.

---

## The Math

```
6 hours / 3 tasks = ~90 minutes per task including:
  0:00–0:10  Read problem statement, understand data
  0:10–0:30  EDA + baseline plan
  0:30–1:00  First working model + first submission
  1:00–1:20  Iterate 2–3 targeted improvements
  1:20–1:30  Buffer: re-evaluate, verify submission format
```

The goal is to have a score on the board within the first 30 minutes. Every additional minute without a submission is a risk.

---

## The 90-Minute Workflow

### Minutes 0–10: Problem Understanding
- [ ] Read the full problem statement (don't skip!)
- [ ] Identify: task type (classification/regression/segmentation/clustering?)
- [ ] Identify: evaluation metric (AUC? F1? RMSE? IoU?)
- [ ] Load data: `df.shape`, `df.dtypes`, `df.isnull().sum()`, `df.head()`

### Minutes 10–30: EDA + Baseline Plan
- [ ] Check class balance (for classification)
- [ ] Plot target distribution
- [ ] Check for obvious outliers or data quality issues
- [ ] Decide: which baseline to start with?

### Minutes 30–60: Baseline Implementation
- [ ] Implement your template for this task type (from memory)
- [ ] Get a working submission to the judge
- [ ] Record score. **This is your insurance score.**

### Minutes 60–80: Improvements
- [ ] One change at a time. Record each score.
- [ ] If improvement < 0.001 in metric: move on
- [ ] Typical improvements: better features, better model, post-processing

### Minutes 80–90: Polish
- [ ] Stop experimenting. Verify best submission is queued.
- [ ] Check output format exactly matches expected

---

## Practice (12h)

Solve 4 different ML tasks (tabular, CV, NLP, anomaly) in exactly 90 minutes each:
- [ ] Tabular: Kaggle House Prices or similar
- [ ] CV classification: any image dataset
- [ ] NLP classification: any text dataset
- [ ] Anomaly detection: credit card fraud or similar

**Requirement**: Make at least one submission per task.

After each task, write a 5-minute post-mortem:
- What was the bottleneck?
- Did you get a baseline in the first 30 minutes?
- What would you do differently?

---

## Build Your Personal Baseline Templates

Document code you can write from memory for each task type (no referencing notes).

---

## Deliverable

`week26_speed_drills/` — 4 solution notebooks + `postmortems.md` with lessons learned

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
