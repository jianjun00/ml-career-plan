# Week 15 — NLP Pipeline: Classification, NER, QA
## Phase 2: Deep Learning Core (Week 15 of 18)

---

## Theory (4h)

- **Text preprocessing**: tokenization (BPE, WordPiece), padding, attention masks
- **Sequence labeling**: NER with BIO tagging, CRF heads
- **Question answering**: extractive QA (SQuAD format), generative QA
- **AI-generated text detection**: perplexity-based, classifier-based approaches

---

## BIO Tagging for NER
```
"Apple Inc. is based in Cupertino ."
 B-ORG I-ORG O  O    O  B-LOC     O

# B = Beginning of entity, I = Inside entity, O = Outside
# Model outputs one label per token
```

## Full NER Pipeline
```python
from transformers import AutoModelForTokenClassification, AutoTokenizer
import torch

tokenizer = AutoTokenizer.from_pretrained("dslim/bert-base-NER")
model = AutoModelForTokenClassification.from_pretrained("dslim/bert-base-NER")

text = "Apple Inc. is headquartered in Cupertino, California."
inputs = tokenizer(text, return_tensors="pt")
with torch.no_grad():
    outputs = model(**inputs)
predictions = outputs.logits.argmax(-1)[0]
tokens = tokenizer.convert_ids_to_tokens(inputs['input_ids'][0])
labels = [model.config.id2label[p.item()] for p in predictions]
```

## Perplexity-based AI detection
```python
from transformers import GPT2LMHeadModel, GPT2TokenizerFast
import torch

def compute_perplexity(text, model, tokenizer):
    inputs = tokenizer(text, return_tensors='pt')
    with torch.no_grad():
        loss = model(**inputs, labels=inputs['input_ids']).loss
    return torch.exp(loss).item()

# AI-generated text: lower perplexity (model is confident)
# Human text: higher perplexity (more natural variation)
# Threshold: tune on labeled examples
```

---

## Practice (8h)

- Complete [HuggingFace NLP Course](https://huggingface.co/learn/nlp-course) Chapters 1–4
- Build a complete NER pipeline on CoNLL-2003 dataset
- Reproduce [IOAI 2024 NLP on-site problem notebook](https://github.com/ssuslyakoff/examples/blob/main/IOAI%202024_%20NLP%20Problem%20on-site.ipynb)
- Implement AI-generated text detection (compare perplexity vs. classifier approach)

---

## Deliverable

`week15_nlp_pipeline.ipynb` — NER pipeline + QA pipeline + AI-text detection comparison

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
