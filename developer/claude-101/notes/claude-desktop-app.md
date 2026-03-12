# The Claude Desktop App: Chat, Cowork, and Code

**Course:** Claude 101
**Section:** Core Features
**Status:** ✅ Complete

---

## Objective

Understand the three modes of the Claude desktop app — Chat, Cowork, and Code — and know when to use each.

---

## Key Concepts

### Overview

The Claude desktop app provides three distinct modes, each optimized for a different type of work:

| Mode | Optimized For | Key Features |
|---|---|---|
| **Chat** | Quick exchanges, brainstorming, drafting, Q&A | Quick entry, screenshots, dictation, desktop connectors |
| **Cowork** | Complex sustained work: research, analysis, finished documents | Folder access, scheduled tasks, browser use, plugins, subagents |
| **Code** | Building software: writing, testing, deploying code | Ask/Code/Plan modes, visual diffs, git integration, local and remote environments |

> Cowork and Code run on the same engine — both are Claude Code underneath, running locally, capable of independent work and spinning up sub-agents.

---

### Chat Mode

Best for back-and-forth conversations, quick questions, and iterative drafting.

**Desktop-specific features:**
- **Quick entry** — Double-tap Option key (Mac) to pull up Claude as an overlay over any app
- **Screenshots & window sharing** — Capture exactly what's on your screen instead of describing it (Mac)
- **Dictation** — Talk through a problem instead of typing (Mac)
- **Desktop connectors** — Connect local tools and services

**Use it when:**
- You need a quick answer while working in another app
- You want to think through a structure verbally
- You want to query across connected tools like Apple Notes

---

### Cowork Mode

Built for work that takes real effort across multiple sources.

**Key capabilities:**
- **Folder access** — Give Claude a folder; it reads, reasons over it, and saves work back to the same place
- **Scheduled tasks** — Define recurring work (daily briefings, weekly roundups, inbox triage) and Claude runs it automatically when the app is open
- **Browser use** — Connect Claude in Chrome to navigate websites, interact with pages, and pull data directly into tasks
- **Plugins** — Add capabilities like live financial data, internal knowledge base search, or compliance frameworks
- **Protected environment** — Cowork runs in a contained space; Claude can only access folders you explicitly share

**Use it when:**
- You're researching across many sources and need a structured brief
- You're parsing large documents where every detail matters
- You have recurring admin work you want automated on a schedule

> *Currently a research preview for Pro, Max, Team, and Enterprise users.*

---

### Code Mode

A full development environment inside the desktop app, powered by Claude Code.

**Interaction modes:**
| Mode | Behavior |
|---|---|
| **Ask** | Claude proposes every change and waits for approval — review a visual diff before anything is modified |
| **Code** | Claude applies file changes automatically but checks before running terminal commands |
| **Plan** | Claude outlines its full approach before touching anything — review the strategy first |

**Environments:**
- **Local** — Claude works directly with files on your machine, can access local tools and run a dev server
- **Remote** — Claude connects to a GitHub repo and works in a cloud environment; sessions continue even if you close the app

> *Rolling out to Pro, Max, Team, and Enterprise users.*

---

## Key Takeaways

- Chat is for dialogue; Cowork is for production; Code is for development
- Cowork and Code share the same Claude Code engine — the difference is the workspace and use case
- Scheduled tasks in Cowork remove the recurring admin loop entirely
- Code's three interaction modes (Ask/Code/Plan) let you dial in exactly how much autonomy Claude has

---

## Personal Notes

- Cowork's "query all your tools like a database" capability is the enterprise automation use case I've been building toward — this is the native version of what I've been doing with scripts
- Scheduled tasks map directly to the Boogaerts calendar automation project — this is the productized version of that workflow
- Code's Plan mode is essentially my mental model for how I scope data engineering work — outline before execute

---

## Follow-Up Questions

- How does Cowork handle conflicting information across multiple connected sources?
- Can scheduled tasks be triggered by events (e.g., a new file in a folder) rather than just time?
- What's the difference between Cowork plugins and MCP servers in Claude Code?
