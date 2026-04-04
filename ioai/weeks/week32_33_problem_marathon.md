# Weeks 32–33 — Historical Problem Marathon
## Phase 4: Pre-Competition Peak (Weeks 32–33 of 40)

---

## Objective

Systematically solve every available IOAI and national selection problem. This builds the pattern recognition that distinguishes gold medalists: "I've seen something like this before."

---

## Problem Inventory

| Source | Problems | URL |
|--------|----------|-----|
| IOAI 2024 | 3+ tasks with solutions | https://github.com/IOAI-official/IOAI-2024 |
| IOAI 2025 | 6 tasks with solutions | https://github.com/IOAI-official/IOAI-2025 |
| NEOAI 2025 | 8 tasks | https://www.kaggle.com/competitions/neoai-2025 |
| APOAI | 4+ tasks | https://github.com/open-cu/awesome-ioai-tasks |
| Polish OAI | Multiple | https://github.com/open-cu/awesome-ioai-tasks |
| Romanian ONIA | Multiple | https://github.com/stefanasandei/roai-solved |
| Japanese JOAI | Multimodal gas classification | https://github.com/open-cu/awesome-ioai-tasks |
| USAAIO Past Problems | Multiple rounds | https://www.usaaio.org/past-problems |

---

## Protocol for Each Problem

1. **Solve independently** (timed — 90 min max): attempt without looking at any solutions
2. **Record score**: what metric value did you achieve?
3. **Read gold/reference solution**: understand every design decision
4. **Document delta**: what technique did you miss? What would have taken you from silver to gold score?

---

## Technique Log Template

After each problem, add one row:

| Problem | Your Approach | Gold Approach | Key Gap |
|---------|--------------|---------------|---------|
| IOAI 2025 Radar | Isolation Forest alone | IF + signal frequency features + ensemble | Missed signal processing features |
| NEOAI Task X | BERT fine-tune | DeBERTa + data augmentation | Model choice + augmentation |

---

## Priority Order

1. IOAI 2025 (most recent, highest signal)
2. IOAI 2024
3. NEOAI 2025 (same difficulty level)
4. APOAI tasks
5. National selections (Romania, Poland)

---

## Deliverable

`week32-33_problem_marathon/` — one notebook per problem + `technique_log.md` with accumulated patterns

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
