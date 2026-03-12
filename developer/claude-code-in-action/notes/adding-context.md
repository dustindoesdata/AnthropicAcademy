# Adding Context

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Understand how to manage context effectively in Claude Code — providing enough relevant information without overwhelming the model with noise.

---

## Key Concepts

### Why Context Management Matters

Too much irrelevant information decreases Claude's performance. The goal is to provide just enough relevant context for Claude to complete tasks effectively.

---

### The `/init` Command

Run on first use in a new project. Claude analyzes the entire codebase and creates a `Claude.md` file containing a project summary, architecture overview, and key files. This file is included in every subsequent request automatically.

---

### Three Types of Claude.md Files

| Type | Scope | Committed to Source Control? |
|---|---|---|
| Project level | Shared with entire team | Yes |
| Local level | Personal instructions only | No |
| Machine level | Global — applies to all projects | No |

---

### Memory Mode (`#` symbol)

Use the `#` symbol to edit any Claude.md file intelligently using natural language. Claude updates the file based on your instruction rather than requiring manual editing.

---

### `@` Symbol — Targeted File References

Use `@filename` to mention specific files in a request. This provides targeted context instead of letting Claude search broadly through the codebase.

**Best practice:** Reference critical files (like database schemas) in `Claude.md` so they are always available as context without needing to `@` mention them every time.

---

## Key Takeaways

- Context quality directly affects output quality — less noise, better results
- `/init` is the right starting point for any new project
- `Claude.md` is the primary context management tool
- `@` mentions give you surgical control over what Claude sees
- Memory mode (`#`) makes updating `Claude.md` fast and natural

---

## Personal Notes

- This maps directly to how I think about prompt engineering — context framing is everything
- The three-tier Claude.md structure (project/local/machine) is a clean separation of concerns — team vs personal vs global
- Database schema always-in-context is a best practice I'd apply immediately to any data engineering project

---

## Follow-Up Questions

- Is there a token limit on Claude.md file size before it starts hurting performance?
- Can multiple `@` mentions be used in a single request?
- How does `/init` handle monorepos with multiple services?
