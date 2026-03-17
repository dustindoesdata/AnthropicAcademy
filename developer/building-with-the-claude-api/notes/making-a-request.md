# Making a Request

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Learn the setup steps and request structure for making API calls to Claude in Python.

---

## Key Concepts

### Setup Steps

1. Install packages: `pip install anthropic python-dotenv`
2. Store API key in `.env`: `ANTHROPIC_API_KEY="your_key"` — add to `.gitignore`
3. Load environment variable using `python-dotenv`
4. Create client and define model variable

### API Request Structure

```python
client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[{"role": "user", "content": "What is quantum computing?"}]
)
```

**Required arguments:** `model`, `max_tokens`, `messages`

### Message Types

| Type | Structure |
|---|---|
| User message | `{"role": "user", "content": "your text"}` |
| Assistant message | Contains model-generated responses |

### Accessing the Response

- Full response: contains metadata and nested structure
- Text only: `message.content[0].text`

---

## Key Takeaways

- `max_tokens` is a safety limit, not a target length
- Always store API keys in `.env` — never hardcode them
- `message.content[0].text` is the fastest path to the generated text

---

## Personal Notes

- The `.env` + `.gitignore` pattern is non-negotiable for any project — same principle as secrets management in Azure Key Vault
- `python-dotenv` is lightweight and sufficient for local dev; production should use a proper secrets manager

---

## Follow-Up Questions

- What happens if `max_tokens` is set too low — does Claude truncate mid-sentence?
- Is there a minimum `max_tokens` value?
