# Custom Commands

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Learn how to create and use custom slash commands in Claude Code to automate repetitive tasks.

---

## Key Concepts

### What Custom Commands Are

User-defined automation commands accessed via forward slash (`/`) in Claude Code. Each command is a markdown file containing instructions for Claude to execute.

---

### Setup

| Detail | Value |
|---|---|
| Location | `.Claude/commands/` folder in project directory |
| File naming | Filename becomes the command name (`audit.md` → `/audit`) |
| Activation | Restart Claude Code after creating a new command file |

---

### Using Arguments

Add `$arguments` as a placeholder in the command markdown file to accept runtime input.

**Example:**
```
/audit src/components/UserProfile.tsx
```

Arguments can be any string — file paths, descriptive text, labels, etc.

---

### Common Use Cases

- Dependency auditing
- Test generation
- Vulnerability scanning and fixes
- Any repetitive task that follows a consistent pattern

---

## Key Takeaways

- Custom commands turn repeatable workflows into single-line calls
- Markdown files are the command definition — simple to write and version control
- Arguments make commands reusable across different inputs
- Restart required after adding new commands

---

## Personal Notes

- This is essentially a macro system — same concept as reusable prompt templates I already build for data workflows
- Storing commands in `.Claude/commands/` means they're version-controlled with the project — good for team consistency
- Immediately useful: a `/document` command that auto-generates docstrings for a given file

---

## Follow-Up Questions

- Can commands call other commands?
- Is there a limit on command file length or complexity?
- Can commands be shared across projects at the machine level, or are they always project-scoped?
