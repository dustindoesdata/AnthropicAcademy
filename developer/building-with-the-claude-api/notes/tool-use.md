# Tool Use

**Course:** Building with the Claude API
**Section:** Tool Use
**Status:** ✅ Complete

---

## Objective

Understand how tool use works, how to define and implement tools, and how to handle multi-turn tool conversations.

---

## What Tool Use Is

A method for Claude to access external information beyond its training data. By default, Claude only knows what it was trained on — tool use enables live, real-time data access.

### Tool Use Flow

1. Send initial request to Claude + tool instructions
2. Claude evaluates if external data is needed, requests specific information
3. Server runs code to fetch data from external sources
4. Send follow-up request to Claude with retrieved data
5. Claude generates final response using original prompt + external data

---

## Tool Functions

Plain Python functions called by Claude when it determines additional data is needed.

**Best practices:**
- Use descriptive function and argument names
- Validate inputs immediately and raise errors with meaningful messages
- Error messages are visible to Claude — it can retry with corrected parameters

```python
def get_current_datetime(date_format="%Y%m%d %H:%M:%S"):
    if not date_format:
        raise ValueError("date format cannot be empty")
    return datetime.now().strftime(date_format)
```

---

## Tool Schemas

JSON schema specifications that describe tool functions and their parameters for Claude.

**Schema structure:**
- `name` — tool identifier
- `description` — 3–4 sentences explaining what the tool does, when to use it, what it returns
- `input_schema` — JSON schema describing arguments with types and descriptions

**Schema generation shortcut:** Paste the tool function into Claude.ai and prompt: *"Write a valid JSON schema spec for tool calling for this function, following Anthropic API best practices."*

**Implementation tip:** Wrap schema dictionaries with `ToolParam()` from `anthropic.types` to prevent type errors.

---

## Handling Message Blocks

When tools are included, Claude's response contains **multiple content blocks**:
- **Text block** — user-facing explanation
- **Tool use block** — contains function name + arguments for execution

**Critical:** Append `response.content` (all blocks) to message history — not just the text block.

---

## Sending Tool Results

```python
# Tool result block structure
{
    "type": "tool_result",
    "tool_use_id": original_tool_use_id,  # matches the request
    "content": json.dumps(tool_output),
    "is_error": False
}
```

**Tool use ID** links multiple tool requests to correct results when Claude makes simultaneous calls.

**Follow-up request requirements:**
- Include complete message history
- Include original tool schemas even if not using tools again
- Tool result block goes in a **user** message, not assistant

---

## Multi-Turn Tool Conversations

Claude can use multiple tools sequentially (tool chaining) to answer a single query.

**Example:** "What day is 103 days from today?"
1. Claude calls `get_current_datetime`
2. Claude calls `add_duration_to_datetime`
3. Claude provides final answer

### Implementation Pattern

```python
while True:
    response = chat(messages, tools=tools)
    messages.append(response)
    
    if response.stop_reason != "tool_use":
        break
    
    tool_results = run_tools(response)
    messages.append({"role": "user", "content": tool_results})
```

**`stop_reason` values:**
- `"tool_use"` — Claude wants to call a tool; continue the loop
- Anything else — final response; break

### Adding Multiple Tools

3-step pattern after initial framework setup:
1. Add tool schemas to the tools list in `run_conversation`
2. Add conditional cases in `run_tool` to handle new tool names
3. Implement the actual tool functions

---

## The Batch Tool

A tool that enables Claude to run multiple tools **in parallel** within a single message.

**Problem:** Claude rarely sends multiple tool use blocks simultaneously in practice — defaults to sequential calls.

**Solution:** Create a batch tool schema that takes a list of invocations. Claude calls the batch tool with an array of desired tool executions, which are then run in parallel.

```python
# Batch tool input structure
{
    "invocations": [
        {"tool_name": "get_current_datetime", "arguments": {}},
        {"tool_name": "get_weather", "arguments": {"city": "Seattle"}}
    ]
}
```

**Result:** Single request-response cycle instead of multiple sequential rounds.

---

## Tools for Structured Data Extraction

An alternative to prefill + stop sequences — more reliable, more complex.

```python
# Force tool calling
tool_choice = {"type": "tool", "name": "your_tool_name"}

# Access structured output
response.content[0].input  # the structured data
```

**When to use:**
- Reliability is more important than simplicity
- Prompt-based methods better for quick/simple extractions
- Tools better for complex/reliable extractions

---

## Fine-Grained Tool Calling & Streaming

| Mode | Behavior | Trade-off |
|---|---|---|
| Default | Buffers + validates JSON before sending chunks | Slower but validated |
| Fine-grained (`fine_grained: true`) | Sends chunks immediately, no validation | Faster streaming but may produce invalid JSON |

---

## Key Takeaways

- Tool use is the bridge between Claude's training data and the real world
- Always maintain full conversation history — including all content blocks
- The `while` loop + `stop_reason` check is the core pattern for multi-turn tool conversations
- Batch tool = parallel execution; tool chaining = sequential execution
- Tools for structured data extraction are more reliable than prefill + stop sequences

---

## Personal Notes

- The tool use pattern is identical to function calling in other LLM frameworks — transferable knowledge
- Batch tool parallelization maps directly to how I think about concurrent API calls in data pipelines — minimize round trips
- Using tools for structured data extraction is the production-grade version of the prefill trick — worth the extra setup for anything mission-critical

---

## Follow-Up Questions

- Is there a limit on how many tools can be registered in a single request?
- Can tools themselves make API calls to other Claude instances?
- How does error handling in tools affect Claude's reasoning — does it know to try a different approach?
