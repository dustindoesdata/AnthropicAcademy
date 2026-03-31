# Tool Use

**Course:** Claude with Amazon Bedrock
**Section:** Tool Use
**Status:** ✅ Complete

---

## Objective

Implement tool use with Claude via the AWS Bedrock API — from defining tool functions and JSON schemas through multi-turn tool conversations, batch tool use, and structured data extraction.

---

## Introducing Tool Use

Tool use enables Claude to access external information beyond its training data.

**Flow:**
1. Send request to Claude + tool instructions
2. Claude requests specific external data
3. Server fetches data from external APIs
4. Send fetched data back to Claude
5. Claude generates final response with external data

**Key challenge:** Implementation order differs from logical flow. Reference the flow diagram frequently when building.

---

## Tool Functions

Plain Python functions called by Claude when it needs additional data.

**Best practices:**
- Use descriptive argument names — critical for Claude's understanding
- Validate inputs; raise errors with meaningful messages
- Claude sees error messages and can retry with corrected arguments

```python
def get_current_datetime(date_format="%Y%m%d%H%M%S"):
    return datetime.now().strftime(date_format)
```

---

## JSON Schema for Tools

**Schema creation process:**
1. Create dictionary with function arguments and sample values
2. Convert to JSON format
3. Use an online "JSON to JSON schema converter"
4. Remove `$schema` statement from output
5. Add description fields to each property

**Description best practices:**
- Overall tool description: 3–4 sentences — what it does, when to use it, what it returns
- Property descriptions: detailed explanations of each argument's purpose

---

## Handling Tool Use Responses

**`stop_reason = "tool_use"` signals Claude wants to call a tool.**

**Assistant message content structure:**
- **Text part** — user-facing explanation
- **Tool use part** — contains `tool_use_id`, `name`, `input`

**Critical:** Append the entire assistant message (all parts) to conversation history — not just text.

**Tool choice configuration:**
| Option | Behavior |
|---|---|
| `auto` | Claude decides whether to use tools (default) |
| `any` | Forces Claude to use any available tool |
| `{tool: {name: "tool_name"}}` | Forces specific tool (useful for testing) |

---

## Running Tool Functions

**Tool use parts extraction:** Filter message parts for dictionaries containing `"tool_use"` key.

**Execution:**
```python
def run_tool(tool_name, tool_input):
    if tool_name == "get_current_datetime":
        return get_current_datetime(**tool_input)
    # ... other tools
```

**Tool result structure:**
```python
{
    "tool_result": {
        "tool_use_id": original_id,
        "content": [{"text": json_output}],
        "status": "success"  # or "error"
    }
}
```

**Error handling:** Wrap in try/except — return error status so Claude can analyze and retry.

---

## Sending Tool Results

**Conversation structure:**
```
User message → Assistant message (tool requests) → User message (tool results) → Assistant final response
```

**Critical:** Tool schemas must be included in follow-up calls — Claude needs them to understand tool definitions and process results correctly.

---

## Multi-Turn Conversations with Tools

```python
def run_conversation(messages):
    while True:
        result = chat(messages, tools)
        add_assistant_message(result.parts, messages)
        if result.stop_reason != "tool_use":
            break
        tool_results = run_tools(result.parts)
        add_user_message(tool_results, messages)
    return messages
```

Only process tool requests when `stop_reason == "tool_use"` — prevents adding empty tool result messages.

---

## Adding Multiple Tools

After initial framework setup, adding tools is straightforward:
1. Add tool schema to the tools list
2. Add case in `run_tools` mapping tool name to function
3. Implement the actual function

---

## Batch Tool Use

Create a special "batch tool" to parallelize tool calls.

**Schema:** Takes a list of `invocations`, each with `tool_name` + JSON-encoded arguments.

**Effect:** Claude treats multiple tool calls as a single batch operation, achieving parallelization in one request-response cycle instead of sequential rounds.

---

## Structured Data with Tools

Define a JSON schema as a fake tool where inputs match the desired data structure. More reliable than prompt-based extraction, more complex to set up.

**`tool_choice` parameter:** Forces Claude to use the specific extraction tool rather than responding normally.

### Flexible Tool Extraction

For rapid iteration without writing large schemas:
1. Define a single `toJSON` tool with a flexible object parameter
2. Specify the desired structure in the prompt text instead of the schema
3. Use escaped curly braces in F-strings for property definitions

**Trade-off:** Slightly lower quality vs. dedicated schemas, but fast to iterate.

---

## The Text Editor Tool

Built-in tool for file/directory manipulation.

**Bedrock-specific:** Must use exact string IDs:
- Claude 3.7: `"str_replace_editor"`
- Claude 3.5: `"str_replace_based_edit_tool"`

**Five commands:** `view`, `str_replace`, `create`, `insert`, `undo_edit`

**Note:** Claude provides the JSON schema automatically — developer must implement the actual file operation handlers.

---

## Key Takeaways

- Tool use bridges Claude's training data and real-world systems
- Always send full conversation history including all content blocks
- The while loop + `stop_reason` check is the core multi-turn tool pattern
- Batch tool = parallel execution; tool chaining = sequential execution
- Tools for structured extraction are more reliable than prefill + stop sequences

---

## Personal Notes

- The Bedrock `converse` API handles tool use with the same logical flow as the Anthropic API — the pattern is transferable
- Flexible tool extraction (`toJSON`) is the right choice for rapid prototyping; dedicated schemas for production
- Error handling in tools with retry is the right pattern for any production agentic system

---

## Follow-Up Questions

- Does the Bedrock `converse` API support the same `tool_choice` parameter as the Anthropic API?
- How does Bedrock handle tool use costs — are tool result tokens charged differently?
- Can Bedrock's inference profiles be used with tool use, or is there a different routing pattern?
