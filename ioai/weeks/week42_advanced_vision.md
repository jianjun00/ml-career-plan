# Week 42 — Advanced Computer Vision: ViT, DINO, SAM
## Phase 5: Peak Performance Maintenance (Week 42 of 48)

---

## Theory (4h)

- **ViT in depth**: patch tokenization, class token classification, position embedding ablations
- **DINO / DINOv2**: self-supervised ViT training, emergent segmentation properties
- **SAM (Segment Anything Model)**: promptable segmentation, architecture

---

## DINOv2 Features for Zero-Shot Tasks
```python
import torch
from transformers import AutoImageProcessor, AutoModel
from PIL import Image

processor = AutoImageProcessor.from_pretrained('facebook/dinov2-base')
model = AutoModel.from_pretrained('facebook/dinov2-base')

def get_features(image_path):
    image = Image.open(image_path).convert('RGB')
    inputs = processor(images=image, return_tensors='pt')
    with torch.no_grad():
        outputs = model(**inputs)
    # CLS token = global image feature
    return outputs.last_hidden_state[:, 0, :]  # (1, 768)

# Zero-shot retrieval: compare features with cosine similarity
# Often beats fine-tuned models on small datasets
```

## SAM for Interactive Segmentation
```python
from segment_anything import SamPredictor, sam_model_registry

sam = sam_model_registry["vit_h"](checkpoint="sam_vit_h.pth")
predictor = SamPredictor(sam)

image = cv2.imread("image.jpg")
predictor.set_image(image)

# Point prompt: click on object
input_point = np.array([[500, 375]])  # (x, y)
input_label = np.array([1])           # 1 = foreground
masks, scores, _ = predictor.predict(point_coords=input_point, point_labels=input_label)
# masks[0] = best segmentation mask
```

## ViT Fine-tuning Strategy
```python
from transformers import ViTForImageClassification, ViTImageProcessor

processor = ViTImageProcessor.from_pretrained('google/vit-base-patch16-224')
model = ViTForImageClassification.from_pretrained(
    'google/vit-base-patch16-224',
    num_labels=num_classes,
    ignore_mismatched_sizes=True  # replace classification head
)

# Gradual unfreezing:
# Epoch 1-3: only train the classifier head
for name, param in model.named_parameters():
    if 'classifier' not in name:
        param.requires_grad = False
# Epoch 4-10: unfreeze last 4 transformer blocks + classifier
```

---

## Practice (8h)

- Use DINOv2 features for zero-shot image classification; compare with CLIP on same task
- Use SAM for interactive segmentation on a custom dataset
- Build a ViT fine-tuning pipeline with gradual unfreezing

---

## Deliverable

`week42_advanced_vision.ipynb` — DINOv2 retrieval + SAM segmentation + ViT fine-tuning comparison

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 5: Peak Performance Maintenance*
