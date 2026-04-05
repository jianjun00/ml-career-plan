# Weeks 7–8: Research Plan + IRB/SRC Approval
## Phase 2 — Problem Definition (Weeks 7–8 of 36)

---

## Objectives
- Write a complete Research Plan (required before ANY experimentation)
- Determine if IRB/SRC approval is needed and submit if so
- Have Adult Sponsor review and sign Forms 1, 1A, 1B
- Finalize your experimental design

---

## CRITICAL RULE: No Experimentation Before Forms Are Complete

This is the single most common reason projects are disqualified at fairs:

> Students start collecting data or running experiments, then try to fill out forms retroactively.

**ISEF rules are explicit**: All required forms must be completed and approved BEFORE any experimentation begins. There is no retroactive approval.

For a pure ML project using only public, pre-existing datasets with no human subjects:
- Forms 1, 1A, 1B required (always)
- IRB/SRC NOT required (you're not collecting new human data)
- You can begin after Adult Sponsor signs Form 1B

For any project involving new data from humans (surveys, recordings, interviews, biometrics):
- Forms 1, 1A, 1B required
- IRB/SRC approval required FIRST
- IRB review can take 2–8 weeks — plan accordingly

---

## The Research Plan: What It Must Include

The Research Plan is a written document submitted to your Adult Sponsor and regional fair. It is NOT displayed on your board. Required contents:

### 1. Rationale and Background
- Why is this problem important?
- What has been done before? (cite your literature review)
- What gap exists that your project addresses?
- Length: 300–500 words

### 2. Research Question / Hypothesis / Engineering Goal
For engineering/CS projects, frame as an **engineering goal**:
> "The goal of this project is to develop [approach] that achieves [specific metric] on [task/dataset] by [method], compared to [baseline]."

For scientific AI projects, frame as a **hypothesis**:
> "I hypothesize that [approach A] will outperform [approach B] on [metric] because [mechanistic reason]."

### 3. Methodology
This is the most critical section — judges evaluate whether your method is rigorous.

For ML/AI projects, include:
- **Dataset description**: source, size, class distribution, train/val/test split
- **Preprocessing pipeline**: normalization, augmentation, handling missing data
- **Model architecture**: describe layers, hyperparameters, activation functions
- **Training procedure**: optimizer, learning rate schedule, batch size, number of epochs
- **Evaluation metrics**: why you chose these metrics (accuracy, F1, AUC-ROC, etc.)
- **Baseline comparison**: which published methods you'll compare against and why
- **Statistical validation**: how you'll determine if differences are significant (e.g., 5-fold cross-validation, t-test)
- **Ablation studies**: which components you'll test removing/modifying

### 4. Risk Assessment
For CS/ML projects with public data, this is minimal:
- Data privacy: confirm dataset is fully anonymized and publicly licensed
- Compute safety: no risk
- If project involves human feedback: document consent procedures

### 5. Resources Required
- Hardware/software needed
- Dataset access (confirm it's available)
- Mentor access

### 6. Timeline
Week-by-week plan for your remaining research weeks.

### 7. Bibliography
Cite all papers referenced in the plan (15+ for a strong project).

---

## Research Plan Template (CS/ML Project)

```markdown
# Research Plan
## [Project Title]
**Student**: [Name] | **Category**: [SOFT/ROBO/CBIO/etc.] | **Date**: [Date]
**Adult Sponsor**: [Name, Institution]

## 1. Rationale and Background
[300–500 words on the problem, prior work, and gap]

## 2. Research Question
[One sentence, precise format from Week 4]

## 3. Methodology

### 3.1 Dataset
- Source: [URL or citation]
- Size: [number of samples, classes]
- Split: [train/val/test percentages]
- Preprocessing: [steps]

### 3.2 Proposed Approach
[Describe your model/algorithm — enough detail that someone could reproduce it]

### 3.3 Baseline
[Which published method you're comparing against; why this baseline is appropriate]

### 3.4 Evaluation
- Primary metric: [metric name and why it's appropriate for this task]
- Secondary metrics: [if any]
- Validation strategy: [k-fold CV / held-out test set / etc.]
- Statistical significance: [how you'll determine if results are significant]

### 3.5 Ablation Studies
- Experiment A: [what you'll remove/change and why]
- Experiment B: [what you'll remove/change and why]

## 4. Risk Assessment
[Data privacy, compute, any human subjects considerations]

## 5. Timeline
[Week-by-week from now to fair]

## 6. Bibliography
[Formatted references — use Zotero to auto-generate]
```

---

## IRB/SRC: Do You Need It?

### You DO need IRB/SRC if your project involves:
- Surveys, questionnaires, or interviews with people
- Recordings (audio, video) of people
- Biometric data (EEG, heart rate, facial recognition of individuals)
- Medical records (even de-identified)
- Data from minors that wasn't previously public
- Vertebrate animals

### You do NOT need IRB/SRC if:
- You are using a public, pre-existing, fully anonymized dataset
- You are working entirely with non-human data (satellite imagery, genomic sequences from public repositories, physical simulations)
- Your "human subjects" work is observational of public information (text on public forums)

### If you need IRB/SRC:
1. Contact your school's science fair coordinator — they coordinate with the SRC
2. Or contact a local university IRB if you're doing research at their lab
3. Submit the application immediately — review takes 2–8 weeks
4. Do NOT collect any data while waiting for approval

---

## Experimental Design: Key Decisions Before Week 9

Before starting experiments, make these decisions explicit:

### Data Split Strategy
```python
# Standard split for ML projects
# Use fixed random seed for reproducibility
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y  # stratify preserves class balance
)
X_train, X_val, y_train, y_val = train_test_split(
    X_train, y_train, test_size=0.125, random_state=42, stratify=y_train
    # 0.125 * 0.8 = 0.1 → 70/10/20 final split
)
```

**ISEF rule**: Your test set is held out completely until final evaluation. You tune hyperparameters on the validation set only.

### Baseline Selection
Your baseline must be:
1. A published method (cite the paper)
2. Implemented correctly (ideally using the authors' code)
3. Evaluated on the same data split you use

If no published code exists, implement it yourself and verify you get the same results reported in the paper before proceeding.

---

## Weeks 7–8 Deliverables
- [ ] Write complete Research Plan (all 6 sections)
- [ ] Have Adult Sponsor review Research Plan
- [ ] Adult Sponsor signs Forms 1, 1A, 1B
- [ ] Submit IRB/SRC application if needed (do this immediately, Week 7 Day 1)
- [ ] Implement data loading and preprocessing pipeline
- [ ] Implement and run baseline model on your dataset
- [ ] Reproduce baseline's published accuracy (sanity check before building on it)
- [ ] Set up experiment tracking (Weights & Biases: https://wandb.ai — free for students)

## Forms Status
- [ ] Form 1 signed by Adult Sponsor
- [ ] Form 1A signed by student
- [ ] Form 1B signed
- [ ] IRB/SRC: Not needed / Submitted (date: ___) / Approved (date: ___)

## Baseline Result
- **Method**: _______________
- **Our result**: _______________
- **Paper's reported result**: _______________
- **Match?**: Yes / No (investigate if No)
