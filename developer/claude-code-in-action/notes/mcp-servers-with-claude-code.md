# MCP Servers with Claude Code

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Learn how to extend Claude Code's capabilities using MCP (Model Context Protocol) servers — enabling Claude to interact with external systems beyond the default toolset.

---

## Key Concepts

### What MCP Servers Are

External tools that extend Claude Code's capabilities. They can run locally or remotely and give Claude access to systems it cannot reach with default tools.

**Most popular example:** Playwright MCP server — enables Claude to control browsers for web automation.

---

### Installation

```bash
claude mcp add [name] [start-command]
```

Run in terminal to add an MCP server to Claude Code.

---

### Permission Management

- First use of any MCP tool requires manual approval
- To auto-approve, add `"MCP__[servername]"` to the `allow` array in `settings.local.json`

---

### Practical Example — Playwright

**Workflow:**
1. Claude navigated to `localhost:3000`
2. Generated a UI component
3. Analyzed the styling quality via screenshot
4. Automatically updated generation prompts based on visual feedback
5. Iterated until styling improved significantly

**Result:** Automated prompt refinement produced measurably better component styling — demonstrating that MCP servers unlock full development automation cycles, not just single actions.

---

## Key Takeaways

- MCP servers expand Claude Code from a code editor into a full development automation platform
- Playwright is the go-to MCP server for anything involving browser interaction or UI testing
- Auto-approval via `settings.local.json` removes friction for trusted servers
- Multi-step external workflows become possible — not just single tool calls

---

## Personal Notes

- MCP servers are the integration layer — same concept as connectors in Fabric or data source bindings in a pipeline
- The visual feedback loop (screenshot → analyze → update prompt → regenerate) is a powerful pattern I could apply to report generation workflows
- Auto-approval settings are important for production automation — need to be intentional about what gets trusted

---

## Follow-Up Questions

- How do MCP servers handle authentication for external services (APIs, databases)?
- Can multiple MCP servers be active simultaneously?
- Is there a registry of available MCP servers beyond Playwright?
