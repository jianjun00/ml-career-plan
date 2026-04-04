# Week 41 — Cutting-Edge Papers (What Gold Medalists Know)
## Phase 5: Peak Performance Maintenance (Week 41 of 48)

---

## Theory (8h)

Read and understand these papers that directly appear in IOAI problems. For each: read abstract + intro + method + key equations. Skip proofs unless you have time.

---

## Required Papers

| Paper | URL | Why It Matters | Key Equation/Idea |
|-------|-----|----------------|-------------------|
| Attention Is All You Need | https://arxiv.org/abs/1706.03762 | Core transformer | Attn(Q,K,V) = softmax(QKᵀ/√d)V |
| BERT | https://arxiv.org/abs/1810.04805 | Pre-training paradigm | Masked LM + NSP |
| ViT: An Image is Worth 16x16 Words | https://arxiv.org/abs/2010.11929 | Vision transformers | Patch embedding |
| CLIP | https://arxiv.org/abs/2103.00020 | Multimodal contrastive | InfoNCE loss |
| UNet | https://arxiv.org/abs/1505.04597 | Segmentation and counting | Skip connections |
| ResNet | https://arxiv.org/abs/1512.03385 | Residual learning | F(x) + x |
| DDPM (Diffusion) | https://arxiv.org/abs/2006.11239 | Generative models | Reverse denoising |
| XGBoost | https://arxiv.org/abs/1603.02754 | Gradient boosting theory | Second-order Taylor approx |

---

## For Each Paper: Write a 3-Sentence Summary

Template:
1. **What problem does this paper solve?**
2. **What is the core idea/mechanism?**
3. **What is the key result/metric?**

---

## Practice (4h)

For each paper, implement the key equation:
- Attention Is All You Need → scaled dot-product attention (already done in Week 13)
- CLIP → InfoNCE contrastive loss:
```python
def clip_loss(image_features, text_features, temperature=0.07):
    image_features = F.normalize(image_features, dim=-1)
    text_features = F.normalize(text_features, dim=-1)
    logits = image_features @ text_features.T / temperature
    labels = torch.arange(len(logits)).to(logits.device)
    return (F.cross_entropy(logits, labels) + F.cross_entropy(logits.T, labels)) / 2
```
- XGBoost → second-order boosting objective:
  `Obj = Σ[gᵢfₜ(xᵢ) + ½hᵢfₜ(xᵢ)²] + Ω(fₜ)` where g=first derivative, h=second derivative of loss

---

## Deliverable

`week41_papers_summary.md` — 8 three-sentence summaries + key equation implementations

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 5: Peak Performance Maintenance*
