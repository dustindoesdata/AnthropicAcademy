# Controlling Context

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Learn the tools available for managing conversation history and context in Claude Code — keeping Claude focused, efficient, and free of accumulated noise.

---

## Key Concepts

### Context Control Commands

| Command | What It Does | When to Use |
|---|---|---|
| **Escape** | Stops Claude mid-response | Redirect a response going the wrong direction |
| **Escape + Memory (`#`)** | Stop + immediately add a memory to prevent repeated mistakes | Claude keeps making the same error |
| **Double Escape** | Rewinds conversation — shows all previous messages to jump back to an earlier point | Long debugging sessions with irrelevant back-and-forth |
| **Compact** | Summarizes conversation history while preserving Claude's learned knowledge | Long conversations that have gotten cluttered but Claude has gained useful expertise |
| **Clear** | Deletes entire conversation history for a fresh start | Switching to a completely unrelated task |

---

### The Escape + Memory Pattern

One of the most powerful error-prevention techniques available. Stop Claude mid-mistake, then use `#` to write a memory that prevents the same issue from recurring in future turns or sessions.

---

### Compact vs. Clear

- **Compact** preserves what Claude has learned — use it when the *knowledge* is worth keeping but the *conversation* has become noisy
- **Clear** is a hard reset — use it when you are starting something entirely different

---

## Key Takeaways

- Context control is as important as context setting
- Escape + Memory is the highest-leverage tool for preventing repeated errors
- Double Escape recovers lost ground without losing relevant context
- Compact and Clear serve different purposes — know which one the situation calls for

---

## Personal Notes

- Compact vs. Clear maps to a concept I use in data pipelines: preserve state vs. reinitialize — same decision framework
- Escape + Memory is essentially inline prompt correction — a technique worth building into any workflow that runs long sessions
- For client work, Double Escape could recover from a bad direction without starting over entirely

---

## Follow-Up Questions

- Does Compact preserve memory across sessions or only within the current conversation?
- Is there a way to see what Claude retained after a Compact operation?
- Can memories added via `#` be reviewed and deleted later?
