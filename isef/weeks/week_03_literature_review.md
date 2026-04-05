# Week 3: Literature Review
## Phase 1 — Foundation (Week 3 of 36)

---

## Objectives
- Deep-read 10–15 papers in your chosen domain
- Map the state of the art (what's the best result on which benchmarks?)
- Identify 3–5 specific research gaps you could potentially address
- Start your reference manager and research log

---

## What a Literature Review Actually Is

A literature review is not a summary of papers you read. It is a **structured argument** for why your future research is necessary:

1. Here is what the field has achieved (prior work)
2. Here is what the field has NOT achieved (the gap)
3. Here is why the gap matters (motivation)
4. Here is what I will do to address it (your research question — coming Week 4)

ISEF judges evaluate whether you know the field. If a judge asks "Are you aware of [paper X]?" and you haven't read it, that's a serious credibility problem.

---

## The 15-Paper Literature Review Protocol

### Step 1: Start from a Survey Paper (Day 1-2)
Find a recent survey (2022–2024) in your domain. Surveys review hundreds of papers and save you weeks of searching.
- Search: "[your domain] survey 2023" or "[your domain] review 2024" on Google Scholar
- Read this survey at Pass 2 level; extract the 20–30 most-cited papers in the subarea you care about

### Step 2: Read the Top-10 Most-Cited Papers (Days 2-4)
Sort by citation count. Read each at Pass 2 level. These establish the baseline knowledge every judge expects you to know.

### Step 3: Read the 5 Most Recent Papers (Days 4-5)
Papers from 2024 show you know the current state of the art. These are often on arXiv before formal publication.

### Step 4: Build Your Gap Map (Days 6-7)
After reading 15 papers, create a table:

| Paper | Method | Dataset | Metric | Best Score | Limitation (stated by authors) |
|-------|--------|---------|--------|-----------|-------------------------------|
| | | | | | |

Then: which limitations appear repeatedly across multiple papers? That's a promising research gap.

---

## Tools for Managing Papers

### Reference Manager: Zotero (free)
- Download: https://www.zotero.org/
- Install the browser plugin — one click saves any paper with full metadata
- Create a collection for your ISEF project
- Use Zotero to auto-generate your bibliography later

### Note-Taking System
Use a consistent template for every paper:
```markdown
## [Paper Title]
**Authors**: 
**Year**: 
**Venue** (conference/journal): 
**URL/DOI**: 

### Problem
[What problem does this paper address?]

### Method
[What is the core technical approach?]

### Key Result
[What is the best performance and on what benchmark?]

### Limitations (stated by authors)
[What do the authors say doesn't work or is out of scope?]

### My Questions
[What don't I understand? What would I want to extend?]

### Relevance to My Project
[How does this connect to what I'm trying to do?]
```

---

## Identifying Research Gaps — Specific Signals to Look For

When reading papers, flag these patterns — they indicate a gap:

1. **"Future work" sections**: Authors explicitly say what they didn't do
2. **Narrow dataset**: "We evaluated only on [one dataset]" — test on more/different data
3. **Computational cost**: "Our method requires X GPU-hours" — can it be made efficient?
4. **Missing modality**: "We use only text, not images" — can you add the other modality?
5. **Specific population excluded**: "We did not study pediatric patients / low-resource languages / rare diseases"
6. **Reproducibility failure**: Many ML papers are not reproducible — redoing with rigorous evaluation is itself a contribution
7. **Benchmark saturation**: If accuracy is 99.9% on [dataset], the benchmark is solved — but maybe there's a harder real-world version

---

## Evaluating Whether a Gap Is ISEF-Worthy

Not all gaps are worth pursuing. Apply this filter:

| Criterion | Question |
|-----------|----------|
| **Feasibility** | Can a high school student address this with available tools and data? |
| **Novelty** | Has this specific gap already been addressed in a paper from the last 6 months? |
| **Impact** | If you solve this, who benefits and how much? |
| **Demonstrability** | Can you show the improvement clearly (quantitative metric)? |
| **Scaleability** | Can you do meaningful work in 6 months of part-time effort? |

A gap that passes all 5 is a strong ISEF project candidate.

---

## Building Your Research Log

Start a research log **now** — before you have a project. ISEF judges value seeing the authentic development of your thinking.

A research log is a dated journal (physical or digital) where you record:
- Papers you read and what you learned
- Questions that came up
- Ideas you had
- Dead ends you explored

**Physical composition notebook**: best for judges to see; shows authenticity and timeline
**Digital alternative**: Notion, Obsidian, or a dated Google Doc

The log should be messy and honest — not cleaned up or retroactively organized.

---

## This Week's Reading List (fill in your domain)

Find papers in your domain using these starting points:

**For Medical AI**: "Towards Foundation Models for Medical Image Segmentation" (arXiv 2023); "A Survey on Deep Learning in Medical Image Analysis" (Litjens et al.); "CheXNet: Radiologist-Level Pneumonia Detection" (Rajpurkar et al.)

**For Accessibility**: "Deep Learning for Sign Language Recognition: A Review" (2022); any recent ASSETS conference proceedings (assets-conference.org)

**For Environmental ML**: "Machine Learning for Climate" (Rolnick et al., 2022); "SatlasPretrain: A Large-Scale Dataset for Remote Sensing" (2023)

**For Novel Architectures**: NeurIPS/ICLR 2024 proceedings; "Efficient Transformers: A Survey" (2022)

**For Computational Biology**: "Evolutionary-scale prediction of atomic-level protein structure with a language model" (ESM2, 2022); "scGPT: toward building a foundation model for single-cell multi-omics" (2024)

---

## Week 3 Deliverables
- [ ] Read 10–15 papers in your chosen domain
- [ ] Fill in paper analysis template for each
- [ ] Build gap map table showing state of the art + limitations
- [ ] Identify 3–5 candidate research gaps
- [ ] Set up Zotero and import all papers
- [ ] Start research log (dated entry for every day you work)

## Research Gap Candidates
1. _______________
2. _______________
3. _______________
4. _______________
5. _______________
