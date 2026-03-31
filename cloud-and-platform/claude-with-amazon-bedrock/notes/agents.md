# Agents

**Course:** Claude with Amazon Bedrock
**Section:** Agents
**Status:** ✅ Complete

---

## Objective

Understand agent architecture, Claude Code as an agent implementation, parallelization patterns, automated debugging, and computer use — all within the AWS Bedrock context.

---

## Qualities of Agents

**Agent = language model with tool access, executed repeatedly until goal achieved.**

### Key Agent Qualities

- **Tool-centric operation** — agents use tools in loops until goal achieved or error occurs
- **Information gathering focus** — most tool calls gather environment data, not modify it
- **Context dependency** — agents rely on tools for environment inspection; they have no inherent world knowledge
- **Focused toolset** — small set of tools with clear purposes
- **High-value, low-risk tasks** — valuable work where mistakes don't cause major economic or safety damage

### Core Agent Loop

```
Tool execution → Environment inspection → Goal progress assessment → Repeat until success/failure
```

**Design principle:** Abstract tools outperform specialized tools. `bash`, `web_fetch`, `file_write` beat `refactor_tool`, `install_dependencies`. Abstract tools can be combined in unexpected ways.

---

## Claude Code as an Agent

Claude Code = AI coding assistant functioning as another engineer on the team — not just a code generator.

**Workflow:**
1. Download project, open in editor
2. Run `claude` command in terminal
3. Ask Claude to read README and execute setup directions
4. Run `init` — Claude scans codebase, creates `claude.md` with architecture and coding style notes
5. `claude.md` automatically included as context in all future requests

**Memory types:** Project, Local, User
- Use `# [note]` to append directions to `claude.md`

### Two Effective Workflows

**Method 1 — Three-step:**
1. Identify relevant files, ask Claude to analyze them
2. Describe feature, ask Claude to plan (no code yet)
3. Ask Claude to implement the plan

**Method 2 — Test-driven:**
1. Ask Claude to read relevant context
2. Ask Claude to suggest tests (no implementation)
3. Select tests, ask Claude to implement them
4. Ask Claude to write code until tests pass

**Core principle:** Effort multiplier — more detailed direction yields significantly better results.

---

## Enhancements with MCP Servers

Claude Code has an embedded MCP client — connect external servers to expand functionality.

```bash
claude mcp add [server-name] [startup-command]
# Example:
claude mcp add documents "uv run main.py"
```

**Common use cases:**
- Sentry — fetch production error details
- Jira — view ticket contents
- Slack — send completion notifications
- Custom development workflow tools

---

## Parallelizing Claude Code

Run multiple Claude instances simultaneously using **Git work trees** — isolated workspaces, each on a separate branch.

**Workflow:**
```
Create work tree → Assign task → Work in isolation → Commit → Merge back to main
```

**Custom commands:** Create `.claude/commands/filename.md` with `$ARGUMENTS` placeholder. Access via `/project:command-name` syntax.

**Result:** One developer managing a virtual team — scales to unlimited parallel instances. Claude handles Git operations including merge conflict resolution.

---

## Automated Debugging

Use AI agents to automatically detect, diagnose, and fix production errors.

**Core workflow:**
1. GitHub Action runs daily
2. Pulls CloudWatch logs from last 24 hours
3. Filters and deduplicates errors
4. Claude analyzes each error and generates fixes
5. Commits changes and opens pull request for review

**Common use case:** Configuration errors between environments (model IDs, API keys valid locally but failing in production).

**Key limitation:** Still requires human review of proposed fixes before merging.

---

## Computer Use

Claude's ability to interact with computer interfaces through visual observation and control.

**Capabilities:** Screenshots, mouse clicks, keyboard input, multi-step autonomous navigation.

**How it works (tool use flow):**
1. Minimal schema sent to Claude (expands internally to full structure)
2. Claude sends tool use request (mouse move, click, screenshot, etc.)
3. Docker container executes programmatic key presses/mouse movements
4. Response sent back to Claude

> Claude doesn't directly manipulate computers — computer use is a tool system + developer-provided Docker computing environment.

**Primary use cases:** QA testing automation, UI interaction testing, repetitive computer task automation.

---

## Key Takeaways

- Agents = LM + tools + a loop — context comes entirely from tool calls, not pre-written knowledge
- Claude Code is the reference implementation of agent architecture in practice
- Git work trees enable true parallelization — the scaling pattern for complex development
- Automated debugging via GitHub Actions + CloudWatch is immediately deployable for any AWS-hosted application
- Computer use is tool use + Docker — Claude decides, the container executes

---

## Personal Notes

- The "information gathering focus" quality of agents maps directly to how I think about data pipeline orchestration — most steps are reads, not writes
- Automated debugging via CloudWatch + Claude Code is a pattern I should prototype for any Bedrock-hosted application — production-only bugs are a real pain point
- Git work tree parallelization is worth exploring for the HomeschoolIQ pipeline — parallel feature branches managed by Claude instances

---

## Follow-Up Questions

- Can Claude Code be configured to use Bedrock as its underlying model instead of the Anthropic API directly?
- How does computer use interact with AWS infrastructure — can it operate on EC2 or ECS instances?
- Is there a Bedrock-native equivalent to GitHub Actions for automated debugging workflows?

---

## Additional Links

- 🏅 [Certificate of Completion](http://verify.skilljar.com/c/dg79n3345m75)
