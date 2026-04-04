# Week 13 — Attention Mechanisms and Transformers (Part 1)
## Phase 2: Deep Learning Core (Week 13 of 18)

**This is the most critical week. Deep transformer understanding is explicitly required by the IOAI syllabus.**

---

## Theory (8h)

- **Self-attention**: Q, K, V matrix derivation; scaled dot-product attention; why scale by √d_k
- **Multi-head attention**: concatenation and projection; what different heads learn
- **Positional encoding**: sinusoidal vs. learned; RoPE basics
- **Transformer encoder block**: layer norm, FFN, residual connections

**Resources**: [Attention Is All You Need](https://arxiv.org/abs/1706.03762), [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

---

## Key Derivations

### Why scale by √d_k?
Without scaling, dot products grow with d_k → softmax saturates → gradients vanish.
Variance of QKᵀ ≈ d_k if Q,K ~ N(0,1). Dividing by √d_k restores unit variance.

### Scaled Dot-Product Attention (from scratch)
```python
import torch
import torch.nn.functional as F

def scaled_dot_product(Q, K, V, mask=None):
    d_k = Q.shape[-1]
    scores = (Q @ K.transpose(-2, -1)) / (d_k ** 0.5)  # (batch, heads, seq, seq)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    attn = F.softmax(scores, dim=-1)
    return attn @ V, attn

class MultiHeadAttention(torch.nn.Module):
    def __init__(self, d_model, num_heads):
        super().__init__()
        assert d_model % num_heads == 0
        self.d_k = d_model // num_heads
        self.num_heads = num_heads
        self.W_q = torch.nn.Linear(d_model, d_model)
        self.W_k = torch.nn.Linear(d_model, d_model)
        self.W_v = torch.nn.Linear(d_model, d_model)
        self.W_o = torch.nn.Linear(d_model, d_model)

    def forward(self, Q, K, V, mask=None):
        B, S, _ = Q.shape
        # Project and split into heads
        Q = self.W_q(Q).view(B, S, self.num_heads, self.d_k).transpose(1,2)
        K = self.W_k(K).view(B, -1, self.num_heads, self.d_k).transpose(1,2)
        V = self.W_v(V).view(B, -1, self.num_heads, self.d_k).transpose(1,2)
        # Attention
        out, _ = scaled_dot_product(Q, K, V, mask)
        # Concatenate heads
        out = out.transpose(1,2).contiguous().view(B, S, -1)
        return self.W_o(out)
```

### Transformer Encoder Block
```python
class EncoderBlock(torch.nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout=0.1):
        super().__init__()
        self.attn = MultiHeadAttention(d_model, num_heads)
        self.ff = torch.nn.Sequential(
            torch.nn.Linear(d_model, d_ff), torch.nn.GELU(),
            torch.nn.Linear(d_ff, d_model)
        )
        self.norm1 = torch.nn.LayerNorm(d_model)
        self.norm2 = torch.nn.LayerNorm(d_model)
        self.drop = torch.nn.Dropout(dropout)

    def forward(self, x, mask=None):
        x = self.norm1(x + self.drop(self.attn(x, x, x, mask)))  # pre-norm variant
        x = self.norm2(x + self.drop(self.ff(x)))
        return x
```

---

## Practice (4h)

- Implement multi-head self-attention from scratch (no `nn.MultiheadAttention`)
- Implement a single Transformer encoder block
- Test on a synthetic sequence classification task (e.g., parity of binary sequence)

---

## Deliverable

`week13_attention_transformer_encoder.ipynb` — full MHA + encoder block from scratch + working classification test

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
