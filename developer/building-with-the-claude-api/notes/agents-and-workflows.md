# Agents & Workflows

**Course:** Building with the Claude API
**Section:** Agents & Workflows
**Status:** ✅ Complete

---

## Objective

Understand the difference between workflows and agents, when to use each, and how to implement the core workflow patterns.

---

## Workflows vs. Agents

| | Workflows | Agents |
|---|---|---|
| **Definition** | Pre-defined series of calls with known exact steps | Flexible approach using tools Claude combines dynamically |
| **Task type** | Precise task with known sequence | Task details unclear; steps emerge at runtime |
| **Testing** | Easier — known execution path | Harder — unpredictable execution |
| **User input** | Requires specific inputs | Creates own inputs; can request more |
| **Success rates** | Higher — structured approach | Lower — delegated complexity |

> **Core principle:** Prioritize workflows for reliability. Use agents only when flexibility is truly required. Users want 100% working products over fancy agents.

---

## Workflow Patterns

### 1. Evaluator-Optimizer (Chaining)

Break large tasks into a series of sequential steps. Each step focuses on one specific subtask.

```
Input → Step 1 → Step 2 → ... → Output
```

**Producer:** Generates output
**Evaluator:** Assesses quality
**Loop:** Continues until evaluator accepts

**Example:** Image → 3D model converter
1. Claude describes uploaded image in detail
2. Claude uses CADQuery to model object from description
3. Create rendering of model
4. Claude compares rendering to original image
5. If inaccurate, repeat from step 2 with feedback

**Primary use case:** Long prompts with many constraints that Claude consistently violates in a single pass — split into generate + review steps.

---

### 2. Parallelization

Break one complex task into multiple **simultaneous** subtasks, then aggregate results.

```
Input → [Subtask A] [Subtask B] [Subtask C] → Aggregator → Output
```

**Benefits:**
- **Focus** — each subtask handles one specific analysis
- **Modularity** — individual prompts can be evaluated and improved separately
- **Scalability** — easy to add new subtasks
- **Quality** — reduces confusion from overly complex single prompts

**Example:** Material selection — run separate parallel requests for metal, polymer, ceramic, and composite analysis, then aggregate.

---

### 3. Routing

Categorize user input first, then route to the appropriate specialized pipeline.

```
Input → [Classifier] → Route to matching pipeline → Output
```

**Example:** Social media video scripts
1. User enters topic
2. Claude categorizes (educational, entertainment, etc.)
3. System routes to category-specific prompt template
4. Claude generates with appropriate tone and structure

**Benefit:** Ensures output matches topic nature without trying to handle all cases in one prompt.

---

## Agents

### What Makes an Agent

AI systems that create plans to complete tasks using provided tools — effective when exact steps are unknown.

**Tool abstraction principle:** Provide **generic/abstract** tools rather than hyper-specialized ones.

| Abstract (good) | Specialized (avoid) |
|---|---|
| `bash`, `web_fetch`, `file_write` | `refactor_tool`, `install_dependencies` |

Abstract tools can be combined in unexpected ways — agents discover combinations you didn't anticipate.

### Environment Inspection

After each action, agents need feedback to understand the new state of their environment.

**Examples:**
- Computer use: Claude takes a screenshot after every click/type to see what changed
- Code editing: Agent reads current file contents before modifying
- Video processing: Extract screenshots at intervals to inspect results

**Key benefit:** Enables agents to gauge progress, detect errors, and adapt rather than operating blindly.

---

## Claude Code as an Agent

Claude Code is the primary example of agent architecture in practice.

**Capabilities:** Search/read/edit files, web fetching, terminal access, MCP client support.

**Effective prompting strategies:**

**Method 1 — Three-step workflow:**
1. Identify relevant files, ask Claude to analyze them
2. Describe feature, ask Claude to plan (no code yet)
3. Ask Claude to implement the plan

**Method 2 — Test-driven development:**
1. Provide relevant context
2. Ask Claude to suggest tests for the feature
3. Select and implement chosen tests
4. Ask Claude to write code until tests pass

### Parallelizing Claude Code

Run multiple Claude instances simultaneously using **Git work trees** — isolated workspaces per instance, each on a different Git branch.

```
Create work tree → Assign task → Work in isolation → Commit → Merge back
```

**Custom commands:** Automate work tree creation via `.claude/commands/filename.md` with `$ARGUMENTS` placeholder.

**Result:** Single developer commanding a virtual team — scales to unlimited parallel instances based on management capacity.

---

## Automated Debugging

Use AI to automatically detect, analyze, and fix production errors.

**Workflow:**
1. GitHub Action runs daily
2. Fetches CloudWatch logs from last 24 hours
3. Claude identifies and deduplicates errors
4. Claude analyzes and generates fixes
5. Creates pull request with proposed solutions

**Common use case:** Configuration errors between environments (API keys, model IDs valid locally but failing in production).

---

## Computer Use

Claude's ability to interact with computer interfaces through visual observation and control.

**Capabilities:** Takes screenshots, clicks buttons, types text, navigates interfaces, follows multi-step instructions autonomously.

**How it works:**
- Runs in isolated Docker container
- User provides instructions via chat
- Claude observes screen visually and executes actions
- Reports task completion with detailed findings

**Tool use flow:**
1. Special schema sent to Claude (expands internally)
2. Claude sends tool use request (mouse move, click, screenshot, etc.)
3. Developer fulfills request via Docker container
4. Container executes programmatic key presses/mouse movements
5. Response sent back to Claude

> Claude doesn't directly manipulate computers — computer use is a tool system + developer-provided computing environment.

---

## Key Takeaways

- Workflows before agents — reliability beats flexibility for most production use cases
- The three workflow patterns (chaining, parallelization, routing) cover the vast majority of multi-step AI tasks
- Abstract tools outperform specialized tools for agent flexibility
- Environment inspection is what separates a useful agent from a blind one
- Claude Code parallelization via Git work trees is the scaling pattern for complex development tasks

---

## Personal Notes

- The routing workflow is exactly what I'd build for a multi-domain chatbot — classify first, then route to specialized prompts
- "Users want 100% working products over fancy agents" is the most important sentence in this section — applies directly to how I'd pitch AI solutions to clients
- Git work trees for parallel Claude instances is a multiplier pattern I should explore for the HomeschoolIQ pipeline — parallel data processing branches

---

## Follow-Up Questions

- How do you handle merge conflicts when multiple Claude Code instances modify overlapping files?
- Is there a recommended maximum number of parallel Claude Code instances before management overhead outweighs benefit?
- Can computer use be used in cloud environments or only local Docker?

---

## Additional Links

- [Claude Code Documentation](https://docs.claude.ai/en/docs/claude-code)
- [Computer Use Reference Implementation](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo)
