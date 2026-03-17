# Multi-Turn Conversations & System Prompts

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Understand how to maintain conversation context across multiple exchanges and how to customize Claude's behavior with system prompts.

---

## Multi-Turn Conversations

### The Core Limitation

The Anthropic API stores **no messages**. Every request is independent with no memory of previous exchanges.

### The Solution

1. Manually maintain a message list in code
2. Send the **entire conversation history** with every follow-up request

### Conversation Flow

```
Send initial user message
→ Receive assistant response
→ Append assistant response to message history
→ Add new user message to history
→ Send complete history for context-aware follow-up
```

### Helper Functions

- `add_user_message(messages, text)` — appends user message to history
- `add_assistant_message(messages, text)` — appends assistant response to history
- `chat(messages)` — sends full message history to API, returns response

> Without history: responses lack context. With history: Claude maintains continuity.

---

## System Prompts

### What They Are

A technique to customize Claude's response style and behavior by assigning it a specific role — passed as a plain string using the `system` keyword argument.

### Purpose

Control **how** Claude responds, not **what** it responds to. Same question gets different treatment based on assigned role.

### Example

```python
system="You are a patient math tutor. Give hints and guidance rather than direct answers."
```

### Implementation Pattern

```python
params = {"model": model, "max_tokens": 1000, "messages": messages}
if system_prompt:
    params["system"] = system_prompt
client.messages.create(**params)
```

> Handle the `None` case by excluding the `system` parameter entirely rather than passing `None`.

---

## Key Takeaways

- No server-side memory — you own the conversation history
- Send the full history every time — there are no shortcuts
- System prompts control behavior, not content
- Always handle the case where no system prompt is provided

---

## Personal Notes

- The stateless API design is intentional — it puts full control of context in the developer's hands, which is the right pattern for enterprise use
- System prompts are the equivalent of role definitions in a data pipeline — set the context once, apply everywhere

---

## Follow-Up Questions

- Is there a practical limit to how long a conversation history can get before performance degrades?
- Can system prompts reference uploaded files or project knowledge?
