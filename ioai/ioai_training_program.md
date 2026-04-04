# IOAI Gold Medal Training Program
## 52-Week Weekly Plan — Targeting Top Individual Results

---

## Competition Overview

**IOAI** (International Olympiad in Artificial Intelligence) is a UNESCO-patronized high school science olympiad founded in 2024.

- **Format**: 2-day on-site individual contest (6 hours/day, ~3 tasks/day) + team challenge
- **Environment**: Python 3.12, PyTorch only (TensorFlow blocked), Scikit-learn, NumPy, Pandas; Jupyter-based
- **Internet**: Blocked except documentation (PyTorch, Scikit-learn, HuggingFace, NumPy docs)
- **AI tools**: Token-limited GPT-4o-mini for documentation lookups only
- **Scoring**: Continuous metric normalization (not binary pass/fail)
- **Medal rate**: ~50% of participants medal; gold is highly competitive

**Key insight from gold medalists**: Secure baselines first. Speed over novelty. Mathematical intuition > raw compute.

---

## Phase 0: Diagnostic Baseline (Week 0)

Before starting, assess your current level:

- [ ] Run through IOAI 2024 Day 1 Task 1 cold (no prep). Score it.
- [ ] Complete a Kaggle Titanic-style problem end to end in under 90 minutes.
- [ ] Implement a 3-layer MLP in pure PyTorch from scratch (no tutorials).
- [ ] Derive backpropagation for a 2-layer network on paper.

Use the results to identify which Phase to accelerate vs. slow down.

---

## Phase 1: Foundations (Weeks 1–8)
### Goal: Solid Python/ML/Math baseline; compete in USAAIO Round 1

---

### Week 1 — Python for ML & NumPy

