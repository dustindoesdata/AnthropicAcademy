# Making Changes

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Learn the tools and modes available in Claude Code for making code changes effectively — including screenshot input, planning modes, and Git integration.

---

## Key Concepts

### Screenshot Integration

Paste screenshots directly into Claude Code using **Control-V** (not Command-V on macOS) to help Claude understand specific UI elements or errors visually.

---

### Performance Boosting Modes

| Mode | Activation | Best For |
|---|---|---|
| Plan Mode | Shift + Tab (twice) | Multi-step tasks requiring wide codebase understanding |
| Thinking Mode | Phrases like "Ultra think" | Tricky logic or debugging specific issues |

- **Plan Mode** = handles *breadth* — research more files, create detailed implementation plans before executing
- **Thinking Mode** = handles *depth* — extended reasoning budget for complex logic
- Both modes can be **combined** for highly complex tasks
- Both consume additional tokens — factor in cost for heavy usage

---

### Git Integration

Claude Code can stage and commit changes and write descriptive commit messages automatically.

---

### Recommended Workflow

1. Screenshot the problematic area
2. Paste with **Control-V**
3. Describe the desired change
4. Enable Plan Mode and/or Thinking Mode for complex tasks
5. Review and accept the implementation

---

## Key Takeaways

- Control-V (not Command-V) is the correct paste shortcut for screenshots on macOS
- Plan Mode = breadth, Thinking Mode = depth — use them for the right job
- Combining both modes is an option for the hardest tasks
- Token costs increase with both modes — use intentionally

---

## Personal Notes

- Plan Mode maps to how I scope data engineering tasks before writing code — research first, execute second
- "Ultra think" as a natural language trigger is an interesting design choice — worth testing different phrasings
- Git auto-commit with descriptive messages is a workflow accelerator for documentation-heavy environments

---

## Follow-Up Questions

- Are there other trigger phrases for Thinking Mode beyond "Ultra think"?
- Can Plan Mode output be reviewed and edited before Claude executes?
- How does token cost scale between normal, Plan, Thinking, and combined modes?
