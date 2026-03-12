# Projects

**Course:** Claude 101
**Section:** Core Features
**Status:** ✅ Complete

---

## Objective

Understand what Claude Projects are, how to set them up, and when to use them over other organizational approaches.

---

## Key Concepts

### What Projects Are

Self-contained workspaces with their own memory, chat histories, knowledge bases, and customized instructions. Think of them as dedicated environments for specific work streams.

**Projects are ideal when you have:**
- Reference materials you'll use repeatedly
- Consistent requirements for how Claude should respond
- Team collaboration needs where multiple people work from the same foundation

---

### Setting Up a Project

**Step 1 — Create the project**
- Navigate to `claude.ai/projects` or hover over the left sidebar and click "Projects"
- Click "+ New Project," give it a descriptive name, and add a brief description

**Step 2 — Add project instructions**

Good instructions include:
- Context about what you're working on
- Process instructions (e.g., "First consider structure, then write the draft")
- Tone and style preferences
- Specific requirements (e.g., "Always include a call-to-action")

**Step 3 — Build your knowledge base**

Upload documents Claude should reference across all chats in the project.

Supported file types: PDF, DOCX, CSV, TXT, HTML, and more. Can also connect Google Drive.

> **Pro tip:** Name files descriptively — `Q4-2024-Brand-Guidelines.pdf` is more useful to Claude than `document1.pdf`.

---

### How Projects Handle Large Knowledge Bases

When a project's knowledge approaches the context window limit, Claude automatically enables **Retrieval Augmented Generation (RAG)** mode — intelligently retrieving only the most relevant content per request. This expands capacity by up to **10x** while maintaining response quality. A visual indicator appears when RAG is active.

---

### Collaboration (Claude for Work)

| Permission Level | Access |
|---|---|
| **Can view** | Read-only — can see contents and chat, but can't make changes |
| **Can edit** | Full collaboration — modify instructions, update knowledge, manage members |
| **Owner** | Full control including visibility settings and sharing |

Team members receive email notifications when a project is shared with them.

---

### Example Project Types

- **Q4 product launch** — product specs, competitive analysis, messaging notes
- **Research support** — competitive review, user research data, customer feedback
- **Client account hub** — brand guidelines, past deliverables, communication history
- **Event planning workspace** — venue contracts, speaker bios, attendee data
- **Job description generator** — past JDs, team charters, headcount request docs

---

## Key Takeaways

- Projects give Claude persistent, structured context across every conversation in the workspace
- Instructions + knowledge base = consistent, context-aware responses without re-explaining every time
- RAG mode handles large knowledge bases automatically — no manual management needed
- For teams, Projects become a shared intelligence layer everyone draws from

---

## Personal Notes

- A client account hub project is immediately applicable — brand guidelines, past deliverables, and communication history in one place would eliminate the re-upload cycle on every engagement
- The RAG auto-scaling is significant for data work — large schema docs, data dictionaries, and technical specs can all live in the project without hitting context limits
- Instructions as a persistent system prompt is the enterprise equivalent of what I do with prompt templates — this is the productized version

---

## Follow-Up Questions

- Can project instructions reference uploaded knowledge base files by name?
- Is there a limit on the number of files or total storage per project?
- How does RAG mode decide what's "most relevant" — is it semantic search?
- Can projects be version-controlled or backed up?
