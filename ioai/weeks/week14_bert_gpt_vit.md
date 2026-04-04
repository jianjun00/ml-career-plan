# Week 14 — Transformers (Part 2): BERT, GPT, ViT
## Phase 2: Deep Learning Core (Week 14 of 18)

---

## Theory (6h)

- **BERT**: masked language modeling, NSP, bidirectional encoding, fine-tuning paradigm
- **GPT**: causal language modeling, autoregressive generation
- **Vision Transformer (ViT)**: patch embeddings, class token, position embeddings
- **CLIP**: contrastive pre-training, zero-shot classification
- **Pre-training vs. fine-tuning**: LoRA, adapter layers, prompt tuning (conceptual)

**Resources**: CS224n Lecture 9–12; [HuggingFace course](https://huggingface.co/learn/nlp-course)

---

## BERT vs GPT: Key differences

| | BERT | GPT |
|--|------|-----|
| Attention | Bidirectional (full) | Causal (masked) |
| Pre-training | MLM + NSP | Next token prediction |
| Use case | Classification, NER, QA | Generation |
| Fine-tuning | Add head, fine-tune | Few-shot prompting or fine-tune |

## HuggingFace BERT fine-tuning template
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, TrainingArguments, Trainer

tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)

def tokenize(batch):
    return tokenizer(batch['text'], truncation=True, padding='max_length', max_length=128)

dataset = dataset.map(tokenize, batched=True)

training_args = TrainingArguments(
    output_dir='./results', num_train_epochs=3,
    per_device_train_batch_size=16, evaluation_strategy='epoch',
    learning_rate=2e-5, weight_decay=0.01
)
trainer = Trainer(model=model, args=training_args, train_dataset=dataset['train'], eval_dataset=dataset['test'])
trainer.train()
```

## ViT key concept
```python
# ViT: divide 224x224 image into 16x16 patches = 196 patches
# Each patch linearly projected to embedding dim
# Prepend [CLS] token; add learnable position embeddings
# Run through Transformer encoder; classify from [CLS] output

class ViTPatchEmbed(nn.Module):
    def __init__(self, img_size=224, patch_size=16, in_chans=3, embed_dim=768):
        super().__init__()
        self.proj = nn.Conv2d(in_chans, embed_dim, kernel_size=patch_size, stride=patch_size)
    def forward(self, x):
        return self.proj(x).flatten(2).transpose(1, 2)  # (B, num_patches, embed_dim)
```

---

## Practice (6h)

- Fine-tune BERT on a text classification task using HuggingFace `transformers`
- Fine-tune ViT on an image classification task using HuggingFace
- Practice IOAI 2025 "Concepts" NLP task

---

## Deliverable

`week14_bert_gpt_vit.ipynb` — BERT fine-tune + ViT fine-tune + results comparison

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
