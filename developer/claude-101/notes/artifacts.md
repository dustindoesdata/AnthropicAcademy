# Artifacts

**Course:** Claude 101
**Section:** Core Features
**Status:** ✅ Complete

---

## Objective

Understand what Artifacts are, the types Claude can create, and how to get the most out of them.

---

## Key Concepts

### What Artifacts Are

Standalone, interactive outputs that Claude creates in a dedicated window alongside your conversation. Instead of a block of text or code buried in the chat, you see content rendered and ready to use.

**Claude automatically creates an artifact when content is:**
- Significant and self-contained (typically 15+ lines)
- Something you're likely to edit, iterate on, or reuse
- Complex content that stands on its own without surrounding conversation
- Content you'll want to reference or use later

---

### Artifact Types

| Type | Best For |
|---|---|
| **Documents** (Markdown, plain text) | Meeting notes, reports, project plans, blog posts — anything text-heavy you'll export or keep editing |
| **Code snippets** | Working code in any language — Python, JavaScript, C++, etc. Copy or download to use directly |
| **HTML pages** | Complete web pages (HTML + CSS + JS in one file) — landing pages, forms, prototypes |
| **SVG images** | Logos, icons, illustrations — renders directly in the artifact window |
| **Mermaid diagrams** | Flowcharts, sequence diagrams, Gantt charts, org charts |
| **React components** | Interactive UI elements with real logic — calculators, dashboards, games, data visualizations |

---

### Creating Artifacts

Just describe what you want — Claude determines whether to present it as an artifact.

**Example prompts:**
- "Create a flowchart showing our customer onboarding process"
- "Build an interactive dashboard for monthly expenses with a breakdown chart"
- "Design a landing page for a productivity app"
- "Write a project brief template I can reuse"

If Claude responds in chat when you expected an artifact:
> "Create this as an artifact" or "Show me this in an artifact"

---

### Working with Artifacts

Once created, the artifact appears in a dedicated window to the right. From there:
- **View** — toggle between preview and underlying code
- **Copy** — grab the content for use elsewhere
- **Download** — save as a file to your computer
- **Inspect code** — see exactly what Claude generated

---

### Sharing and Publishing

| Method | Who Can Access |
|---|---|
| Copy / Download | For personal use or sharing via other channels |
| Share within org (Claude for Work) | Team/Enterprise — stays internal, requires team authentication |
| Publish publicly | Anyone with the link — no Claude account required; others can "remix" it |

> Publishing makes the artifact public but your chat remains private. You can unpublish at any time.

---

### Tips for Better Artifacts

- **Be specific** — "Build a budget tracker where I can input expenses by category, see a pie chart, and get a warning when over budget" beats "Build a budget tracker"
- **Describe the end user** — "This flowchart is for new employees" changes the output vs. "This is for the engineering team"
- **Iterate incrementally** — one change at a time makes it easier to catch issues
- **Request explicitly when needed** — if Claude doesn't create an artifact automatically, just ask

---

## Key Takeaways

- Artifacts are rendered, interactive, and ready to use — not just text responses
- Six types cover documents, code, web pages, graphics, diagrams, and interactive components
- Publishing makes artifacts shareable without exposing your conversation
- Specificity and end-user context are the two biggest drivers of artifact quality

---

## Personal Notes

- React components as working artifacts (not mockups) is a significant capability — interactive data visualizations without a full dev environment
- The "remix" feature on public artifacts is an interesting community layer — worth exploring for sharing reusable templates
- Mermaid diagrams are directly applicable for data pipeline documentation and system architecture visuals in client deliverables

---

## Follow-Up Questions

- Can artifacts be embedded in external websites or tools?
- Is there a way to version-control artifact iterations within a conversation?
- Can a React artifact connect to external APIs or is it fully sandboxed?
- How does the artifact window handle very large outputs (e.g., a 500-line Python script)?
