# Temperature, Streaming & Output Control

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Learn how to control randomness in Claude's output, stream responses in real time, and steer generation using prefilling and stop sequences.

---

## Temperature

### What It Is

A parameter (0–1) that controls randomness in token selection by influencing probability distributions.

### Effects

| Temperature | Behavior | Use Case |
|---|---|---|
| Near 0 | Deterministic — always picks highest probability token | Data extraction, factual tasks requiring consistency |
| Near 1 | Higher chance of lower-probability tokens | Creative tasks: brainstorming, writing, marketing |

> Higher temperature doesn't guarantee different outputs — it increases the *probability* of variation.

---

## Response Streaming

### The Problem

AI responses can take 10–30 seconds. Users expect immediate feedback.

### How It Works

1. Server sends user message to Claude
2. Claude immediately sends acknowledgment (no text yet)
3. Stream of events follows, each containing text chunks
4. Server forwards chunks to frontend for real-time display

### Event Types

| Event | Purpose |
|---|---|
| `message_start` | Initial acknowledgment |
| `content_block_start` | Text generation begins |
| `content_block_delta` | **Actual text chunks** (most important) |
| `content_block_stop` / `message_stop` | Generation complete |

### Implementation

```python
# Basic
client.messages.create(stream=True)  # returns event iterator

# Simplified
client.messages.stream()  # use .text_stream for text only

# Capture full message
stream.get_final_message()  # assembles all chunks for storage
```

---

## Controlling Output: Prefilling & Stop Sequences

### Pre-filling Assistant Messages

Manually add an assistant message at the end of the conversation to steer response direction. Claude continues from the exact endpoint of the pre-fill.

```python
messages = [
    {"role": "user", "content": "Which is better, coffee or tea?"},
    {"role": "assistant", "content": "Coffee is better because"}
]
```

> Must stitch together pre-fill + generated response in final output.

### Stop Sequences

Force Claude to halt generation when a specific string appears. The stop sequence itself is **not included** in the final output.

```python
# Stops before "five" is included
stop_sequences=["five"]

# Cleaner output — stops before ", five"
stop_sequences=[", five"]
```

---

## Key Takeaways

- Temperature 0 = consistency; Temperature 1 = creativity — match to use case
- Streaming dramatically improves perceived performance for end users
- Pre-filling steers direction; stop sequences control length and delimiters
- Always capture the final assembled message for storage, not just the stream

---

## Personal Notes

- Streaming is table stakes for any user-facing application — a spinner for 30 seconds is not acceptable UX
- Pre-filling + stop sequences together are the key to clean structured output — this is the pattern behind JSON extraction
- Temperature 0 is the right default for anything going into a report or client deliverable

---

## Follow-Up Questions

- Does streaming affect token costs or only latency?
- Can pre-filling be combined with system prompts?
- What happens if a stop sequence never appears — does Claude just run to max_tokens?
