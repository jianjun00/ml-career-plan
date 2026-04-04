# Week 47 — Mental Preparation and Competition Strategy
## Phase 5: Peak Performance Maintenance (Week 47 of 48)

---

## Wisdom from Top Competitors

**Tudor Musat (Romania, Gold 2025)**:
> Strong mathematical foundation. When he saw the spiral cluster in "Antique Painting," he immediately applied PCA → visualize → DBSCAN rather than reaching for a neural network.

**Tudor Morariu (Romania, Silver 2025)**:
> "Experience beats novelty. In a 6-hour window, rely on what you've tested."
> Key lesson: he lost points on tasks where he tried unfamiliar techniques under pressure.

**Stefan Asandei (Romania, Bronze 2025)**:
> "Do not overthink. Secure the baseline before experimenting."
> Key lesson: "In hindsight, I overprepared for tasks by learning techniques that were too complicated."

**Russian team preparation principle**:
> Strong mathematical foundation is the core differentiator. They can derive and adapt algorithms on the fly because they understand why they work.

---

## Your Personal Competition Checklist

### 30 Minutes Before Competition Start
- [ ] Review your starter kit templates mentally (not on paper — from memory)
- [ ] Check that PyTorch, Scikit-learn, NumPy are importable in your environment
- [ ] Know the whitelisted documentation URLs by heart
- [ ] Read the competition rules one more time

### Task Start Protocol
```
0:00–0:10  Read full problem statement (don't skip — misreading costs more time)
0:10–0:25  Load data: shapes, dtypes, missing values, target distribution
0:25–0:45  Implement baseline template for this task type
0:45–0:50  Submit baseline — you now have insurance points
0:50–1:20  Two targeted improvements (one at a time, record each score)
1:20–1:30  Final check: best submission queued, output format correct
```

### First Submission Rule
**Never spend more than 45 minutes without a submission.** A mediocre baseline beats a perfect model that crashes at minute 89.

### Iteration Discipline
- Change **one thing** at a time
- Record every score and what you changed
- If a change doesn't improve by >0.002 in metric: revert
- If you're unsure between two approaches: submit both and keep the better one

### 60 Minutes Remaining
- Stop all experiments
- Polish your best current approach
- Verify output format matches exactly what's expected

### Final 15 Minutes
- Read the output specification one more time
- Check that your submission file has the correct number of rows
- Verify no NaN values in predictions

---

## Dealing With Being Stuck

If you're completely stuck after 30 minutes:
1. **Check data**: is there something obvious you missed in EDA?
2. **Simplify**: what's the absolute dumbest baseline that could work?
3. **Read metric**: are you optimizing the right thing?
4. **Different approach**: if tabular isn't working, is there a pattern to exploit directly?

If still stuck after 60 minutes:
- **Submit a uniform baseline** (e.g., predict majority class, or predict mean)
- **Move on** to the next task
- Come back with fresh eyes if time allows

---

## Practice (8h)

Run two 3-hour mock sessions with a timer this week:
- Session 1: 2 tasks from national selections (not IOAI 2024/2025 — save those for later)
- Session 2: 2 tasks from APOAI/NEOAI

Apply the competition checklist rigorously for each task.

---

## Deliverable

`week47_competition_strategy.md` — your finalized personal playbook including:
- Your baseline templates (which template for which task type)
- Your personal competition checklist (customized from above)
- Your time allocation strategy
- Your "stuck" protocol

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 5: Peak Performance Maintenance*
