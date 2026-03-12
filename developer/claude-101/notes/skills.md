# Skills

**Course:** Claude 101
**Section:** Core Features
**Status:** ✅ Complete

---

## Objective

Understand what Skills are, how they differ from Projects, and how to create and use custom Skills.

---

## Key Concepts

### What Skills Are

Folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. Think of them as expertise packages — they teach Claude how to complete specific tasks in a repeatable, consistent way.

> If you've used Claude to create Excel spreadsheets, PowerPoint presentations, Word documents, or PDFs — those are powered by Skills running behind the scenes.

---

### Two Types of Skills

| Type | Who Creates It | How It's Invoked |
|---|---|---|
| **Anthropic Skills** | Created and maintained by Anthropic | Automatically — Claude invokes when relevant, no action needed |
| **Custom Skills** | You or your organization | Automatically when relevant, once uploaded to settings |

**Anthropic Skills include:** Enhanced document creation for Excel, Word, PowerPoint, and PDF.

**Custom Skills examples:**
- Apply your company's brand guidelines to presentations
- Structure meeting notes in a specific format
- Execute your organization's data analysis workflows

---

### Enabling Skills

Skills require **Code execution and file creation** to be enabled (Skills run in Claude's secure sandboxed environment).

**Setup:**
1. Navigate to **Settings > Capabilities**
2. Ensure **Code execution and file creation** is toggled on
3. Scroll to the **Skills** section
4. Toggle individual skills on or off as needed

> *Currently a feature preview for Pro, Max, Team, and Enterprise plans.*
> Enterprise: Org Owners must enable both Code execution and Skills in Admin settings first.
> Team: Enabled by default at the org level.

---

### Creating a Custom Skill

The easiest method is through conversation with Claude — no code required.

**Process:**
1. Start a new chat and describe what you want: "I want to create a skill for writing quarterly business reviews"
2. Answer Claude's questions about your workflow, what good output looks like, and when you'd use it
3. Upload reference materials if you have them (templates, style guides, examples)
4. Download the generated ZIP file
5. Go to **Settings > Capabilities > Skills > Upload skill** and select the ZIP

The skill will appear in your Skills list and Claude will invoke it automatically on relevant tasks.

---

### File Execution Capabilities

Upload actual files (`.xlsx`, `.pptx`, `.docx`, `.pdf`) and Claude can:
- Create slides
- Perform analyses
- Add suggested edits
- Save outputs back to Google Drive or for download

> Requires toggling **Allow limited network access** when prompted.

---

### Skills vs. Projects

| | Projects | Skills |
|---|---|---|
| **Purpose** | Store knowledge Claude references | Define processes Claude executes |
| **Best for** | Long-term context, reference materials, team collaboration | Repeatable workflows, multi-step tasks, consistent methodology |
| **Example** | Customer hub, research buddy, feedback generator | Brand guidelines, blog drafting, PDF creation |
| **Persistence** | Available across all chats in the project | Applied when the skill is invoked |

> **The combination:** A skill can reference knowledge stored in a project. The project provides the *what* (information); the skill provides the *how* (process).

---

## Key Takeaways

- Skills are process packages; Projects are knowledge packages — they complement each other
- Anthropic's built-in Skills handle common document creation tasks automatically
- Custom Skills can encode your specific workflows, brand guidelines, and methodology
- Creating a custom Skill requires no code — just a conversation with Claude

---

## Personal Notes

- The Skills + Projects combination is the architecture I'd use for a client engagement template: Project holds the client context, Skill encodes the deliverable process
- Custom Skills for data analysis workflows could encode my standard EDA process, report structure, and visualization preferences — consistent output every time
- The no-code Skill creation via conversation is an accessibility win — clients could build their own without technical support

---

## Follow-Up Questions

- Can a custom Skill call external APIs or is it limited to Claude's sandboxed environment?
- How do Skills interact with MCP servers — can a Skill trigger an MCP tool?
- Is there a size or complexity limit on custom Skills?
- Can Skills be shared across an organization or are they individual only?
