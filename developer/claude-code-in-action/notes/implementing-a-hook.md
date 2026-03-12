# Implementing a Hook

**Course:** Claude Code in Action
**Section:** Hooks and the SDK
**Status:** ✅ Complete

---

## Objective

Walk through a complete hook implementation — configuration, script logic, and testing — using a `.env` file protection hook as the working example.

---

## Key Concepts

### The Use Case

Prevent Claude from reading `.env` files, which may contain secrets, API keys, or credentials.

---

### Configuration (`settings.local.json`)

```json
{
  "hooks": {
    "preToolUse": [
      {
        "matcher": "read|grep",
        "command": "node ./hooks/read_hook.js"
      }
    ]
  }
}
```

- **Hook type:** Pre-tool use (blocks before execution)
- **Matcher:** `read|grep` — pipe symbol separates tool names
- **Command:** Points to the hook script

---

### Hook Script Logic (`read_hook.js`)

```javascript
const input = JSON.parse(await readStdin());
const filePath = input.tool_input?.path || "";

if (filePath.includes(".env")) {
  console.error("Blocked: .env file access is not permitted.");
  process.exit(2);
}

process.exit(0);
```

**Key details:**
- Read JSON from stdin
- Check `tool_input.path` for the file path (with fallback handling)
- `console.error()` sends the message to Claude via stderr
- `process.exit(2)` blocks the tool call
- `process.exit(0)` allows it to proceed

---

### Testing Results

- Successfully blocks `.env` file access for both `read` and `grep` tools
- Claude recognizes it was blocked and acknowledges the read hook prevented access
- No workarounds — Claude respects the hook outcome

---

### Important: Restart Required

Claude Code must be restarted after any hook changes for them to take effect.

---

## Key Takeaways

- Hook configuration lives in `settings.local.json`
- The matcher uses pipe `|` to target multiple tools
- Script receives JSON via stdin, exits with `0` or `2`
- `console.error()` is the correct channel for Claude feedback (stderr)
- Always restart Claude Code after hook changes

---

## Personal Notes

- This `.env` protection pattern is immediately deployable on any project with secrets management requirements
- `console.error()` for Claude feedback is counterintuitive at first — worth remembering it's stderr, not stdout
- The fallback path handling (`tool_input?.path`) is good defensive coding practice for hook scripts

---

## Follow-Up Questions

- Can hooks be written in Python instead of Node.js?
- How do you handle hook scripts that need environment variables themselves?
- Is `settings.local.json` the only place hooks can be configured, or can they go in the project-level settings too?
