# Week 43 — Advanced NLP: Instruction Tuning, RAG, Evals
## Phase 5: Peak Performance Maintenance (Week 43 of 48)

---

## Theory (4h)

- **Instruction tuning**: how ChatGPT-style models are trained (SFT + RLHF overview)
- **LLM evaluation**: perplexity, BLEU, ROUGE, BERTScore, human eval
- **RAG pipeline**: chunking, embedding, retrieval, generation; when to use
- **Hallucination**: types, detection, mitigation strategies

---

## RAG System with FAISS
```python
from transformers import AutoTokenizer, AutoModel
import faiss
import numpy as np

# Step 1: Embed documents
def embed(texts, model, tokenizer):
    inputs = tokenizer(texts, return_tensors='pt', truncation=True, padding=True, max_length=512)
    with torch.no_grad():
        embeddings = model(**inputs).last_hidden_state[:,0,:]  # CLS token
    return F.normalize(embeddings, dim=-1).numpy()

encoder = AutoModel.from_pretrained('sentence-transformers/all-MiniLM-L6-v2')
tokenizer = AutoTokenizer.from_pretrained('sentence-transformers/all-MiniLM-L6-v2')

doc_embeddings = embed(documents, encoder, tokenizer)  # (n_docs, 384)

# Step 2: Build FAISS index
index = faiss.IndexFlatIP(384)   # inner product = cosine similarity (with normalized vecs)
index.add(doc_embeddings)

# Step 3: Retrieve top-k for query
query_emb = embed([query], encoder, tokenizer)
distances, indices = index.search(query_emb, k=5)
retrieved_docs = [documents[i] for i in indices[0]]

# Step 4: Generate with context
context = "\n\n".join(retrieved_docs)
prompt = f"Context:\n{context}\n\nQuestion: {query}\nAnswer:"
```

## BERTScore Evaluation
```python
from bert_score import score

P, R, F1 = score(predictions, references, lang='en', verbose=True)
print(f"BERTScore F1: {F1.mean():.4f}")
# BERTScore computes token-level similarity using BERT embeddings
# Better than BLEU for semantically equivalent outputs
```

---

## Practice (8h)

- Build a simple RAG system using FAISS + HuggingFace embeddings
- Evaluate an LLM's hallucination rate on a knowledge-intensive QA task
- Practice [APOAI "Evading AI-Generated Text Detection" task](https://github.com/open-cu/awesome-ioai-tasks)

---

## Deliverable

`week43_advanced_nlp.ipynb` — RAG pipeline + BERTScore evaluation + AI text detection system

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 5: Peak Performance Maintenance*
