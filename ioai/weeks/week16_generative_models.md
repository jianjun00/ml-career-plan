# Week 16 — Generative Models: GANs and Diffusion
## Phase 2: Deep Learning Core (Week 16 of 18)

---

## Theory (4h)

- **GANs**: minimax game, training instability, mode collapse, Wasserstein GAN
- **Diffusion models**: forward noising process, reverse denoising, DDPM, DDIM sampling
- **Practical uses**: image generation, image editing, inpainting
- **IOAI relevance**: IOAI 2024 team challenge used diffusion-based image generation tools

---

## GAN Core Concept
```python
# Minimax: G minimizes, D maximizes:
# min_G max_D E[log D(x)] + E[log(1 - D(G(z)))]

class Generator(nn.Module):
    def __init__(self, z_dim=100, img_dim=784):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(z_dim, 256), nn.LeakyReLU(0.2),
            nn.Linear(256, 512), nn.LeakyReLU(0.2),
            nn.Linear(512, img_dim), nn.Tanh()
        )
    def forward(self, z): return self.net(z)

# Training loop alternates:
# 1. Update D: max log D(real) + log(1 - D(G(z)))
# 2. Update G: min log(1 - D(G(z))) ≡ max log D(G(z))
```

## Diffusion model intuition
```
Forward process: x_t = √α_t x_{t-1} + √(1-α_t) ε   (gradually add noise)
Reverse process: learn to predict ε from x_t → denoise step by step
DDPM: 1000 steps; DDIM: 50 steps (same quality, deterministic)

Key: model learns ε_θ(x_t, t) — the noise added at step t
Loss: L = ||ε - ε_θ(x_t, t)||²
```

## Diffusion inference with HuggingFace diffusers
```python
from diffusers import StableDiffusionPipeline
import torch

pipe = StableDiffusionPipeline.from_pretrained("stabilityai/stable-diffusion-2-1",
                                                torch_dtype=torch.float16).to("cuda")
image = pipe("a photo of a hog in the style of Van Gogh", num_inference_steps=50).images[0]
# ControlNet for conditional generation (structure-guided):
from diffusers import ControlNetModel, StableDiffusionControlNetPipeline
```

---

## Practice (8h)

- Train a small GAN on MNIST (aim for recognizable digits)
- Use a pretrained diffusion model (Stable Diffusion via diffusers) for controlled generation
- Practice IOAI 2024 Team Challenge "Hogspell": steer a diffusion model to generate hog images matching a target style

**Resource**: [IOAI 2025 GitHub — Team Challenge tasks](https://github.com/IOAI-official/IOAI-2025)

---

## Deliverable

`week16_generative_models.ipynb` — GAN on MNIST + diffusion inference + team challenge approach

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
