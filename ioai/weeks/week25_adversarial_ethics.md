# Week 25 — AI Ethics, Fairness, Robustness, and Adversarial ML
## Phase 3: Advanced Topics + Competition Hardening (Week 25 of 30)

**IOAI relevance**: IOAI 2024 on-site ML task involved adversarial attacks and model quantization.

---

## Theory (6h)

- **Bias types**: historical bias, representation bias, measurement bias
- **Fairness metrics**: demographic parity, equalized odds, calibration — conflicts between them
- **Adversarial attacks**: FGSM, PGD, C&W — how they fool classifiers
- **Adversarial defense**: adversarial training, input preprocessing, certified defenses
- **Model quantization**: int8, int4, post-training quantization vs. QAT

---

## FGSM Attack (Fast Gradient Sign Method)
```python
def fgsm_attack(model, X, y, epsilon=0.03):
    X.requires_grad = True
    loss = F.cross_entropy(model(X), y)
    loss.backward()
    # Perturb in direction of gradient sign
    X_adv = X + epsilon * X.grad.sign()
    return torch.clamp(X_adv, 0, 1)

# Measure accuracy degradation
acc_clean = evaluate(model, clean_loader)
acc_adv = evaluate(model, adv_loader)
print(f"Clean: {acc_clean:.3f}, Adversarial: {acc_adv:.3f}")
```

## PGD Attack (Projected Gradient Descent — stronger)
```python
def pgd_attack(model, X, y, epsilon=0.03, alpha=0.007, steps=40):
    X_adv = X.clone().detach() + torch.randn_like(X) * epsilon
    for _ in range(steps):
        X_adv.requires_grad_(True)
        loss = F.cross_entropy(model(X_adv), y)
        grad = torch.autograd.grad(loss, X_adv)[0]
        X_adv = X_adv.detach() + alpha * grad.sign()
        # Project back into epsilon-ball
        delta = torch.clamp(X_adv - X, -epsilon, epsilon)
        X_adv = torch.clamp(X + delta, 0, 1)
    return X_adv
```

## Model Quantization
```python
import torch.quantization

# Post-training static quantization
model.eval()
model.qconfig = torch.quantization.get_default_qconfig('fbgemm')
torch.quantization.prepare(model, inplace=True)
# Calibrate with representative data
with torch.no_grad():
    for X, _ in calibration_loader: model(X)
torch.quantization.convert(model, inplace=True)
# Model is now int8; ~4x smaller, ~2x faster inference
```

---

## Practice (6h)

- Implement FGSM attack on a trained image classifier; measure accuracy degradation
- Implement adversarial training (fine-tune model on adversarial examples)
- Practice [IOAI 2024 on-site ML adversarial/quantization task](https://github.com/IOAI-official/IOAI-2024)

---

## Deliverable

`week25_adversarial_ethics.ipynb` — FGSM/PGD attacks + adversarial training + quantization demo

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
