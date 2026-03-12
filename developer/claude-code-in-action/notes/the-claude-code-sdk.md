# The Claude Code SDK

**Course:** Claude Code in Action
**Section:** Hooks and the SDK
**Status:** ✅ Complete

---

## Objective

Understand what the Claude Code SDK is, how it differs from the terminal interface, and when to use it.

---

## Key Concepts

### What the Claude Code SDK Is

A programmatic interface to Claude Code available via CLI, TypeScript, and Python libraries. It contains the same tools as the terminal version and is designed for integration into larger pipelines and workflows.

---

### Default Permissions

| Permission | Default State |
|---|---|
| Read files | ✅ Enabled |
| Read directories | ✅ Enabled |
| Grep operations | ✅ Enabled |
| Write / edit files | ❌ Disabled — must be explicitly enabled |

**To enable write permissions:**
```typescript
const response = await query({
  prompt: "...",
  options: {
    allowTools: ["edit"]
  }
});
```

Or configure via the `.Claude` directory settings file.

---

### Output Format

SDK execution shows the raw conversation between the local Claude Code instance and the language model. The final response from Claude is the last message in the output.

---

### Best Use Cases

- Helper commands and scripts within existing projects
- Custom hooks that launch secondary Claude instances
- Adding AI intelligence to existing automation pipelines
- Anywhere you need Claude Code behavior triggered programmatically

**Not best suited for:** Standalone usage outside of an existing project context.

---

## Key Takeaways

- The SDK is the integration layer — it brings Claude Code into pipelines, not just terminals
- Read-only by default — write permissions are opt-in
- Output is the raw conversation log; final answer is always the last message
- Best as a component within a larger system, not a standalone tool

---

## Personal Notes

- The SDK is the piece that makes everything else in this course composable — hooks, custom commands, and GitHub Actions all connect here
- Read-only default is the right security posture — explicit opt-in for write is good design for enterprise environments
- Direct application: integrate SDK into a Fabric notebook or pipeline step to add Claude reasoning to an existing data workflow

---

## Follow-Up Questions

- Does the Python library have full feature parity with the TypeScript library?
- Can the SDK be used in a serverless environment (e.g., AWS Lambda)?
- Is there rate limiting applied at the SDK level or only at the API key level?
