# Week 22 — Advanced NLP: LLMs, Prompting, Fine-Tuning
## Phase 3: Advanced Topics + Competition Hardening (Week 22 of 30)

---

## Theory (4h)

- **LLM capabilities and limitations**: in-context learning, hallucination, factuality
- **Prompting strategies**: zero-shot, few-shot, chain-of-thought, role prompting
- **Parameter-efficient fine-tuning**: LoRA, prefix tuning, adapter layers
- **RAG** (Retrieval-Augmented Generation): conceptual understanding

---

## LoRA Fine-tuning with PEFT
```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import LoraConfig, get_peft_model, TaskType

model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-3.2-1B")
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-3.2-1B")

# LoRA adds rank-r adapters to attention layers only
# Trainable params: r*(d_in + d_out) << d_in*d_out full fine-tune
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=16,               # rank (smaller = fewer params but less capacity)
    lora_alpha=32,      # scaling factor
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.1,
)
model = get_peft_model(model, lora_config)
model.print_trainable_parameters()  # ~0.1% of total params
```

## Prompting Strategies
```python
# Zero-shot
prompt = "Classify the sentiment of: 'This product is amazing!'\nSentiment:"

# Few-shot
prompt = """Classify sentiment:
Text: "Terrible experience." → Negative
Text: "Great product!" → Positive
Text: "It's okay I guess." → Neutral
Text: "This product is amazing!" → """

# Chain-of-thought
prompt = """Is the following news AI-generated?
Text: [article]
Think step by step: check vocabulary diversity, sentence structure regularity, ...
Answer:"""
```

## OpenRouter API for IOAI (token-limited assistant)
```python
import openai

client = openai.OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key="YOUR_OPENROUTER_KEY"
)
response = client.chat.completions.create(
    model="openai/gpt-4o-mini",
    messages=[{"role": "user", "content": "Explain how to use BERT for text classification"}],
    max_tokens=500
)
# At IOAI, you have ~10 such queries per session. Use them wisely.
```

---

## Practice (8h)

- Fine-tune GPT-2 or LLaMA-3-1B on a domain-specific task using LoRA
- Build a prompting system for the IOAI 2025 "Concepts" NLP task
- Implement AI-generated text detection using perplexity scoring + classifier ensemble

---

## Deliverable

`week22_llm_finetuning.ipynb` — LoRA fine-tune + prompting comparison + AI text detection

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
