# Working with the API

**Course:** Claude with Amazon Bedrock
**Section:** Working with the API
**Status:** ✅ Complete

---

## Objective

Understand how to access Claude models through AWS Bedrock, make requests, manage multi-turn conversations, and control model output.

---

## Critical Distinction

**AWS Bedrock API ≠ Anthropic API.** Same Claude models (Sonnet, Haiku) but different services, different SDKs, different documentation. Always use AWS Bedrock-specific resources.

---

## Accessing the API

**Chat bot flow:**
```
User submits text → Server receives request → Bedrock client makes API call
→ Request includes user message + model ID → Model generates text
→ Returns assistant message → Server sends response to browser
```

---

## Making a Request

**Three requirements:**
1. **Boto3 client** — connects to Bedrock runtime service (specify region, e.g., US West 2)
2. **Model ID** — identifier for the specific model
3. **User message** — specially formatted object containing input text

**Model ID complexity:**
- Not all models hosted in every region — wrong region = cryptic errors
- **Inference profiles** = route requests to regions where the model actually exists
- Use inference profile ID instead of direct model ID for guaranteed routing
- Found in AWS console under "cross region inference" — not "model catalog"

**Message structure:**
```python
user_message = {"role": "user", "content": [{"text": "your input"}]}
```
Content is a list because messages can contain multiple parts (text + images).

**Making the request:**
```python
response = client.converse(modelId="profile_id", messages=[user_message])
text = response["output"]["message"]["content"][0]["text"]
```

---

## Multi-Turn Conversations

**Key constraint:** Bedrock/Claude APIs are stateless — store no messages or responses.

**Requirements:**
1. Manually maintain a complete message list in your code
2. Send entire conversation history with every follow-up request

**Message structure rules:**
- Messages = list of alternating user/assistant roles
- Never have consecutive messages with the same role
- Pattern must be: user → assistant → user → assistant

**Without history:** follow-up questions produce nonsensical responses. With history: Claude maintains full context.

---

## System Prompts

Assign a role to guide Claude's responses — passed using the `system` keyword parameter.

```python
system="You are an AWS cloud support specialist"
```

**Technical requirement:** System prompt must have at least 1 character; passed as a list with a dictionary containing a text field.

**Best practice:** Make system prompt parameter optional in chat functions, defaulting to `None` to avoid empty string errors.

---

## Temperature

Parameter (0–1) controlling randomness in token selection.

| Temperature | Behavior | Use Case |
|---|---|---|
| Near 0 | Deterministic — always picks highest probability token | Data extraction, factual tasks |
| Near 1 (default) | More varied, creative outputs | Creative writing, brainstorming |

**Implementation:** Pass `temperature` in `inference_config` when calling the converse function.

---

## Streaming

**Problem:** Responses can take 3–30 seconds. Users expect immediate feedback.

**Solution:** `converse_stream` function — streams generated text as it's produced.

**Event types:**
| Event | Purpose |
|---|---|
| Message start | First event, no text |
| Content block delta | **Actual text chunks** — most important |
| Content block stop | Generation complete |
| Message stop | Final event |
| Metadata | Last event |

```python
for event in response['stream']:
    if 'content_block_delta' in event:
        chunk = event['content_block_delta']['delta']['text']
        total_text += chunk
```

---

## Controlling Model Output

**Pre-filling assistant messages:** Manually add an assistant message at the end of the message list to steer response direction. Claude continues from the exact endpoint of the pre-fill — must concatenate pre-fill + response for complete output.

**Stop sequences:** Force Claude to stop generation when a specific string is encountered. The stopping sequence itself is excluded from the final response.

---

## Structured Data

Combine stop sequences + assistant message prefilling to extract clean structured output without extra commentary.

```
User message:      Request for structured data
Assistant prefill: Opening delimiter (e.g., ```json)
Stop sequence:     Closing delimiter (e.g., ```)
```

Works for any structured format — JSON, Python, regex, lists.

---

## Key Takeaways

- Always use inference profiles, not direct model IDs — avoids region routing errors
- The Bedrock API is stateless — you own the full conversation history
- Streaming is essential for production user-facing applications
- Pre-fill + stop sequences = the cleanest path to raw structured output

---

## Personal Notes

- The inference profile requirement is a Bedrock-specific gotcha that doesn't exist in the Anthropic API — worth documenting clearly for any client work on AWS
- The boto3 client + region configuration is the standard AWS SDK pattern I already use — familiar territory
- Bedrock's `converse` API mirrors the Anthropic Messages API closely enough that the mental models transfer directly

---

## Follow-Up Questions

- How do inference profiles affect latency compared to direct model IDs?
- Is there a Bedrock-specific rate limit separate from the underlying Claude model limits?
- Can Bedrock's converse API use the same tool schemas as the Anthropic API, or do they need to be reformatted?
