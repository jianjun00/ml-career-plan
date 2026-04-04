# Week 24 — Multimodal ML: Vision + Language
## Phase 3: Advanced Topics + Competition Hardening (Week 24 of 30)

**IOAI relevance**: IOAI 2024 "Lab Repository" required multimodal fusion (image + text).

---

## Theory (4h)

- **CLIP**: contrastive image-text pretraining, zero-shot transfer
- **BLIP, LLaVA**: vision-language instruction tuning
- **Multimodal fusion strategies**: early fusion, late fusion, cross-attention

---

## CLIP Zero-Shot Classification
```python
import clip
import torch

model, preprocess = clip.load("ViT-B/32", device="cuda")

# Zero-shot: no training examples needed
image = preprocess(Image.open("image.jpg")).unsqueeze(0).to("cuda")
text_labels = ["a photo of a cat", "a photo of a dog", "a photo of a car"]
text = clip.tokenize(text_labels).to("cuda")

with torch.no_grad():
    image_features = model.encode_image(image)
    text_features = model.encode_text(text)
    # Cosine similarity
    similarity = (image_features @ text_features.T).softmax(dim=-1)

print(f"Predicted: {text_labels[similarity.argmax()]}")
```

## Late Fusion (image + tabular)
```python
class MultimodalModel(nn.Module):
    def __init__(self, img_feat_dim, tab_feat_dim, num_classes):
        super().__init__()
        self.img_enc = models.resnet18(pretrained=True)
        self.img_enc.fc = nn.Identity()          # remove classification head
        self.tab_enc = nn.Sequential(
            nn.Linear(tab_feat_dim, 128), nn.ReLU(), nn.Linear(128, 64)
        )
        self.classifier = nn.Linear(img_feat_dim + 64, num_classes)

    def forward(self, img, tab):
        img_feat = self.img_enc(img)             # (B, 512)
        tab_feat = self.tab_enc(tab)             # (B, 64)
        combined = torch.cat([img_feat, tab_feat], dim=1)
        return self.classifier(combined)
```

## HuggingFace BLIP for image captioning
```python
from transformers import BlipProcessor, BlipForConditionalGeneration

processor = BlipProcessor.from_pretrained("Salesforce/blip-image-captioning-base")
model = BlipForConditionalGeneration.from_pretrained("Salesforce/blip-image-captioning-base")

inputs = processor(image, return_tensors="pt")
caption_ids = model.generate(**inputs)
caption = processor.decode(caption_ids[0], skip_special_tokens=True)
```

---

## Practice (8h)

- Use CLIP for zero-shot classification; compare with fine-tuned ResNet on same task
- Build a multimodal classifier (image + tabular metadata): fuse CNN features with MLP features
- Practice IOAI 2024 CV task with multimodal fusion component

---

## Deliverable

`week24_multimodal.ipynb` — CLIP zero-shot + late fusion model + IOAI task attempt

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
