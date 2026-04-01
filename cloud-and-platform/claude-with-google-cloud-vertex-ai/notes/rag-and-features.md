# RAG, Features of Claude & MCP

**Course:** Claude with Google Cloud's Vertex AI
**Section:** RAG, Features of Claude, MCP
**Status:** ✅ Complete

---

## Retrieval Augmented Generation (RAG)

### The Problem

Querying large documents (100–1000+ pages) hits token limits and degrades performance.

### The RAG Approach

1. Break document into small chunks
2. Find the most relevant chunks for each query
3. Include only those chunks in the prompt

### Text Chunking Strategies

| Strategy | Pros | Cons |
|---|---|---|
| **Size-based** | Easiest, most common | Cuts words, lacks context |
| **Structure-based** | Well-formed sections | Requires structured docs |
| **Semantic-based** | Most accurate | Most complex |

**Overlap strategy:** Include characters from neighboring chunks — preserves context at cost of duplication.

### Text Embeddings

**Vertex AI option:** Google's `text-embedding-005` model — available natively on the platform.

Outputs list of numbers (~1024 values, range –1 to +1) representing semantic meaning. Enables similarity search by meaning rather than keywords.

### The Full RAG Flow (7 Steps)

1. Text chunking
2. Generate embeddings
3. Normalization (auto-handled)
4. Vector database storage
5. Query processing (embed user question)
6. Similarity search (cosine similarity)
7. Prompt assembly + LLM generation

**Cosine distance:** 1 minus cosine similarity — closer to 0 = higher similarity.

### Hybrid Search (BM25 + Semantic)

BM25 weights rare terms higher, complementing semantic search for exact keyword matches.

**Reciprocal Rank Fusion:** `score = sum of (1 / (1 + rank))` across all search methods.

### Reranking

Post-processing LLM step to reorder results by relevance. Increases accuracy, adds latency.

### Contextual Retrieval

Add LLM-generated context to each chunk before embedding — preserves document relationships.

**Large document handling:** Include starter chunks + chunks immediately before target. Skip middle.

---

## Features of Claude

### Extended Thinking

- **Thinking budget:** minimum 1024 tokens; `max_tokens` must significantly exceed budget
- **Charged** for thinking tokens
- **When to use:** Only after prompt optimization fails — always eval-driven
- Response contains: thinking block (with cryptographic signature) + text block

### Image Support

- Up to 100 images per request
- Images consume tokens based on pixel dimensions
- **Critical:** Prompt engineering drives accuracy — simple prompts consistently fail
- Effective techniques: step-by-step analysis, one-shot/multi-shot examples

### PDF Support

Nearly identical to image processing:
- File type: `"document"` (not `"image"`)
- Media type: `"application/pdf"` (not `"image/png"`)

### Citations

```python
"citations": {"enabled": True}
```

Types: `citation_page_location` (PDF) and `citation_char_location` (plain text).

Enables UI overlays showing users exactly which source text supports each statement.

### Prompt Caching

**Vertex AI cache TTL: 5 minutes** (same as Bedrock, shorter than Anthropic direct at 1 hour).

**Requirements:**
- Minimum 1024 tokens before cache point
- Content must be exactly identical to hit cache
- Processing order: tools → system prompt → messages

**Token monitoring:**
- `cache_creation_input_tokens` — first use
- `cache_read_input_tokens` — subsequent identical requests

---

## Model Context Protocol (MCP)

### The Three Primitives

| Primitive | Controlled By | Purpose |
|---|---|---|
| **Tools** | Model (Claude decides) | Add capabilities |
| **Resources** | Application (app decides) | Provide data |
| **Prompts** | User (triggered by action) | Predefined workflows |

### Key Client Functions

```python
list_tools()              # await self.session.list_tools()
call_tool(name, input)    # await self.session.call_tool(...)
list_prompts()            # await self.session.list_prompts()
get_prompt(name, args)    # await self.session.get_prompt(...)
```

**MCP Inspector:** `mcp dev server_file.py` — in-browser debugging tool. Essential before wiring to a full application.

---

## Key Takeaways

- Vertex AI's native `text-embedding-005` is the Google Cloud embedding option — evaluate against Voyage AI for RAG quality
- 5-minute cache TTL on Vertex AI — design cache breakpoints around stable content
- MCP pattern is identical across all three platforms — the architecture is provider-agnostic
- Contextual retrieval + reranking are the two highest-leverage RAG quality improvements on any platform

---

## Personal Notes

- `text-embedding-005` being native to Vertex AI is a meaningful integration advantage for Google Cloud-native projects — no separate Voyage AI account needed
- The 5-minute cache TTL is the same as Bedrock — worth noting for cost projections on high-traffic applications
- MCP running on Vertex AI enables connecting Google Cloud services (BigQuery, Cloud Storage, etc.) as MCP servers — a powerful native integration pattern

---

## Follow-Up Questions

- Does Vertex AI have a native vector database (like AlloyDB or Vertex AI Vector Search) that integrates directly with this RAG pattern?
- How does `text-embedding-005` compare to Voyage AI for multilingual or domain-specific retrieval tasks?
- Can Vertex AI Pipelines orchestrate a full RAG preprocessing workflow automatically?