**Theory (6h)**
- NumPy broadcasting rules: understand axes, reshape, einsum
- Vectorized operations vs. loops: performance implications
- Resource: NumPy docs + [CS231n Python/NumPy tutorial](http://cs231n.github.io/python-numpy-tutorial/)

**Practice (6h)**
- Implement matrix multiplication, softmax, cross-entropy loss in NumPy only (no libraries)
- Solve 10 NumPy exercises: [100 NumPy exercises](https://github.com/rougier/numpy-100)
- Time yourself: aim to implement softmax + cross-entropy in <15 minutes

**Deliverable**: `week01_numpy_fundamentals.ipynb`

---

### Week 2 — Pandas, Data Wrangling, EDA

**Theory (4h)**
- DataFrame operations: groupby, merge, pivot, apply, vectorized string ops
- Missing data strategies: imputation vs. deletion, when each is appropriate
- Outlier detection: IQR method, Z-score, domain-specific bounds

**Practice (8h)**
- Kaggle Learn: [Pandas micro-course](https://www.kaggle.com/learn/pandas) — complete all exercises
- EDA on Titanic dataset: generate 10 meaningful insights with visualizations (Matplotlib/Seaborn)
- Implement a full preprocessing pipeline (handle nulls, encode categoricals, scale features)

**Deliverable**: `week02_eda_pipeline.ipynb`

---

### Week 3 — Probability, Statistics, and Information Theory

**Theory (8h)**
- Probability: conditional probability, Bayes theorem, law of total probability
- Distributions: Normal, Binomial, Poisson, Bernoulli — PDFs, CDFs, moments
- MLE (Maximum Likelihood Estimation): derive for Gaussian and Bernoulli
- Information theory: entropy, KL divergence, cross-entropy — derive the connections
- Resources: Stanford CS229 notes (Lecture 1–3), [Goodfellow Chapter 3](https://www.deeplearningbook.org/)

**Practice (4h)**
- Implement MLE estimation for a Gaussian from scratch
- Show mathematically why cross-entropy loss = negative log-likelihood
- Solve 5 probability problems from AMC/AIME level as warm-up

**Deliverable**: Written derivations in `week03_probability_stats.pdf` or LaTeX

---

### Week 4 — Linear Algebra for ML

**Theory (8h)**
- Matrix operations: multiplication, inversion, transpose, determinant
- Eigenvalues/eigenvectors: geometric interpretation, power iteration
- SVD: how it factors any matrix, connection to PCA
- Norms: L1, L2, Frobenius — when each matters
- Resources: [3Blue1Brown Essence of Linear Algebra](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) (all videos), Goodfellow Chapter 2

**Practice (4h)**
- Implement PCA from scratch using only NumPy (compute covariance matrix, eigendecomposition)
- Verify your PCA matches Scikit-learn's on the Iris dataset
- Gold medalist exercise: reproduce Tudor Musat's IOAI 2025 "Antique Painting" trick — apply PCA to a dataset and visualize the spiral cluster pattern

**Deliverable**: `week04_pca_from_scratch.ipynb`

---

### Week 5 — Classical ML: Linear & Logistic Regression, KNN

**Theory (6h)**
- Linear regression: normal equation derivation, OLS, gradient descent form
- Logistic regression: sigmoid, cross-entropy loss, gradient derivation
- KNN: curse of dimensionality, distance metrics (Euclidean, Manhattan, cosine)
- Resources: CS229 Lecture Notes 1–3

**Practice (6h)**
- Implement linear regression with both normal equation and gradient descent; compare
- Implement logistic regression in NumPy (no Scikit-learn); test on a 2D dataset
- Scikit-learn: fit all three on 3 different Kaggle datasets; tune hyperparameters with cross-validation

**Deliverable**: `week05_regression_knn.ipynb`

---

### Week 6 — Trees: Decision Trees, Random Forests, Gradient Boosting

**Theory (6h)**
- Decision Trees: information gain, Gini impurity, tree depth vs. overfitting
- Random Forests: bagging, feature subsampling, OOB error
- Gradient Boosting: additive model formulation, XGBoost vs. LightGBM specifics
- Resources: CS229 notes + [XGBoost paper](https://arxiv.org/abs/1603.02754) (skim)

**Practice (6h)**
- Kaggle: [Intermediate ML course](https://www.kaggle.com/learn/intermediate-machine-learning) — complete all modules
- Tune an XGBoost model on a tabular Kaggle competition using Optuna for hyperparameter search
- Practice: submit to [House Prices](https://www.kaggle.com/c/house-prices-advanced-regression-techniques), aim for top 20%

**Deliverable**: `week06_trees_boosting.ipynb`

---

### Week 7 — Model Evaluation, Validation, and Feature Engineering

**Theory (4h)**
- Evaluation metrics: accuracy, precision, recall, F1, AUC-ROC, MSE, RMSE, MAE
- When to use which metric: class imbalance → AUC; equal error cost → accuracy
- Cross-validation: k-fold, stratified k-fold, time-series CV
- Overfitting diagnosis: learning curves, bias-variance tradeoff
- Feature engineering: polynomial features, interaction terms, target encoding, frequency encoding

**Practice (8h)**
- Implement AUC-ROC from scratch (sort by score, compute trapezoid area)
- Full ML pipeline: feature engineering → cross-validation → model selection → final submission
- Target: [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) — achieve AUC > 0.85

**Deliverable**: `week07_evaluation_pipeline.ipynb`

---

### Week 8 — Phase 1 Integration + USAAIO Mock Round 1

**Practice (12h)**
- Complete 3 full ML pipelines end-to-end (tabular) in under 90 minutes each
- Solve 10 USAAIO Round 1 style questions (theoretical AI/ML knowledge)
- Mock test: pick one IOAI 2024 at-home task, solve it under timed conditions (6 hours)
- Review all errors; write "lessons learned" doc

**Resources**:
- [USAAIO Past Problems](https://www.usaaio.org/past-problems)
- [IOAI 2024 GitHub](https://github.com/IOAI-official/IOAI-2024)

**Deliverable**: `week08_phase1_assessment.md` — self-assessment with gap analysis

---

## Phase 2: Deep Learning Core (Weeks 9–18)
### Goal: PyTorch mastery; CNNs; Transformers; NLP; compete in national selection

---

### Week 9 — Neural Networks from Scratch + PyTorch Basics

**Theory (6h)**
- Forward pass: activation functions (ReLU, sigmoid, tanh, GELU) — pros/cons
- Backpropagation: chain rule derivation for a 3-layer network
- Computational graphs: how PyTorch autograd builds them
- Resources: Goodfellow Chapter 6; [Andrej Karpathy's micrograd](https://github.com/karpathy/micrograd)

**Practice (6h)**
- Clone and study Karpathy's micrograd — understand every line
- Build a 3-layer MLP in pure PyTorch (using `torch.nn.Module`); train on MNIST
- Achieve >98% accuracy on MNIST with your custom MLP
- Time it: can you write the full training loop in <20 minutes?

**Deliverable**: `week09_mlp_pytorch.ipynb`

---

### Week 10 — Optimization and Regularization

**Theory (4h)**
- Optimizers: SGD with momentum, RMSProp, Adam — derive Adam update rules
- Learning rate schedules: cosine annealing, warmup, step decay
- Regularization: L1/L2 weight decay, dropout (derive expectation argument), batch normalization
- Gradient issues: vanishing/exploding gradients; gradient clipping

**Practice (8h)**
- Compare SGD vs. Adam vs. AdamW on the same task; plot loss curves
- Implement dropout from scratch; verify expected behavior
- Implement batch normalization from scratch; compare to `torch.nn.BatchNorm1d`
- Practical: train a model that overfits, then fix it with dropout + weight decay

**Deliverable**: `week10_optimization_regularization.ipynb`

---

### Week 11 — Convolutional Neural Networks

**Theory (6h)**
- Convolution: stride, padding, dilation — compute output dimensions manually
- Pooling: max, average, global average pooling
- CNN architectures: LeNet → AlexNet → VGG → ResNet (residual connections, why they work)
- Transfer learning: feature extraction vs. fine-tuning; when to unfreeze layers
- Resources: CS231n Lecture 5–9

**Practice (6h)**
- Implement a minimal ResNet-like block in PyTorch; train on CIFAR-10 (target: >90% accuracy)
- Fine-tune a pretrained ResNet-18 (torchvision.models) on a small custom dataset
- Data augmentation: implement RandomCrop, RandomFlip, ColorJitter; measure impact on test accuracy

**Deliverable**: `week11_cnn_resnet.ipynb`

---

### Week 12 — Computer Vision: Advanced Topics

**Theory (4h)**
- Semantic segmentation: FCN, UNet architecture (encoder-decoder + skip connections)
- Object detection overview: YOLO architecture (grid cells, anchor boxes, IoU, NMS)
- Evaluation metrics: mAP, IoU threshold, pixel accuracy
- Image augmentation for dense prediction: why flipping must transform labels too

**Practice (8h)**
- Implement UNet from scratch in PyTorch; train on Oxford Pets segmentation dataset
- Reproduce Stefan Asandei's IOAI 2025 chicken counting approach: UNet with attention + MSE loss
- Practice IOAI 2024 "Lab Repository" CV task from GitHub

**Resource**: [IOAI 2025 GitHub — Chicken Counting task](https://github.com/IOAI-official/IOAI-2025)

**Deliverable**: `week12_segmentation_detection.ipynb`

---

### Week 13 — Attention Mechanisms and Transformers (Part 1)

**Theory (8h)**
This is the most critical week. Deep transformer understanding is explicitly required by the IOAI syllabus.

- Self-attention: Q, K, V matrix derivation; scaled dot-product attention; why scale by sqrt(d_k)
- Multi-head attention: concatenation and projection; what different heads learn
- Positional encoding: sinusoidal vs. learned; RoPE basics
- Transformer encoder block: layer norm, FFN, residual connections
- Resources: [Attention Is All You Need](https://arxiv.org/abs/1706.03762), [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

**Practice (4h)**
- Implement multi-head self-attention from scratch in PyTorch (no `nn.MultiheadAttention`)
- Implement a single Transformer encoder block from scratch
- Test on a synthetic sequence classification task

**Deliverable**: `week13_attention_transformer_encoder.ipynb`

---

### Week 14 — Transformers (Part 2): BERT, GPT, ViT

**Theory (6h)**
- BERT: masked language modeling, NSP, bidirectional encoding, fine-tuning paradigm
- GPT: causal language modeling, autoregressive generation
- Vision Transformer (ViT): patch embeddings, class token, position embeddings
- CLIP: contrastive pre-training, zero-shot classification
- Pre-training vs. fine-tuning: LoRA, adapter layers, prompt tuning (conceptual)
- Resources: CS224n Lecture 9–12; [HuggingFace course](https://huggingface.co/learn/nlp-course)

**Practice (6h)**
- Fine-tune BERT on a text classification task using HuggingFace `transformers`
- Fine-tune ViT on an image classification task using HuggingFace
- Practice: IOAI 2025 "Concepts" NLP task (using LLM API via OpenRouter)

**Deliverable**: `week14_bert_gpt_vit.ipynb`

---

### Week 15 — NLP Pipeline: Classification, NER, QA

**Theory (4h)**
- Text preprocessing: tokenization (BPE, WordPiece), padding, attention masks
- Sequence labeling: NER with BIO tagging, CRF heads
- Question answering: extractive QA (SQuAD format), generative QA
- AI-generated text detection: perplexity-based, classifier-based approaches

**Practice (8h)**
- Complete [HuggingFace NLP Course](https://huggingface.co/learn/nlp-course) Chapters 1–4
- Build a complete NER pipeline on CoNLL-2003 dataset
- Reproduce IOAI 2024 NLP on-site problem notebook: [link](https://github.com/ssuslyakoff/examples/blob/main/IOAI%202024_%20NLP%20Problem%20on-site.ipynb)
- Implement AI-generated text detection (compare perplexity vs. classifier approach)

**Deliverable**: `week15_nlp_pipeline.ipynb`

---

### Week 16 — Generative Models: GANs and Diffusion

**Theory (4h)**
- GANs: minimax game, training instability, mode collapse, Wasserstein GAN
- Diffusion models: forward noising process, reverse denoising, DDPM, DDIM sampling
- Practical uses: image generation, image editing, inpainting
- IOAI relevance: team challenge (2024 used diffusion-based image generation tools)

**Practice (8h)**
- Train a small GAN on MNIST
- Use a pretrained diffusion model (Stable Diffusion via diffusers library) for controlled generation
- Practice IOAI 2024 Team Challenge: "Hogspell" — steer a diffusion model to generate images of hogs matching a target style; document your prompting/conditioning strategies

**Deliverable**: `week16_generative_models.ipynb`

---

### Week 17 — Unsupervised Learning & Dimensionality Reduction

**Theory (4h)**
- K-Means: algorithm, k-selection (elbow method, silhouette), limitations
- DBSCAN: density-based clusters, eps/min_samples, handles noise
- Hierarchical clustering: dendrogram, linkage methods
- PCA revisited: explained variance ratio, choosing components
- t-SNE: perplexity, high-dimensional to 2D, what it preserves vs. distorts
- UMAP: faster than t-SNE, better global structure

**Practice (8h)**
- Reproduce Tudor Musat's gold medal IOAI 2025 approach: PCA → visualize spiral cluster → manual class separation → t-SNE refinement
- Apply UMAP to a high-dimensional dataset; compare with PCA and t-SNE
- Practice IOAI 2025 "Antique Painting" task from GitHub

**Deliverable**: `week17_unsupervised_clustering.ipynb`

---

### Week 18 — Phase 2 Integration + National Selection Mock

**Practice (12h)**
- Timed mock: 2 IOAI 2024 on-site tasks, 3 hours each (Day 1 + Day 2 simulation)
- Complete one NEOAI 2025 Kaggle competition task
- Self-assessment: which task types are weakest? (CV segmentation? NLP? tabular?)
- Write personal "competition playbook": your go-to baselines for each task type

**Resources**:
- [NEOAI 2025 Kaggle](https://www.kaggle.com/competitions/neoai-2025/overview)
- [IOAI 2025 Solution Writeups](https://ioai-writeup.github.io/)
- [Awesome IOAI Tasks](https://github.com/open-cu/awesome-ioai-tasks)

**Deliverable**: `week18_competition_playbook.md`

---

## Phase 3: Advanced Topics + Competition Hardening (Weeks 19–30)
### Goal: Master advanced techniques; consistently strong on IOAI-difficulty problems

---

### Week 19 — Advanced PyTorch: Custom Training Loops & Efficiency

**Theory (4h)**
- Mixed precision training: float16 vs. bfloat16, torch.cuda.amp.autocast
- Gradient accumulation: simulate larger batch sizes
- Custom loss functions: when to subclass `nn.Module`
- DataLoader optimization: num_workers, pin_memory, prefetching
- Profiling: `torch.profiler`, identifying bottlenecks

**Practice (8h)**
- Rewrite a previous model with mixed precision; measure speed/memory improvement
- Implement gradient accumulation for fine-tuning BERT on a small GPU
- Implement 3 custom loss functions: focal loss, contrastive loss, dice loss
- Benchmark: train ResNet-50 and optimize data loading until GPU utilization >85%

**Deliverable**: `week19_pytorch_efficiency.ipynb`

---

### Week 20 — Anomaly Detection & Signal Processing

**Theory (4h)**
- Anomaly detection: Isolation Forest, Local Outlier Factor, Autoencoder-based
- One-class classification vs. binary classification for anomaly tasks
- Signal processing basics: Fourier transform, spectrogram, wavelet transform
- IOAI relevance: IOAI 2025 Day 1 Task 1 "Radar" was a signal/anomaly task

**Practice (8h)**
- Implement Isolation Forest and autoencoder-based anomaly detection; compare on same dataset
- Practice IOAI 2025 "Radar" task from GitHub
- Build a classifier on audio spectrograms (use RAVDESS or UrbanSound dataset)

**Deliverable**: `week20_anomaly_signal.ipynb`

---

### Week 21 — Computer Vision: Counting, Matching, and Metric Learning

**Theory (4h)**
- Density estimation for counting: predict density maps, sum for count
- Image matching: feature descriptors (ORB, SIFT conceptually), learned descriptors
- Metric learning: contrastive loss, triplet loss, cosine similarity in embedding space
- SimCLR, DINO: self-supervised visual representations

**Practice (8h)**
- Implement density map approach for object counting (key technique for "Chicken Counting" IOAI 2025 task)
- Practice IOAI 2025 "Restroom Icon Matching" task
- Build a Siamese network for image similarity using triplet loss

**Deliverable**: `week21_counting_matching.ipynb`

---

### Week 22 — Advanced NLP: LLMs, Prompting, Fine-Tuning

**Theory (4h)**
- LLM capabilities and limitations: in-context learning, hallucination, factuality
- Prompting strategies: zero-shot, few-shot, chain-of-thought, role prompting
- Parameter-efficient fine-tuning: LoRA, prefix tuning, adapter layers
- RAG (Retrieval-Augmented Generation): conceptual understanding

**Practice (8h)**
- Fine-tune a small LLM (GPT-2 or LLaMA-3-1B) on a domain-specific task using LoRA
- Build a prompting system for the IOAI 2025 "Concepts" NLP task using OpenRouter API
- Implement AI-generated text detection using perplexity scoring + classifier ensemble

**Deliverable**: `week22_llm_finetuning.ipynb`

---

### Week 23 — Constrained Optimization and Mathematical ML

**Theory (6h)**
- Convex optimization: gradient descent convergence, convexity conditions
- Constrained optimization: Lagrange multipliers (SVM dual derivation)
- SVM: hard margin, soft margin, kernel trick (RBF, polynomial), dual problem
- Combinatorial optimization basics: relevance to ML (e.g., Hungarian algorithm for matching)
- IOAI relevance: "Pixel Parsimony" (IOAI 2025) was a constrained optimization problem

**Practice (6h)**
- Derive the SVM dual problem from scratch
- Practice IOAI 2025 "Pixel Parsimony/Efficiency" task
- Implement a simple constrained optimization solver using scipy.optimize

**Deliverable**: `week23_optimization_svm.ipynb`

---

### Week 24 — Multimodal ML: Vision + Language

**Theory (4h)**
- CLIP: contrastive image-text pretraining, zero-shot transfer
- BLIP, LLaVA: vision-language instruction tuning
- Multimodal fusion strategies: early fusion, late fusion, cross-attention
- IOAI relevance: IOAI 2024 "Lab Repository" required multimodal fusion (image + text)

**Practice (8h)**
- Use CLIP for zero-shot classification; compare with fine-tuned ResNet on same task
- Build a multimodal classifier (image + tabular metadata): fuse CNN features with MLP features
- Practice IOAI 2024 CV task with multimodal fusion component

**Deliverable**: `week24_multimodal.ipynb`

---

### Week 25 — AI Ethics, Fairness, Robustness, and Adversarial ML

**Theory (6h)**
- Bias types: historical bias, representation bias, measurement bias
- Fairness metrics: demographic parity, equalized odds, calibration — conflicts between them
- Adversarial attacks: FGSM, PGD, C&W — how they fool classifiers
- Adversarial defense: adversarial training, input preprocessing, certified defenses
- Model quantization: int8, int4, post-training quantization vs. QAT
- IOAI relevance: IOAI 2024 on-site ML task involved adversarial attacks and quantization

**Practice (6h)**
- Implement FGSM attack on a trained image classifier; measure accuracy degradation
- Implement adversarial training; compare robustness vs. standard training
- Practice IOAI 2024 on-site ML adversarial/quantization task

**Deliverable**: `week25_adversarial_ethics.ipynb`

---

### Week 26 — Speed Training: The 90-Minute Task Challenge

**Focus**: Competitive time management (the most important meta-skill)

Competition reality: 6 hours / 3 tasks = ~90 minutes per task including:
- Reading problem statement and understanding data: 10 min
- EDA and baseline: 20 min
- First model + submission: 30 min
- Iterate 2–3 improvements: 30 min
- Buffer for re-evaluation wait: 10 min

**Practice (12h)**
- Timed: solve 4 different ML tasks (tabular, CV, NLP, anomaly) in exactly 90 min each
- Requirement: must make at least one Kaggle submission per task
- After each: write a 5-minute post-mortem: what was the bottleneck?
- Build your personal "15-minute baseline" template for each task type

**Deliverable**: `week26_speed_drills/` — 4 solution notebooks + `postmortems.md`

---

### Week 27 — APOAI/NEOAI Full Mock Competition

**Practice (12h)**
- Complete all available APOAI and NEOAI problems from the awesome-ioai-tasks repo
- Simulate competition conditions: no personal notes allowed, limited documentation
- APOAI tasks to cover: "Evading AI-Generated Text Detection," "Broken BERT," "Cluster Pictures," "Cuties Segmentation"
- Score each task; benchmark against known leaderboard scores

**Resources**:
- [Awesome IOAI Tasks](https://github.com/open-cu/awesome-ioai-tasks)
- [Romanian competitor solutions](https://github.com/stefanasandei/roai-solved)

**Deliverable**: `week27_apoai_neoai_mock/` — solutions + score analysis

---

### Week 28 — Weak Area Deep Dive (Personalized)

Based on your Week 18 and Week 27 post-mortems, identify your two weakest domains and do a targeted deep dive.

**Example weak areas and resources**:

| Weak Area | Resources |
|-----------|-----------|
| Segmentation | [UNet paper](https://arxiv.org/abs/1505.04597) + Kaggle [Carvana](https://www.kaggle.com/c/carvana-image-masking-challenge) |
| Transformers from scratch | Karpathy's [nanoGPT](https://github.com/karpathy/nanoGPT) |
| Signal processing | [librosa docs](https://librosa.org/doc/latest/index.html) + DCASE challenges |
| Tabular + feature engineering | [Feature Engineering book](https://www.oreilly.com/library/view/feature-engineering-for/9781491953235/) |
| Constrained optimization | [SciPy optimization tutorial](https://docs.scipy.org/doc/scipy/tutorial/optimize.html) |
| LLM fine-tuning | [PEFT library docs](https://huggingface.co/docs/peft) |

**Deliverable**: `week28_weak_area_drills.ipynb`

---

### Week 29 — Theoretical Depth: Goodfellow Chapters

**Theory (10h)**
Work through [Deep Learning book](https://www.deeplearningbook.org/) — the sections most tested at IOAI:
- Chapter 5: Machine Learning Basics (capacity, overfitting, bias-variance)
- Chapter 6: Deep Feedforward Networks (activation functions, gradient-based learning)
- Chapter 7: Regularization (L1/L2, dropout, data augmentation, multi-task learning)
- Chapter 9: Convolutional Networks (pooling, architectural patterns)
- Chapter 10: Sequence Modeling (RNN, LSTM conceptual)

**Practice (2h)**
- Derive 5 results from the book on paper without looking
- Write 1-page summaries of each chapter's key insights

**Deliverable**: `week29_theory_depth.md`

---

### Week 30 — Full IOAI 2025 Simulation (Day 1 + Day 2)

**Practice (14h total over 2 days)**

**Day 1 simulation (6h)**: IOAI 2025 Day 1 tasks
- Radar (signal/anomaly detection)
- Chicken Counting (UNet-based counting)
- Concepts (NLP with LLM API)

**Day 2 simulation (6h)**: IOAI 2025 Day 2 tasks (harder)
- Restroom Icon Matching (image matching)
- Antique Painting Authentication (PCA + clustering)
- Pixel Parsimony (constrained optimization)

**Post-simulation (2h)**:
- Compare your scores against known gold medalist solutions
- Read all official solution writeups: [ioai-writeup.github.io](https://ioai-writeup.github.io/)
- Document: where did you lose points? Time management? Algorithm choice? Implementation bugs?

**Deliverable**: `week30_full_simulation/` — all 6 solutions + `gap_analysis.md`

---

## Phase 4: Pre-Competition Peak (Weeks 31–40)
### Goal: Competition-ready; consistent gold-medal-level performance

---

### Week 31 — Competition Playbook: Baselines for Every Task Type

Build and memorize your personal "starter kit" — code you can write from memory in under 15 minutes for each task type:

**Tabular Baseline** (XGBoost + cross-validation + feature engineering template)
**CV Classification Baseline** (pretrained ResNet-50, fine-tune last 2 layers, augmentation)
**CV Segmentation Baseline** (UNet, dice loss, Adam, LR scheduler)
**NLP Classification Baseline** (BERT fine-tune, tokenizer, training loop)
**Counting/Density Baseline** (UNet with MSE on density maps)
**Anomaly Detection Baseline** (Isolation Forest + Autoencoder ensemble)
**Clustering Baseline** (PCA → t-SNE/UMAP visualization → K-means or DBSCAN)

**Practice (12h)**
- Write each baseline from memory without referencing previous notebooks
- Target: under 15 minutes per baseline type, running and submitting
- Refine until each is clean, fast, and correct

**Deliverable**: `week31_starter_kit/` — 7 template notebooks

---

### Weeks 32–33 — Historical Problem Marathon

**Practice (20h over 2 weeks)**

Systematically solve every available IOAI and national selection problem:

| Source | Problems Available |
|--------|-------------------|
| IOAI 2024 GitHub | 3+ tasks with solutions |
| IOAI 2025 GitHub | 6 tasks with solutions |
| NEOAI 2025 Kaggle | 8 tasks |
| APOAI | 4+ tasks |
| Polish OAI | Multiple tasks |
| Romanian ONIA | Multiple tasks |
| Japanese JOAI | Multimodal gas classification |
| USAAIO past problems | Multiple rounds |

For each problem:
1. Solve it yourself first (timed)
2. Read the reference/gold solution
3. Document: what technique did you miss?

**Deliverable**: `week32-33_problem_marathon/` — solutions + technique log

---

### Week 34 — Kaggle Sprint: Active Competition

**Practice (12h)**
- Join an active Kaggle competition in a domain you're weakest in
- Focus: iterate fast, submit often, read top public notebooks
- Target: top 20% finish
- Goal is not the competition result itself but the speed of the iteration loop

**Deliverable**: Kaggle competition profile link + `week34_competition_notes.md`

---

### Week 35 — Mathematical Reasoning Under Pressure

**Theory + Practice (12h)**
IOAI gold medalists are distinguished by mathematical intuition, not just engineering skill.

- Derive 10 ML results from memory: MLE for Gaussian, SVM dual, backprop for softmax, attention gradient, BN backward pass
- Practice: given a new dataset with a hidden structure (like the IOAI 2025 spiral clusters), can you visually diagnose it in under 5 minutes?
- Exercises: write derivations for attention mechanism, PCA variance maximization, Bayes optimal classifier
- Practice "visual ML": given a scatter plot, predict which algorithm will work best and why

**Deliverable**: `week35_math_drills.pdf`

---

### Week 36 — Hyperparameter Optimization & AutoML Techniques

**Theory (4h)**
- Grid search vs. random search vs. Bayesian optimization (Optuna)
- Early stopping strategies: patience-based, ReduceLROnPlateau
- Ensemble methods: voting, stacking, blending — when to use each
- Post-processing tricks: calibration (Platt scaling, isotonic regression), threshold tuning

**Practice (8h)**
- Set up Optuna for automated hyperparameter tuning on 3 tasks; compare to manual tuning
- Implement a stacking ensemble (meta-learner on out-of-fold predictions)
- IOAI-specific: implement threshold optimization for F1 score

**Deliverable**: `week36_hpopt_ensembles.ipynb`

---

### Week 37 — Team Challenge Preparation

**Practice (8h)**
IOAI team challenges change annually. Prepare for likely formats:

- **Diffusion/image generation** (2024 format): practice steering Stable Diffusion with ControlNet, IP-Adapter; understand CFG, negative prompts
- **Robotics/simulation** (2025 format): practice [PyBullet](https://pybullet.org/) or [MuJoCo](https://mujoco.org/) basics; understand robot arm kinematics conceptually
- **2026 unknowns**: likely multimodal or agentic format; practice tool use with LLMs

**Deliverable**: `week37_team_challenge_prep.md`

---

### Week 38 — AI Safety, Fairness, and Ethics (Deep Dive)

**Theory (6h)**
- Algorithmic fairness: case studies (COMPAS recidivism, hiring algorithms)
- AI safety: alignment problem, reward hacking, specification gaming
- Model cards and datasheets: how to document ML systems responsibly
- Privacy: differential privacy, federated learning basics
- Explainability: SHAP values, LIME, gradient-based attribution

**Practice (6h)**
- Compute SHAP values for an XGBoost model; interpret feature importance
- Implement a fairness-aware classifier with demographic parity constraint
- Read: [Fairness and ML (Hardt et al.)](https://fairmlbook.org/)

**Deliverable**: `week38_ethics_fairness.ipynb`

---

### Week 39 — Full 2-Day Mock Competition (IOAI 2024 Format)

**Practice (14h over 2 days)**

Recreate exact IOAI 2024 conditions:
- No personal notes; only whitelisted documentation (PyTorch, Scikit-learn, NumPy, HuggingFace, Python docs)
- Token-limited AI assistant: allow yourself 10 GPT-4o-mini queries maximum per 6-hour session
- Simulate evaluation wait time (10 min per submission)

**Day 1**: IOAI 2024 at-home tasks (6h)
**Day 2**: IOAI 2024 on-site tasks (6h)

Post-competition:
- Score objectively against provided ground truth
- Benchmark against public leaderboard positions
- Final gap analysis: what remains between you and gold?

**Deliverable**: `week39_final_mock/` — full solutions + score report

---

### Week 40 — Weak Points Final Sweep

Based on Week 39 gap analysis: targeted fixes for the last remaining weaknesses.

**High-value focus areas** (based on what distinguishes gold from silver at IOAI):
- Speed: if you're spending >2h on any single task, time yourself until you're under 90 min
- Mathematical insight: can you visualize data structure in <5 minutes?
- PyTorch fluency: write any model architecture without referencing documentation
- Baseline → improvement loop: practice submitting baseline in 30 min, then iterating

**Deliverable**: `week40_final_gaps.md`

---

## Phase 5: Peak Performance Maintenance (Weeks 41–48)
### Goal: Maintain sharpness; avoid burnout; integrate final techniques

---

### Week 41 — Cutting-Edge Papers (What Gold Medalists Know)

**Theory (8h)**
Read and understand these papers that directly appear in IOAI problems:

| Paper | Why It Matters |
|-------|---------------|
| [Attention Is All You Need](https://arxiv.org/abs/1706.03762) | Core transformer architecture |
| [BERT](https://arxiv.org/abs/1810.04805) | Pre-training paradigm |
| [ViT: An Image is Worth 16x16 Words](https://arxiv.org/abs/2010.11929) | Vision transformers |
| [CLIP](https://arxiv.org/abs/2103.00020) | Multimodal contrastive learning |
| [UNet](https://arxiv.org/abs/1505.04597) | Segmentation and counting |
| [ResNet](https://arxiv.org/abs/1512.03385) | Residual learning |
| [DDPM (Diffusion)](https://arxiv.org/abs/2006.11239) | Generative models |
| [XGBoost](https://arxiv.org/abs/1603.02754) | Gradient boosting theory |

**Practice (4h)**: For each paper, write a 3-sentence summary of the core idea and implement the key equation.

**Deliverable**: `week41_papers_summary.md`

---

### Week 42 — Advanced Computer Vision: ViT, DINO, SAM

**Theory (4h)**
- ViT in depth: patch tokenization, class token classification, position embedding ablations
- DINO / DINOv2: self-supervised ViT training, emergent segmentation properties
- SAM (Segment Anything Model): promptable segmentation, architecture

**Practice (8h)**
- Use DINOv2 features for a zero-shot classification task; compare with CLIP
- Use SAM for interactive segmentation on a custom dataset
- Build a ViT fine-tuning pipeline with gradual unfreezing strategy

**Deliverable**: `week42_advanced_vision.ipynb`

---

### Week 43 — Advanced NLP: Instruction Tuning, RAG, Evals

**Theory (4h)**
- Instruction tuning: how ChatGPT-style models are trained (SFT + RLHF overview)
- LLM evaluation: perplexity, BLEU, ROUGE, BERTScore, human eval
- RAG pipeline: chunking, embedding, retrieval, generation; when to use
- Hallucination: types, detection, mitigation strategies

**Practice (8h)**
- Build a simple RAG system using FAISS + HuggingFace embeddings
- Evaluate an LLM's hallucination rate on a knowledge-intensive QA task
- Practice APOAI "Evading AI-Generated Text Detection" task

**Deliverable**: `week43_advanced_nlp.ipynb`

---

### Weeks 44–45 — Kaggle Grand Competition (Full Sprint)

**Practice (20h over 2 weeks)**

Join a Kaggle competition in your strongest domain and pursue a competitive finish:
- Target: top 10% finish
- Strategy: fast baseline → systematic ablations → ensemble final models
- Keep a competition diary: every decision, what worked, what didn't

This builds genuine competition experience under real pressure.

**Deliverable**: Kaggle result + `week44-45_competition_diary.md`

---

### Week 46 — Robotics and Simulation Basics (Team Challenge Prep)

**Theory + Practice (8h)**
Since team challenges often involve robotics/simulation (IOAI 2025 used Galbot):

- Python robot programming basics: joint control, path planning, gripper control
- PyBullet: basic simulation, loading URDF, applying forces
- Sim-to-real concepts: domain randomization, why simulation transfer is hard
- Practice: solve a simple pick-and-place task in PyBullet simulation

**Deliverable**: `week46_robotics_basics.ipynb`

---

### Week 47 — Mental Preparation and Competition Strategy

**Strategy Review (4h)**
Study the competition strategies of top performers:

**Tudor Morariu (Romania, Silver 2025)**:
> "Experience beats novelty. In a 6-hour window, rely on what you've tested."

**Stefan Asandei (Romania, Bronze 2025)**:
> "Do not overthink. Secure the baseline before experimenting."

**Russian team preparation principle**:
> Strong mathematical foundation is the core differentiator.

**Your personal competition checklist**:
- [ ] 30 min before: review your starter kit templates mentally
- [ ] Task start: read full problem (10 min), EDA (15 min), baseline (20 min), first submit (45 min total)
- [ ] First submit done: now iterate systematically, one change at a time
- [ ] 60 min remaining: stop experimenting, polish best submission
- [ ] Final 15 min: double-check submission format, verify output shape

**Practice (8h)**: Run two 3-hour mock sessions with a timer.

**Deliverable**: `week47_competition_strategy.md` — your personal playbook

---

### Week 48 — Final Full Mock (IOAI 2025 Format)

**Practice (14h over 2 days)**

Exact IOAI 2025 simulation:
- Bohrium-style environment (or Kaggle Notebooks as substitute)
- No personal notes; whitelisted docs only
- 10 GPT-4o-mini queries maximum per day
- 6 tasks across 2 days

Score yourself, compare to published gold solutions, document final status.

**Deliverable**: `week48_final_mock_2025/` — solutions + medal-level assessment

---

## Phase 6: Competition Month (Weeks 49–52)
### Goal: Rest, review, compete

---

### Week 49 — Rest and Light Review

- No new learning. Solve 1 easy problem per day to stay sharp.
- Review your Week 31 starter kit; make sure it's memorized.
- Sleep 8+ hours. Physical exercise daily.
- Read competition rules and logistics carefully.

---

### Week 50 — Light Warm-Up Problems

- Solve 2–3 problems from previous national selections (easy/medium difficulty only)
- Review Week 47 competition strategy checklist
- Ensure PyTorch and Scikit-learn environment is set up and familiar
- Practice writing code without documentation (build documentation independence)

---

### Week 51 — Competition Week (Pre-Competition)

**If at-home task is assigned (IOAI format)**:
- Treat it as your most important deliverable
- Follow your competition playbook: baseline → iterate → document reasoning
- Submit early versions; improve iteratively
- Keep notes on every experiment with its score

**If no at-home task**: light maintenance + mental preparation

---

### Week 52 — On-Site Competition

**Competition days**:
- Morning routine: light exercise, good breakfast
- Task 1: follow playbook — read (10 min) → EDA (15 min) → baseline (20 min) → iterate
- Between tasks: 5-minute break, reset mentally
- Final hour: no new experiments, polish and verify submissions

---

## Resource Master List

### Official IOAI
| Resource | URL |
|----------|-----|
| Official website | https://ioai-official.org/ |
| Syllabus 2026 | https://ioai-official.org/wp-content/uploads/2025/10/Syllabus.pdf |
| IOAI 2025 GitHub | https://github.com/IOAI-official/IOAI-2025 |
| IOAI 2024 GitHub | https://github.com/IOAI-official/IOAI-2024 |
| IOAI 2025 Datasets | https://huggingface.co/datasets/IOAI-official/IOAI2025 |
| Solution writeups | https://ioai-writeup.github.io/ |
| Awesome IOAI Tasks | https://github.com/open-cu/awesome-ioai-tasks |
| Romanian solved problems | https://github.com/stefanasandei/roai-solved |

### Courses
| Course | URL |
|--------|-----|
| Fast.ai Practical Deep Learning | https://course.fast.ai/ |
| MIT 6.S191 Intro to Deep Learning | http://introtodeeplearning.com/ |
| Stanford CS231n (Computer Vision) | http://cs231n.stanford.edu/ |
| Stanford CS224n (NLP) | http://web.stanford.edu/class/cs224n/ |
| Stanford CS229 (ML) | https://cs229.stanford.edu/ |
| HuggingFace NLP Course | https://huggingface.co/learn/nlp-course |

### Textbooks
| Book | URL |
|------|-----|
| Dive into Deep Learning | https://d2l.ai/ |
| Deep Learning (Goodfellow) | https://www.deeplearningbook.org/ |
| Fairness and ML | https://fairmlbook.org/ |

### Practice Platforms
| Platform | URL |
|----------|-----|
| Kaggle Competitions | https://www.kaggle.com/competitions |
| NEOAI 2025 | https://www.kaggle.com/competitions/neoai-2025 |
| USAAIO Past Problems | https://www.usaaio.org/past-problems |
| PyTorch Tutorials | https://pytorch.org/tutorials/ |

### National Selections
| Country | URL |
|---------|-----|
| USA (USAAIO) | https://www.usaaio.org/ |
| India (INAIO) | https://india.acm.org/education/inaio |
| UK (UKOAI) | https://ukoai.org/ |
| Canada | https://www.ioai-canada.org/ |
| Singapore | https://aisingapore.org/talent/national-olympiad-in-artificial-intelligence/ |

---

## Weekly Time Budget

| Phase | Weekly Hours | Distribution |
|-------|-------------|--------------|
| Phase 1 (Weeks 1–8) | 12h | 6h theory + 6h practice |
| Phase 2 (Weeks 9–18) | 12h | 4–6h theory + 6–8h practice |
| Phase 3 (Weeks 19–30) | 12h | Mostly practice + timed drills |
| Phase 4 (Weeks 31–40) | 14h | Heavy competition simulation |
| Phase 5 (Weeks 41–48) | 12h | Maintenance + advanced topics |
| Phase 6 (Weeks 49–52) | 4–6h | Rest + compete |

Total: ~600 hours over 52 weeks (roughly 1.5–2 hours/day on weekdays + longer weekend sessions)

---

## Gold Medal Principles (Memorize These)

1. **Secure the baseline first.** Always get a working submission before experimenting.
2. **Time is your scarcest resource.** 90 minutes per task. Do not over-invest in one problem.
3. **Surgical improvements over ambition.** Proven techniques beat novel ideas in a 6-hour window.
4. **Mathematical intuition is the differentiator.** Visualize data structure. PCA/t-SNE in the first 5 minutes.
5. **Know your toolkit cold.** Write PyTorch models without referencing documentation.
6. **Understand the metric.** Different optimization strategies for AUC vs. F1 vs. MSE.
7. **Never overthink.** (Stefan Asandei, Bronze 2025: "In hindsight, I overprepared for tasks by learning techniques that were too complicated.")
8. **Experience beats novelty.** (Tudor Morariu, Silver 2025: "Rely on what you've tested.")

---

*Program designed targeting IOAI Individual Contest gold medal. Last updated: April 2026.*
*Sources: IOAI official website, IOAI 2024/2025 GitHub repositories, USAAIO, competitor interviews, IOAI solution writeups.*
