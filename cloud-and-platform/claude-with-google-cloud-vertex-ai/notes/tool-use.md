# Tool Use

**Course:** Claude with Google Cloud's Vertex AI
**Section:** Tool Use with Claude
**Status:** ✅ Complete

---

## Objective

Implement tool use with Claude via Vertex AI — from tool functions and JSON schemas through multi-turn tool conversations, batch tools, and structured data extraction.

---

## Introducing Tool Use

Enables Claude to access external information beyond its training data.

**Flow:**
1. Send request + tool instructions to Claude
2. Claude requests specific external data
3. Server fetches data from external APIs
4. Send data back to Claude
5. Claude generates final response

---

## Tool Functions

Plain Python functions called when Claude needs additional data.

**Best practices:**
- Descriptive function and argument names — critical for Claude's understanding
- Validate inputs; raise meaningful errors — Claude sees error messages and can retry

---

## Tool Schemas

JSON configuration describing tool functions for Claude.

**Schema structure:** `name`, `description` (3–4 sentences), `input_schema`

**Generation shortcut:** Paste function into Claude.ai with Anthropic tool use docs — generates high-quality schema automatically.

---

## Handling Message Blocks

`stop_reason = "tool_use"` signals Claude wants to call a tool.

Response contains:
- **Text block** — user-facing explanation
- **Tool use block** — `tool_use_id`, `name`, `input`

**Critical:** Append entire `response.content` (all blocks) to history — not just text.

---

## Sending Tool Results

```python
{
    "type": "tool_result",
    "tool_use_id": original_id,
    "content": json_output,
    "is_error": False
}
```

Tool result goes in a **user** message. Tool schemas must be included in all follow-up calls.

---

## Multi-Turn Conversations with Tools

```python
while True:
    result = chat(messages, tools)
    messages.append(result)
    if result.stop_reason != "tool_use":
        break
    tool_results = run_tools(result.content)
    messages.append({"role": "user", "content": tool_results})
```

**`run_tools`:** Filters for `tool_use` blocks, executes each, returns list of `tool_result` blocks.
**`run_tool`:** Maps tool names to functions via if/elif statements.
**Error handling:** Wrap in try/except; set `is_error=True` and include error message — Claude can analyze and retry.

---

## Adding Multiple Tools

After initial framework: add schema to tools list + add case in `run_tool` + implement function. Process is standardized and scalable.

---

## The Batch Tool

Creates a special `batch_tool` schema that takes a list of `invocations`. Claude calls the batch tool instead of individual tools — achieves parallelization in one request-response cycle.

---

## Tools for Structured Data

Define a JSON schema as a fake tool where inputs match desired output structure. Force execution with `tool_choice` parameter. More reliable than pre-fill + stop sequences; more complex setup.

---

## The Text Edit Tool

Built-in tool for file/directory manipulation. Claude provides the JSON schema — developer implements the five command handlers: `view`, `str_replace`, `create`, `insert`, `undo_edit`.

**Vertex AI note:** Use exact model-version-specific string ID for the tool name.

---

## The Web Search Tool

Built-in tool — no custom implementation needed.

```python
{"type": "web_search_20250305", "name": "web_search", "max_uses": 5}
```

**Response blocks:** text, server tool use, web search results, citations.

**Domain restriction:** `allowed_domains` constrains search to authoritative sources.

---

## Key Takeaways

- Tool use pattern is identical across Anthropic, Bedrock, and Vertex AI — only SDK calls differ
- Always maintain full conversation history including all content blocks
- While loop + `stop_reason` check is the universal multi-turn tool pattern
- Batch tool = parallel; tool chaining = sequential

---

## Personal Notes

- The tool use mental model fully transfers from the Anthropic API and Bedrock courses — Vertex AI is just a different authentication layer on the same pattern
- Web search domain restriction is immediately useful on Vertex for any Google Cloud documentation assistant

---

## Follow-Up Questions

- Does Vertex AI support Google Search as a native tool beyond the standard web search schema?
- Can Vertex AI tools interact with BigQuery or other Google Cloud services natively?
