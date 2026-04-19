# Gemma 4 E4B → Opus 4.6 Reasoning Adapter

## What this is

I fine-tuned Gemma 4 E4B-it on Claude Opus 4.6 reasoning-style outputs.

Across multiple prompts, something interesting showed up:

The final answer often stayed the same.  
The path to the answer changed.

The model started adding small intermediate steps like  
“the age difference stays constant” before solving.

---

## Key Observation

This adapter doesn’t necessarily improve correctness.

It changes *how* the model reasons.

In many cases, it introduces simple scaffolding before solving, making the output feel closer to how a human would explain the problem.

This shows up most clearly in:
- logic puzzles
- algorithm explanations
- step-by-step reasoning tasks

---

## Example

### Prompt
A few years ago, Jimin was 16 and his father was 47. If the father is twice Jimin's age this year, how many years have passed?

### Base Gemma
Goes straight into forming the equation.

### With Adapter
First introduces:
> "the age difference stays constant"

Then proceeds to solve.

That one extra step makes the explanation noticeably more structured.

---

## Try it yourself

### Notebook (A/B comparison)
See the full comparison in the notebook.

### Model weights (Hugging Face)
https://huggingface.co/krishnamraja13/gemma-4-e4b-opus46-reasoning

---

## Quick Setup

```bash
pip install -U transformers peft accelerate bitsandbytes torch

--------

## Quick Usage

from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

base_model = AutoModelForCausalLM.from_pretrained(
    "google/gemma-4-e4b-it",
    device_map="auto"
)

model = PeftModel.from_pretrained(
    base_model,
    "krishnamraja13/gemma-4-e4b-opus46-reasoning"
)

tokenizer = AutoTokenizer.from_pretrained(
    "krishnamraja13/gemma-4-e4b-opus46-reasoning"
)

