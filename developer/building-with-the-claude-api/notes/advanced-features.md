# Advanced Features

**Course:** Building with the Claude API
**Section:** Advanced Features
**Status:** ✅ Complete

---

## Objective

Understand extended thinking, image and PDF support, citations, prompt caching, and the code execution + Files API.

---

## Extended Thinking

Allows Claude to reason before generating its final response.

**Key mechanics:**
- Displays a separate thinking process visible to users
- Increases accuracy for complex tasks but adds cost (thinking tokens are charged) and latency
- **Thinking budget:** minimum 1024 tokens
- `max_tokens` must exceed thinking budget (e.g., budget 1024 → `max_tokens` ≥ 1025)

**Response structure:**
- **Thinking block** — contains reasoning text + cryptographic signature
- **Text block** — final response
- **Signature** — prevents tampering with thinking text (safety measure)

**Special cases:**
- **Redacted thinking blocks** — encrypted thinking text flagged by safety systems; provided for conversation continuity

**When to use:** After prompt optimization fails to achieve desired accuracy. Use eval pipelines to determine if it's actually needed.

---

## Image Support

Claude can process images within user messages for analysis, comparison, counting, and description.

**Limitations:**
- Max 100 images per request
- Size/dimension restrictions apply
- Images consume tokens (charged based on pixel height/width)

**Image block structure:** Holds either raw base64 image data or a URL reference.

**Critical success factor:** Strong prompting is required for accurate results — simple prompts often fail.

**Prompting techniques for images:**
- Step-by-step analysis instructions
- One-shot/multi-shot examples (alternating image + text pairs)
- Clear guidelines and verification steps
- Structured analysis frameworks

---

## PDF Support

Claude can read PDF files directly using similar code to image processing.

**Key implementation differences from images:**

| Parameter | Images | PDFs |
|---|---|---|
| File type | `"image"` | `"document"` |
| Media type | `"image/png"` | `"application/pdf"` |

**Claude PDF capabilities:** Text + images + charts + tables + mixed content extraction — a one-stop solution for comprehensive document analysis.

---

## Citations

Allows Claude to reference source documents and show exactly where information comes from.

**Citation types:**
- `citation_page_location` — for PDFs (document index, title, start/end page, cited text)
- `citation_char_location` — for plain text (character position in text block)

**Implementation:**
```python
# In request
"citations": {"enabled": True}

# In document
"title": "Document Name"  # identifies the source
```

**Response structure:** Content becomes a list of text blocks, some containing citations arrays with location data.

**UI benefit:** Enables citation popups showing source document, page numbers, and exact cited text — users can verify Claude's sources directly.

---

## Prompt Caching

Speeds up responses and reduces costs by reusing computational work from previous requests.

**How it works:** Instead of discarding processing work after each request, Claude stores results in a temporary cache. Subsequent requests with identical content retrieve cached work rather than reprocessing.

**Rules:**
- Cache duration: **1 hour maximum**
- Requires manual cache breakpoint addition to message blocks
- Content must use longhand format (not shorthand string) to add cache control
- Cache scope: all content up to and including the breakpoint
- Any change before the breakpoint invalidates the entire cache
- Processing order: tools → system prompt → messages
- Maximum **4 breakpoints** per request
- Minimum **1024 tokens** required to cache

**Token usage in responses:**
- `cache_creation_input_tokens` — tokens written to cache on first use
- `cache_read_input_tokens` — tokens retrieved from cache on subsequent identical requests

**Best use cases:** Repeated identical content — system prompts, tool definitions, static message prefixes.

**Implementation:**
```python
# Tool schema caching — add to last tool in list
{"cache_control": {"type": "ephemeral"}}

# System prompt caching
{"type": "text", "text": system_prompt, "cache_control": {"type": "ephemeral"}}
```

---

## Code Execution & Files API

### Files API

Upload files ahead of time and reference them later via file ID.

```
Upload file → get file metadata with ID → use ID in future requests
```

Eliminates sending raw file data in every request.

### Code Execution

Server-based tool where Claude executes Python code in isolated Docker containers. No custom implementation needed — just include the predefined tool schema.

**Key constraints:**
- Docker containers have no network access
- Data input/output relies on Files API integration

**Combined workflow:**
1. Upload file via Files API → get file ID
2. Include ID in container upload block
3. Ask Claude to analyze
4. Claude writes and executes code with access to the file
5. Returns analysis and generated outputs (plots, reports) with downloadable file IDs

---

## Key Takeaways

- Extended thinking is a last resort after prompt optimization — not a first response to poor output
- Image accuracy depends entirely on prompt sophistication, not image quality
- PDFs are treated almost identically to images — minor parameter changes
- Citations are a transparency layer that builds user trust in Claude's responses
- Prompt caching has a 1-hour TTL — design cache breakpoints around stable, repeated content
- Code execution + Files API enables full data analysis workflows without a separate compute environment

---

## Personal Notes

- Prompt caching is immediately applicable for any application with a heavy system prompt or large static tool set — the cost savings compound quickly
- Citations are non-negotiable for any compliance-adjacent or research application — users need to trace Claude's reasoning to sources
- Code execution in Docker containers is essentially a serverless Python environment — the Files API is the I/O layer. This maps cleanly to how I'd design a Fabric notebook pipeline

---

## Follow-Up Questions

- Does prompt caching apply across different users or only within the same session/API key?
- Can code execution containers access the internet if needed, or is network isolation absolute?
- How does extended thinking interact with prompt caching — are thinking tokens cached?
