# Weeks 25–28: Research Paper
## Phase 5 — Analysis and Writing (Weeks 25–28 of 36)

---

## Objectives
- Write a complete, publication-quality research paper
- Paper is NOT required by ISEF but is strongly recommended — judges treat students with a paper differently
- Having an arXiv preprint before the fair significantly strengthens credibility
- Paper also serves as the primary reference document for writing your abstract and poster

---

## Why Write a Paper for ISEF

1. **Judge credibility**: When a judge sees an arXiv link or a bound paper on your table, the project is immediately perceived as more rigorous
2. **Depth of understanding**: Writing forces you to articulate exactly what you did and why — you will discover gaps in your reasoning
3. **Abstract source**: Your 250-word ISEF abstract is drawn directly from the paper abstract
4. **Admissions**: A posted arXiv preprint is one of the strongest signals for top university admissions officers

---

## Paper Structure (Standard CS/ML Format)

### Title
Descriptive and specific. Not "AI for Medical Diagnosis." Instead:
> "Multi-Scale Frequency-Domain Augmentation Improves Early-Stage Pancreatic Tumor Detection in CT Images: A CNN Approach with 87.4% Sensitivity"

### Abstract (250 words — same length as ISEF abstract)
Template:
```
Background: [1–2 sentences on why the problem matters]
Problem: [1 sentence: what the gap is]
Method: [2–3 sentences: what you did]
Results: [2–3 sentences: what you found, with numbers]
Conclusion: [1–2 sentences: significance and future implications]
```

### 1. Introduction (500–700 words)
- Hook: real-world impact of the problem
- Background: what exists and why it's insufficient
- Your contribution: one paragraph, clearly stated
- Paper outline: "Section 2 describes... Section 3 presents..."

### 2. Related Work (400–600 words)
Group prior work into 3–5 themes. For each:
- What approach does this group use?
- What limitation does your work address?
- End with: "Unlike all prior work, our method [key differentiator]."

### 3. Method (600–900 words)
The most technical section. Must be reproducible.
- 3.1: Problem formulation (define notation)
- 3.2: Architecture / algorithm description (include a figure)
- 3.3: Training procedure (optimizer, loss, hyperparameters)
- 3.4: Implementation details (software, hardware, dataset preprocessing)

### 4. Experiments (500–700 words)
- 4.1: Dataset description
- 4.2: Evaluation metrics and why
- 4.3: Baselines (what you compare against)
- 4.4: Main results table
- 4.5: Ablation study table

### 5. Results and Discussion (400–600 words)
- What do the numbers mean?
- Why does your approach work (link to ablation)?
- Failure cases and limitations
- Future work (3 specific ideas)

### 6. Conclusion (150–200 words)
- Restate what you did and what you found
- Broader impact statement

### References
- Use Zotero to auto-generate
- Format: IEEE or ACM style (standard for CS)
- 20–40 references minimum for a competitive ISEF paper

---

## Writing Timeline (4 weeks)

### Week 25: Write Methods and Experiments
- These are the easiest to write because they're factual (you just describe what you did)
- Start here, not with the introduction
- Write Methods first: describe your model and training procedure in enough detail for reproduction
- Write Experiments: describe your dataset, evaluation setup, and baselines

### Week 26: Write Results and Discussion
- Report your numbers in the Results section (pure fact, no interpretation)
- Interpret in the Discussion: why it works, where it fails, what you'd do next

### Week 27: Write Introduction and Related Work
- Introduction is hardest — write it last
- Related Work: go back to your Zotero library from Weeks 2–3; write one paragraph per theme
- Introduction: hook → background → gap → your contribution → outline

### Week 28: Write Abstract; Revise Full Paper; Submit to arXiv
- Write the 250-word abstract last (summarizes the complete paper)
- Full revision pass: check every claim has evidence, every number is correct
- Have your mentor review the paper
- Submit to arXiv (cs.LG, cs.CV, or relevant domain section): https://arxiv.org/

---

## LaTeX Template (Professional Output)

Use Overleaf (free): https://www.overleaf.com

```latex
\documentclass[10pt]{article}
\usepackage[margin=1in]{geometry}
\usepackage{graphicx, booktabs, amsmath, hyperref, cite}
\usepackage{microtype}  % better typography

\title{Your Paper Title Here}
\author{Your Name \\ School Name \\ \texttt{email@school.edu}}
\date{2025}

\begin{document}
\maketitle

\begin{abstract}
[250-word abstract]
\end{abstract}

\section{Introduction}
...

\section{Related Work}
...

\section{Method}
\subsection{Problem Formulation}
Let $\mathcal{D} = \{(x_i, y_i)\}_{i=1}^N$ denote the training dataset where...

\subsection{Architecture}
% Include figure
\begin{figure}[h]
\centering
\includegraphics[width=0.8\textwidth]{architecture.pdf}
\caption{Overview of our proposed architecture.}
\label{fig:arch}
\end{figure}

\section{Experiments}
% Main results table
\begin{table}[h]
\centering
\caption{Comparison with prior work on [Dataset].}
\begin{tabular}{lcccc}
\toprule
Method & Accuracy & F1 & AUC & Params \\
\midrule
Baseline~\cite{author2021} & 84.1 & 0.82 & 0.91 & 23.5M \\
Prior SOTA~\cite{author2023} & 85.1 & 0.84 & 0.93 & 67.1M \\
\textbf{Ours} & \textbf{87.4} & \textbf{0.87} & \textbf{0.95} & 28.3M \\
\bottomrule
\end{tabular}
\end{table}

\section{Results and Discussion}
...

\section{Conclusion}
...

\bibliographystyle{IEEEtran}
\bibliography{references}  % references.bib from Zotero export

\end{document}
```

---

## ISEF Abstract (250 words) — Written from Paper Abstract

The ISEF abstract must:
- Be ≤250 words, single page
- Describe ONLY the current year's work
- Describe only YOUR research (not your mentor's broader work)
- Be written by you (no AI assistance)
- Submitted through your regional fair's system before ISEF registration

Write it after the paper is complete. It is a compressed version of the paper abstract.

---

## Weeks 25–28 Deliverables
- [ ] Methods section written
- [ ] Experiments section written
- [ ] Results and Discussion written
- [ ] Introduction and Related Work written
- [ ] Abstract written (250 words)
- [ ] All figures at publication quality, embedded in paper
- [ ] Mentor reviews paper draft
- [ ] References formatted in Zotero, exported to .bib
- [ ] Paper submitted to arXiv (optional but highly recommended)
- [ ] ISEF abstract finalized (250 words)

## Paper Status
- Draft complete: _______________
- Mentor reviewed: _______________
- arXiv submitted: _______________
- arXiv URL: _______________
