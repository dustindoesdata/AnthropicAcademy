# Anthropic Apps, Agents & Workflows

**Course:** Claude with Google Cloud's Vertex AI
**Section:** Anthropic Apps & Agents and Workflows
**Status:** ✅ Complete

---

## Objective

Understand agent architecture, Claude Code patterns, and workflow design — all transferable across platforms including Vertex AI.

---

## Anthropic Apps — Claude Code and Computer Use

### Claude Code

Terminal-based AI coding assistant functioning as another engineer on the team.

**Workflow:**
1. Run `claude` in terminal
2. Ask Claude to read README and execute setup
3. Run `init` — scans codebase, creates `claude.md`
4. `claude.md` auto-included as context in all future sessions

**Two effective prompting strategies:**

**Method 1 — Three-step:**
1. Identify relevant files, ask Claude to analyze
2. Describe feature, ask Claude to plan (no code yet)
3. Ask Claude to implement the plan

**Method 2 — Test-driven:**
1. Ask Claude to read relevant context
2. Ask Claude to suggest tests (no implementation)
3. Select tests, ask Claude to implement
4. Ask Claude to write code until tests pass

**Enhancements with MCP:** `claude mcp add [server-name] [startup-command]` — connects external servers (Sentry, Jira, Slack, custom tools).

**Parallelizing with Git work trees:**
```
Create work tree → Assign task → Work in isolation → Commit → Merge
```
Custom commands in `.claude/commands/` with `$ARGUMENTS` placeholder enable automation.

### Computer Use

Claude interacts with computer interfaces via screenshots, clicks, and keyboard input — running in isolated Docker containers.

**How it works:** Minimal schema → Claude sends tool use request (mouse/click/screenshot) → Docker executes → response back to Claude.

---

## Workflows and Agents

**Decision rule:** Use workflows when steps are known. Use agents when steps are uncertain.

### Workflow Patterns

**1. Evaluator-Optimizer (Chaining)**
Sequential steps where each feeds into the next. Best when AI consistently violates constraints in single-pass prompts — split into generate + review steps.

```
Input → Step 1 → Step 2 → Evaluator → Loop if needed → Output
```

**2. Parallelization**
Break complex task into simultaneous subtasks, aggregate results.

```
Input → [Subtask A] [Subtask B] [Subtask C] → Aggregator → Output
```

**Benefits:** Focus, modularity, scalability, quality.

**3. Routing**
Classify input first, then route to specialized pipeline.

```
Input → [Classifier] → Matching pipeline → Output
```

---

## Agents

**Agent = language model + tools + loop until goal achieved.**

**Tool abstraction principle:** Abstract tools beat specialized ones.
- ✅ `bash`, `web_fetch`, `file_write`
- ❌ `refactor_tool`, `install_dependencies`

Abstract tools combine in unexpected ways — agents discover combinations you didn't anticipate.

**Environment inspection:** Agents examine environment state after actions to understand results.
- Computer use: screenshot after every click
- Code editing: read file before modifying
- Video processing: extract frames to verify output

---

## Workflows vs. Agents

| | Workflows | Agents |
|---|---|---|
| **Steps** | Predetermined | Dynamic |
| **Accuracy** | Higher | Lower |
| **Testing** | Easier | Harder |
| **Flexibility** | Lower | Higher |

> **Core principle:** Prioritize workflows for reliability. Use agents only when flexibility is truly required. Users want 100% working products over fancy but unreliable agents.

---

## Key Takeaways

- Claude Code and computer use are platform-agnostic — they run locally regardless of which cloud API backs them
- The three workflow patterns (chaining, parallelization, routing) cover the vast majority of multi-step AI tasks
- Abstract tools outperform specialized tools for agent flexibility
- Environment inspection is what separates a useful agent from a blind one

---

## Personal Notes

- The "users want 100% working products over fancy agents" principle is the most important client-facing guidance from this entire course series
- Git work tree parallelization is worth prototyping for the HomeschoolIQ pipeline — parallel data processing branches managed by Claude Code instances
- Routing workflows are the right architecture for any multi-domain enterprise chatbot — classify first, then route to specialized prompts

---

## Follow-Up Questions

- Can Claude Code be configured to use Vertex AI as its underlying model instead of the Anthropic API directly?
- Is there a Google Cloud-native equivalent to GitHub Actions for automated debugging workflows?
- How does Vertex AI's integration with Google Cloud services affect agent architecture — can agents access BigQuery natively?

---

## Additional Links

- 🏅 [Certificate of Completion](http://verify.skilljar.com/c/wy38cny6iwrg)
