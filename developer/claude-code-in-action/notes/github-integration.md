# GitHub Integration

**Course:** Claude Code in Action
**Section:** Getting Hands On
**Status:** ✅ Complete

---

## Objective

Learn how to integrate Claude Code into GitHub Actions for automated code review, task assignment, and workflow automation triggered by pull requests and issues.

---

## Key Concepts

### What GitHub Integration Is

An official integration that allows Claude Code to run inside GitHub Actions. Claude receives GitHub-specific tools — comments, commits, PR creation — and can be triggered by PRs, issues, or mentions.

---

### Setup Process

1. Run `/install GitHub app` in Claude Code
2. Install the Claude Code app on GitHub
3. Add your API key
4. Two GitHub Actions are auto-generated

---

### Default Actions

| Action | Trigger | Behavior |
|---|---|---|
| Mention support | `@Claude` in issues or PRs | Assigns and executes the described task |
| PR review | New pull request opened | Automatic code review |

---

### Customization

- Actions are customizable via config files in `.github/workflows/`
- Custom instructions can be passed directly as context to Claude
- MCP servers can be integrated — Claude gains access to external tools during Actions runs

---

### Permission Requirements

- All permissions for Claude Code must be explicitly listed in the Actions config
- MCP server tools each require individual permission listing — no shortcuts or wildcards

---

### Extended Example — Playwright in GitHub Actions

1. Playwright MCP server integrated into the workflow
2. Development server spins up before Claude runs
3. Claude visits the running app in a browser
4. Tests functionality, creates checklists, verifies issues
5. Produces automated test results as PR feedback

---

## Key Takeaways

- GitHub integration turns Claude Code into a persistent team member on every PR
- `@Claude` mentions enable on-demand task execution directly from issues and PRs
- MCP servers extend what Claude can do inside Actions — not just code review but full test automation
- Permissions must be explicit — plan for this during setup

---

## Personal Notes

- This is CI/CD with AI reasoning built in — a significant step beyond standard linting and test pipelines
- PII detection via infrastructure analysis (from the demo lesson) running automatically on every PR is immediately valuable for any project handling sensitive data
- The explicit permissions requirement is the right design for security-conscious environments

---

## Follow-Up Questions

- Can Claude be configured to only review specific file types or directories in a PR?
- How are API costs managed when Claude runs on every PR in a high-volume repo?
- Is there a way to have Claude approve or block a PR merge based on its review findings?
