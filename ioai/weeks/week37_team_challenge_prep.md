# Week 37 — Team Challenge Preparation
## Phase 4: Pre-Competition Peak (Week 37 of 40)

---

## Overview

IOAI team challenges change format annually. You cannot predict the exact format, but you can prepare for the most likely categories based on history.

---

## Historical Formats

| Year | Team Challenge Format |
|------|----------------------|
| IOAI 2024 | "Hogspell" — steer a diffusion model to generate hog images matching a target style |
| IOAI 2025 | Robot arm simulation (Galbot) — pick-and-place task in simulated environment |

---

## Preparation Track 1: Diffusion/Image Generation (2024 format)

**Relevant if format is creative AI / generative:**
```python
from diffusers import StableDiffusionPipeline, ControlNetModel, StableDiffusionControlNetPipeline

# ControlNet for structure-guided generation
controlnet = ControlNetModel.from_pretrained("lllyasviel/sd-controlnet-canny")
pipe = StableDiffusionControlNetPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5", controlnet=controlnet)

# IP-Adapter for style transfer
# CFG (Classifier-Free Guidance): higher CFG = more prompt adherence
image = pipe(
    prompt="a detailed photo of a hog, Studio Ghibli style",
    negative_prompt="ugly, blurry, low quality",
    guidance_scale=7.5,
    num_inference_steps=50,
    image=canny_image
).images[0]
```

**Practice**: Recreate IOAI 2024 "Hogspell" challenge from [IOAI 2024 GitHub](https://github.com/IOAI-official/IOAI-2024).

---

## Preparation Track 2: Robotics/Simulation (2025 format)

**Relevant if format is robot control / simulation:**
```python
import pybullet as p
import pybullet_data

physicsClient = p.connect(p.GUI)
p.setAdditionalSearchPath(pybullet_data.getDataPath())
p.setGravity(0, 0, -10)

planeId = p.loadURDF("plane.urdf")
robotId = p.loadURDF("kuka_iiwa/model.urdf", [0,0,0], useFixedBase=True)

# Control: set joint angles
for joint_idx, angle in enumerate(target_angles):
    p.setJointMotorControl2(robotId, joint_idx,
                            p.POSITION_CONTROL, targetPosition=angle)
p.stepSimulation()
```

**Practice**: Solve a pick-and-place task in PyBullet.

---

## Preparation Track 3: 2026 Unknown Format

Likely multimodal or agentic:
- LLM tool use: agents that call APIs, read files, generate code
- Practice: [LangChain agents tutorial](https://python.langchain.com/docs/modules/agents/)
- Practice: ReAct-style prompting (Reason + Act)

---

## Practice (8h)

- Complete one full diffusion-based generation task with ControlNet
- Solve a simple pick-and-place task in PyBullet simulation
- Write a 1-page strategy document: how would you approach each format?

---

## Deliverable

`week37_team_challenge_prep.md` — strategy doc for each format + at least one working demo

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 4: Pre-Competition Peak*
