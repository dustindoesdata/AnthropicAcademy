# Defining Hooks

**Course:** Claude Code in Action
**Section:** Hooks and the SDK
**Status:** ✅ Complete

---

## Objective

Learn the technical structure of hooks — how they receive data, how exit codes control behavior, and how to identify which tools to target.

---

## Key Concepts

### Hook Execution Flow

1. Claude prepares to call a tool
2. Hook command fires, receiving tool call data via **stdin as JSON**
3. Hook parses the JSON and applies its logic
4. Hook exits with an appropriate code
5. Claude proceeds or is blocked based on the exit code

---

### Tool Call Data Structure (JSON via stdin)

```json
{
  "tool_name": "read",
  "input": {
    "file_path": "/path/to/file"
  }
}
```

---

### Exit Codes

| Exit Code | Meaning |
|---|---|
| `0` | Allow — tool call proceeds normally |
| `2` | Block — tool call is cancelled (pre-tool use only) |

- When exit code `2` is returned, **stderr output** is sent back to Claude as a feedback message explaining why it was blocked.
- Post-tool use hooks cannot block — exit code `2` has no effect after execution.

---

### Finding Available Tool Names

Rather than memorizing tool names, ask Claude directly:
> *"What are all the available tool names I can use in a hook matcher?"*

**Common file-access tools:**
- `read` — reads file contents
- `grep` — searches within files

---

## Key Takeaways

- Hooks communicate via stdin (input) and exit codes (output)
- Exit code `0` = allow, exit code `2` = block
- Stderr is the feedback channel back to Claude when blocking
- Pre-hooks can block; post-hooks cannot
- Ask Claude for tool names rather than guessing

---

## Personal Notes

- Stdin JSON + exit codes is a clean, Unix-style interface — easy to implement in any language
- Stderr as the Claude feedback channel is a clever reuse of standard streams
- The "ask Claude for tool names" tip is a good example of using the tool to learn the tool

---

## Follow-Up Questions

- Is there a full list of all available tool names documented somewhere?
- Can a hook modify the tool input before it executes (not just allow or block)?
- What happens if a hook script crashes or times out — does Claude proceed or halt?
