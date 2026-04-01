# Working with the API

**Course:** Claude with Google Cloud's Vertex AI
**Section:** Accessing Claude with the API
**Status:** ✅ Complete

---

## Objective

Understand how to access Claude models through Google Cloud's Vertex AI, make requests, manage conversations, and control model output.

---

## Critical Distinction

**Vertex AI API ≠ Anthropic API.** Same Claude models but different service, different SDK, different documentation. Always use Vertex AI-specific resources.

---

## Accessing the API

**API Access Flow:**
```
Client sends text → Developer server → Vertex AI SDK request
→ Claude tokenizes → embeds → contextualizes → generates
→ Response returned → Server forwards to client
```

**Security:** Always route API calls through a server — never expose credentials in client applications.

---

## Making a Request

**Setup:**
```python
%pip install "anthropic[vertex]"
from anthropic import vertex
# Instantiate with region="global" and project_id
```

**Required arguments for `client.messages.create()`:**
- `model` — specific Claude model version string
- `max_tokens` — safety limit on response length (not a target)
- `messages` — list of message dictionaries

**Message types:**
```python
{"role": "user", "content": "text"}
{"role": "assistant", "content": "response"}
```

**Response access:** `message.content[0].text`

**Key detail:** Use `project_id` from Google Cloud console — required for Vertex AI authentication.

---

## Multi-Turn Conversations

**Key constraint:** Vertex AI / Claude API is stateless — no memory between requests.

**Solution:**
1. Manually maintain complete message list in code
2. Send entire conversation history with every request

**Helper functions:**
- `add_user_message(messages, text)`
- `add_assistant_message(messages, text)`
- `chat(messages)` — sends full history, returns response

---

## System Prompts

Passed as a plain string using the `system` keyword argument. Controls *how* Claude responds, not *what* it responds to.

**Technical consideration:** System parameter cannot be `None` — conditionally include in API call only when prompt exists.

---

## Temperature

Parameter (0–1) controlling randomness in token selection.

| Temperature | Behavior | Use Case |
|---|---|---|
| Near 0 | Deterministic | Data extraction, factual tasks |
| Near 1 (default) | Creative, varied | Brainstorming, creative writing |

---

## Response Streaming

**Problem:** Responses can take 10–30 seconds. Users expect immediate feedback.

**Solution:** Stream events containing text chunks as they're generated.

**Key event types:**
- `content_block_delta` — contains actual text chunks
- `message_stop` — generation complete

```python
# Text-focused streaming
client.messages.stream()  # use stream.text_stream
stream.get_final_message()  # assemble for storage
```

---

## Controlling Model Output

**Pre-filling:** Add assistant message at end of message list — Claude continues from that exact point.

**Stop sequences:** Force generation to halt when a specific string appears. The sequence itself is excluded from output.

---

## Structured Data

Combine pre-filling + stop sequences to extract raw structured output.

```
Assistant prefill: ```json
Stop sequence:    ```
Result:           Raw JSON only — no commentary
```

Works for any structured format: JSON, Python, regex, lists.

---

## Key Takeaways

- Vertex AI uses the `anthropic[vertex]` SDK — not the standard Anthropic SDK
- Authentication uses Google Cloud `project_id` — not an API key
- All other patterns (stateless API, pre-fill, stop sequences) are identical to the Anthropic API
- `region="global"` recommended for model availability

---

## Personal Notes

- The Vertex AI SDK wraps the same Claude models — mental models transfer directly from the Anthropic API course
- Google Cloud authentication (`project_id`) is the key difference from Bedrock (boto3 + region) and Anthropic direct (API key)
- The `region="global"` setting is the Vertex AI equivalent of Bedrock inference profiles — handles model routing automatically

---

## Follow-Up Questions

- Does Vertex AI support the same inference profile concept as Bedrock for model routing?
- How does Google Cloud IAM interact with Vertex AI Claude access — is there a service account pattern?
- Can Vertex AI Claude be accessed from within Google Colab without additional auth setup?
