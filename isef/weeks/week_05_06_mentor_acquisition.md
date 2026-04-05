# Weeks 5–6: Mentor Acquisition
## Phase 2 — Problem Definition (Weeks 5–6 of 36)

---

## Why a Mentor Is Not Optional

Almost every top ISEF AI/CS finalist has a PhD-level mentor. ISEF rules require an "adult sponsor" for all projects — but a mentor goes further:
- Gives you access to real research methods and thinking
- Provides feedback that judges would otherwise give you at the fair (too late)
- Gives your work credibility and depth
- Can sometimes provide GPU access, data, or lab resources

**The adult sponsor** (required by ISEF rules) approves your research plan before experimentation. This can be a teacher, but a university researcher is far stronger.

---

## Finding Mentor Candidates (Week 5)

### Strategy 1: Cold Email (Best ROI, takes effort)
Email 25–30 researchers whose published work directly relates to your research question. Expect 2–5 responses leading to 1–2 meetings.

**Who to target** (in order of likelihood to say yes):
1. **PhD students** (4th/5th year) — most likely to mentor; benefit from advising experience
2. **Postdoctoral researchers** — similar to PhD students; slightly more selective
3. **Assistant professors** — busy, but some love mentoring ambitious HS students
4. **Industry researchers** (Google Brain, DeepMind, OpenAI, etc.) — hard to reach but very impactful

**Where to find them**:
- Department websites at nearby universities (search "machine learning" + your domain)
- Google Scholar: authors of the papers you read in Weeks 2–3
- Twitter/X: ML researchers are active on social media; find authors discussing your topic
- LinkedIn: search "[domain] PhD student [university]"

### Cold Email Template (customize for every single email — do NOT send identical emails)

```
Subject: High School Student ISEF Research — [Their Specific Paper Title]

Dear [Name],

I am a [grade] student at [school] working on an ISEF science fair project in [domain]. 
I recently read your paper "[their paper title]" and found [specific thing you found 
interesting — one sentence that proves you read it].

I am investigating [your specific research question in one sentence]. My approach 
proposes [key innovation], which I believe addresses the limitation you noted in 
your paper regarding [specific limitation they mentioned].

I am seeking a faculty or graduate student mentor who could provide guidance on 
[specific type of help: methodology review / dataset access / weekly check-ins]. 
I would need approximately [X hours/month] of your time, primarily via email and 
one monthly video call.

I have already [what you've done so far: read 15 papers / reproduced the [paper] 
baseline / downloaded and explored the [dataset]].

Would you be willing to have a 20-minute video call to discuss whether this could 
be a good fit? I am happy to share my research plan draft in advance.

Thank you for your time,
[Your name]
[School name]
[Email]
```

**Key rules for cold emails**:
- Reference their specific paper — not "your research"
- One specific ask: "20-minute call" — not "be my mentor"
- Show you've already done work — don't ask them to define your project
- Keep it under 250 words
- Send on Tuesday–Thursday, 9am–noon (highest response rates)
- Follow up once after 2 weeks if no response

### Strategy 2: Structured Mentorship Programs (Paid, Higher Success Rate)

| Program | Cost | Quality | Notes |
|---------|------|---------|-------|
| **Polygence** | ~$2,000–4,000 | High | PhD mentors; explicitly designed for ISEF; 1:1 sessions |
| **Lumiere Research Scholar** | ~$2,200–3,400 | High | Selective; PhD mentors; outputs a research paper |
| **Inspirit AI** | ~$800–1,200 | Moderate | Group format; good for ML foundations; 25 hours |
| **Horizon Research** | ~$1,500–2,500 | Moderate | 600+ specializations; research paper output |
| **YRI Science** | Varies | High | Specifically targets ISEF; coaching + mentorship |

### Strategy 3: Local University Programs
- Many universities have formal HS research programs (often free)
- Search "[university name] high school research program" or "[university name] PRIMES" or "summer research program"
- These often have waitlists — apply early

---

## The First Mentor Meeting (Week 6)

If you get a meeting, prepare:
1. **2-minute overview** of your research question (use your pitch from Week 4)
2. **3 papers you've read** that are most relevant — be ready to discuss them
3. **Your baseline plan** — what experiments you want to run
4. **Specific ask** — what guidance do you need? (methodology? dataset? weekly check-ins?)
5. **Questions for them** — what do they see as the hardest part? Are there papers you missed?

**After the meeting**: Send a follow-up email within 24 hours summarizing what was discussed and confirming any agreements.

---

## What to Agree On with Your Mentor

Clarify these explicitly at the start:
- **Meeting cadence**: weekly? biweekly? async email only?
- **Role**: advisor only, or co-investigator? (affects how you acknowledge them)
- **ISEF forms**: Mentor may need to sign Form 1C (if working at a university/hospital lab)
- **Authorship**: ISEF projects are student projects. The mentor advises but does NOT do your experiments for you. Judges will probe this.
- **Expectations**: Don't overpromise. It's OK to say "I will update you weekly and need [specific guidance]."

---

## Parallel Task: Technical Foundation (Weeks 5–6)

While emailing for a mentor, build your technical foundation in your domain:

### If your project is deep learning:
- Complete fast.ai Part 1: https://course.fast.ai/ (free, 7 lessons, ~20 hours)
- OR Stanford CS231n (if computer vision): http://cs231n.stanford.edu/
- OR Stanford CS224N (if NLP): http://web.stanford.edu/class/cs224n/

### If your project is ML + biology:
- Rosalind (bioinformatics problems): http://rosalind.info/
- "Bioinformatics Algorithms" textbook (free online): https://bioinformaticsalgorithms.com/

### Set up your development environment:
```bash
# Install Python environment
pip install torch torchvision torchaudio  # or: conda install pytorch
pip install numpy pandas matplotlib seaborn scikit-learn
pip install jupyter wandb  # Weights & Biases for experiment tracking

# Version control
git init your_isef_project
# Create GitHub repo (private) — show judges you use real research tools
```

---

## Weeks 5–6 Deliverables
- [ ] Send 25–30 cold emails to potential mentors
- [ ] OR apply to Polygence/Lumiere/Inspirit by end of Week 5
- [ ] Follow up with any responses; schedule calls
- [ ] Complete fast.ai Part 1 (or equivalent course for your domain)
- [ ] Set up development environment + GitHub repo
- [ ] Successfully load and explore your dataset in a Jupyter notebook

## Mentor Tracking
| Name | Institution | Email sent | Response | Meeting scheduled |
|------|------------|-----------|---------|------------------|
| | | | | |
| | | | | |
| | | | | |

## Mentor Confirmed: _______________
