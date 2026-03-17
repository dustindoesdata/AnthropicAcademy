# Built-In Tools

**Course:** Building with the Claude API
**Section:** Tool Use
**Status:** ✅ Complete

---

## Objective

Understand the built-in tools Anthropic provides — the Text Editor Tool and Web Search Tool — and how to use them without custom implementation.

---

## The Text Editor Tool

A built-in Claude tool for file and text operations: read, write, create, string replace, undo.

**Key characteristics:**
- Only the JSON schema stub is built in — **implementation must be custom-coded**
- Schema stub sent to Claude gets auto-expanded to full schema internally
- Schema type string varies by Claude model version (3.5 vs 3.7 have different dates)
- Enables Claude to act as a software engineer out of the box

**Workflow:**
1. Send minimal schema stub (name + type with version-specific date)
2. Claude expands to full schema internally
3. Claude sends tool use requests
4. Custom implementation executes actual file operations
5. Results sent back to Claude

**Use cases:**
- Replicate AI code editor functionality
- File system operations where native editors unavailable
- Automated code generation and refactoring
- Multi-file project manipulation

---

## The Web Search Tool

A built-in Claude tool for searching the web to find up-to-date or specialized information. **No custom implementation needed** — Claude handles search execution automatically.

### Schema

```python
{
    "type": "web_search_20250305",
    "name": "web_search",
    "max_uses": 5,           # limits total searches per request
    "allowed_domains": []    # optional — restrict to specific domains
}
```

### Response Structure

| Block Type | Contents |
|---|---|
| Text blocks | Claude's explanatory text |
| Tool use blocks | Search queries Claude executed |
| Web search result blocks | Found pages (title, URL) |
| Citation blocks | Specific text supporting Claude's statements |

### UI Rendering Pattern

- Display text blocks as normal text
- Show search results as reference list
- Highlight citations with source attribution (domain, title, URL, quoted text)

**Domain restriction example:** Restricting to `NIH.gov` for medical/exercise advice ensures scientifically-backed information vs generic web content.

---

## Key Takeaways

- Text Editor Tool provides the schema; you provide the file system implementation
- Web Search Tool requires zero implementation — schema only
- Domain restriction on web search is a powerful quality control mechanism
- Citations give users a way to verify Claude's sources directly

---

## Personal Notes

- The Text Editor Tool is essentially the Claude Code file operation layer exposed at the API level — useful for building custom coding assistants
- Domain-restricted web search is immediately applicable for any tool that needs authoritative sources (medical, legal, regulatory)
- Citations are a trust and transparency feature — important for any client-facing application

---

## Follow-Up Questions

- Can multiple web search tools with different domain restrictions be used in the same request?
- Does the Text Editor Tool support binary files or only text?
- How are citation blocks rendered in the API response — is it a separate content block type?
