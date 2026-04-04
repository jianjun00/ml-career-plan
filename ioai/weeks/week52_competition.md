# Week 52 — On-Site Competition
## Phase 6: Competition Month (Week 52 of 52)

---

## Competition Days

### Morning Routine (Both Days)
- Light exercise: 15–20 min walk
- Good breakfast: enough energy for 6 hours
- Arrive/login 10–15 minutes early
- Do not read code or notes before competition starts

---

## Task 1: Read → EDA → Baseline → Submit → Iterate

### The Protocol (drill this until automatic)

```
0:00–0:10  READ THE FULL PROBLEM STATEMENT
           - What is the task? (classification? regression? segmentation?)
           - What is the metric? (AUC? F1? RMSE? custom?)
           - What are the constraints? (format, number of outputs)

0:10–0:25  LOAD DATA AND EDA
           - df.shape, dtypes, isnull().sum()
           - Target distribution
           - One visualization that reveals the most about the data

0:25–0:45  IMPLEMENT BASELINE TEMPLATE
           - From memory — no referencing notes
           - Choose template based on task type

0:45–0:50  SUBMIT BASELINE
           - You now have an insurance score
           - Record the score

0:50–1:20  ITERATE (one change at a time)
           - Change 1: better features / better model / different preprocessing
           - Submit, record score, decide: keep or revert
           - Change 2: different approach if change 1 didn't help enough

1:20–1:30  FINALIZE
           - Stop experimenting
           - Verify: correct output format? correct number of rows? no NaN?
           - Queue your best submission
```

---

## Between Tasks

- 5-minute mental break: stand up, breathe, reset
- Do NOT think about the previous task (nothing you can do now)
- Drink water

---

## If You're Stuck

### After 20 minutes stuck (no algorithm idea):
1. Check: what's the simplest approach that could give any signal?
2. Try: predict the mean/majority class (uniform baseline)
3. Submit that — it's better than nothing

### After 30 minutes stuck (wrong algorithm):
1. Submit what you have
2. Try a completely different approach
3. Go back to the simpler one if the new approach isn't working after 20 min

### The cardinal rule: **Never submit zero.** Always have something on the board.

---

## Final Hour of Each Day

- Review all tasks — is there anything obvious you missed?
- If one task has a bug, spend up to 15 min fixing it
- Verify all submissions are correctly queued
- Do NOT start a new experiment if you haven't finished a previous one

---

## Gold Medal Principles — Final Reminder

1. Secure the baseline first — always
2. 90 minutes per task — enforce this
3. Surgical improvements over ambition
4. Mathematical intuition is the differentiator
5. Know your toolkit cold — write it without docs
6. Understand the metric — different strategies for different metrics
7. Never overthink
8. Experience beats novelty — rely on what you've tested

---

## After the Competition

- Do not check others' solutions until all tasks are submitted
- Write a brief post-competition analysis: what went well, what would you do differently?
- If results are not what you hoped: every top competitor has disappointing competitions. Analyze and move forward.

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 6: Competition Month*
*You've put in the work. Trust your preparation.*
