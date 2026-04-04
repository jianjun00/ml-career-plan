# Week 8 — Phase 1 Integration + USAAIO Mock Round 1
## Phase 1: Foundations (Week 8 of 8) — Integration Week

---

## Objectives

This is a consolidation week. No new material — only integration and assessment.

---

## Practice (12h)

- Complete 3 full ML pipelines end-to-end (tabular) in under 90 minutes each
- Solve 10 USAAIO Round 1 style questions (theoretical AI/ML knowledge)
- Mock test: pick one IOAI 2024 at-home task, solve it under timed conditions (6 hours)
- Review all errors; write "lessons learned" doc

---

## 90-Minute Pipeline Challenge

For each of 3 datasets, complete the full cycle in 90 minutes:

```
0:00–0:10  Load data, check shapes, dtypes, missing values
0:10–0:25  EDA: distributions, correlations, outliers (5 plots max)
0:25–0:40  Feature engineering: encode, impute, create new features
0:40–0:60  Train XGBoost with 5-fold CV; get AUC/RMSE
0:60–0:80  Try LightGBM or simple neural net; compare
0:80–0:90  Select best, document in 5 bullet points
```

---

## First Speed Drill: The 30-Minute Baseline Rule

Starting this week, every practice session must follow this rule: **a working submission within 30 minutes, or it counts as a failed session.**

The goal is to build the habit early. Competition speed is a skill that requires 40+ repetitions before it becomes automatic. Starting at Week 26 is too late.

**Minimum viable baseline for tabular (target: 20 min):**
```python
import pandas as pd, xgboost as xgb
from sklearn.model_selection import cross_val_score
from sklearn.preprocessing import OrdinalEncoder

df = pd.read_csv('train.csv')
cat = [c for c in df.columns if df[c].dtype == 'object' and c != 'target']
df[cat] = OrdinalEncoder(handle_unknown='use_encoded_value', unknown_value=-1).fit_transform(df[cat])
X, y = df.drop('target', axis=1).fillna(-999), df['target']
model = xgb.XGBClassifier(n_estimators=300, random_state=42, n_jobs=-1)
print(cross_val_score(model, X, y, cv=5, scoring='roc_auc').mean())
```

Time yourself. If this takes > 20 minutes, practice it again daily until it's automatic.

---

## Self-Assessment Checklist with Success Criteria

After the mock IOAI task, score yourself:

| Criterion | Bronze | Silver | Gold |
|-----------|--------|--------|------|
| First working submission | Within 60 min | Within 45 min | Within 30 min |
| Number of improvement iterations | 1 | 2–3 | 4+ |
| AUC/metric vs. public baseline | Within 5% | Within 2% | Above |
| Understood the metric being optimized | Yes | Yes | Yes |
| Checked for data leakage | Partially | Yes | Yes + fixed |
| Finished all tasks within time | Yes | Yes, with buffer | Yes, 30 min buffer |

**If you score bronze or below**: do not proceed to Phase 2. Spend an extra week repeating the 90-minute pipeline challenge with different datasets until you reach silver.

---

## Resources

- [USAAIO Past Problems](https://www.usaaio.org/past-problems)
- [IOAI 2024 GitHub](https://github.com/IOAI-official/IOAI-2024)

---

## Deliverable

`week08_phase1_assessment.md` — self-assessment with gap analysis:
- What topics need more practice?
- Which task type took longest?
- What will you change in Phase 2?

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
