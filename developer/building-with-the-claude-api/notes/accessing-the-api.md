# Accessing the API

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Understand the full request-response flow from client to Anthropic API, including how text generation works under the hood.

---

## Key Concepts

### API Access Flow (5 Steps)

1. **Client → Server** — User text is sent to the developer's server. Never access the Anthropic API directly from a client app — keeps the API key secret.
2. **Server → Anthropic API** — Server makes the request using an SDK (Python, TypeScript, JavaScript, Go, Ruby) or plain HTTP. Required parameters: API key, model name, messages list, max_tokens limit.
3. **Text Generation (4 stages):**
   - **Tokenization** — Input broken into tokens (words, word parts, symbols, spaces)
   - **Embedding** — Tokens converted to number lists representing all possible word meanings
   - **Contextualization** — Embeddings adjusted based on neighboring tokens to determine precise meaning
   - **Generation** — Output layer produces probabilities for next word; model selects using probability + randomness; repeats
4. **Stopping** — Model stops when max_tokens is reached or a special end_of_sequence token is generated
5. **Response** — API returns generated text + usage counts + stop_reason to server; server sends to client

### Key Terms

| Term | Definition |
|---|---|
| Token | Text chunk (word, part, symbol) |
| Embedding | Numerical representation of word meanings |
| Contextualization | Meaning refinement using neighboring words |
| max_tokens | Generation length limit |
| stop_reason | Why the model stopped generating |

---

## Key Takeaways

- Client apps should never hold the API key — always proxy through a server
- Text generation is probabilistic, not deterministic — the model selects from a probability distribution each step
- max_tokens is a safety cap, not a target length

---

## Personal Notes

- The tokenization → embedding → contextualization → generation pipeline mirrors how I think about NLP feature engineering — the mechanics are the same, just abstracted
- The "never expose API key on client" rule is the same principle as never committing credentials to source control

---

## Follow-Up Questions

- How does the tokenization differ between Claude models — is the vocabulary the same across Opus/Sonnet/Haiku?
- What's the relationship between max_tokens and response quality — does hitting the limit degrade output?
