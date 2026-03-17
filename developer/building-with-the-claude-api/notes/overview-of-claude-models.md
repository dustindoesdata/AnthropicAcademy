# Overview of Claude Models

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Understand the Claude model families, their trade-offs, and how to select the right model for a given task.

---

## Key Concepts

### The Three Model Families

| Model | Priority | Best For | Trade-offs |
|---|---|---|---|
| **Opus** | Highest intelligence | Complex, multi-step tasks requiring deep reasoning and planning | Higher cost and latency |
| **Sonnet** | Balanced | Most practical use cases; strong coding and precise code editing | — |
| **Haiku** | Speed and cost efficiency | Real-time user interactions, high-volume processing | No reasoning capabilities |

### Selection Framework

- **Intelligence priority** → Opus
- **Speed priority** → Haiku
- **Balanced requirements** → Sonnet

### Multi-Model Architecture

A common and recommended approach is using **multiple models in the same application** based on task requirements rather than committing to a single model. All models share core capabilities (text generation, coding, image analysis) — the main difference is optimization focus.

---

## Key Takeaways

- Sonnet is the default choice for most practical use cases
- Haiku sacrifices reasoning for speed — don't use it where deep reasoning matters
- Mixing models per task is a legitimate production pattern, not a workaround

---

## Personal Notes

- Multi-model architecture maps to how I think about tool selection in data pipelines — use the right tool for the right step, not one tool for everything
- Haiku for high-volume batch processing + Sonnet for analysis + Opus for complex reasoning = a cost-effective enterprise pattern

---

## Follow-Up Questions

- How do latency and cost differences scale between Opus and Sonnet in production workloads?
- Are there tasks where Haiku outperforms Sonnet despite the reasoning gap?
