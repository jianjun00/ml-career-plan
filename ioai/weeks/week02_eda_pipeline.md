# Week 2 — Pandas, Data Wrangling, EDA
## Phase 1: Foundations (Week 2 of 8)

---

## Theory (4h)

- DataFrame operations: groupby, merge, pivot, apply, vectorized string ops
- Missing data strategies: imputation vs. deletion — when each is appropriate
- Outlier detection: IQR method, Z-score, domain-specific bounds

---

## Practice (8h)

- Kaggle Learn: [Pandas micro-course](https://www.kaggle.com/learn/pandas) — complete all exercises
- EDA on Titanic dataset: generate 10 meaningful insights with visualizations (Matplotlib/Seaborn)
- Implement a full preprocessing pipeline (handle nulls, encode categoricals, scale features)

### Essential Pandas patterns

```python
# Missing values
df.isnull().sum()
df['col'].fillna(df['col'].median(), inplace=True)
df.dropna(subset=['critical_col'], inplace=True)

# Categorical encoding
df['cat'] = pd.Categorical(df['cat']).codes           # label encoding
df = pd.get_dummies(df, columns=['cat'], drop_first=True)  # one-hot

# Feature engineering
df['age_bucket'] = pd.cut(df['age'], bins=[0,18,35,60,100], labels=['youth','adult','middle','senior'])

# GroupBy aggregation
df.groupby('category')['value'].agg(['mean','std','count'])

# Outlier removal (IQR)
Q1, Q3 = df['col'].quantile([0.25, 0.75])
IQR = Q3 - Q1
df = df[(df['col'] >= Q1 - 1.5*IQR) & (df['col'] <= Q3 + 1.5*IQR)]
```

---

## Deliverable

`week02_eda_pipeline.ipynb` — full Titanic EDA with 10 insights + reusable preprocessing pipeline

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 1: Foundations*
