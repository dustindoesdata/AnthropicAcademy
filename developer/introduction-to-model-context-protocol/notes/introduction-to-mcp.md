# Introduction to Model Context Protocol (MCP)

**Course:** Introduction to Model Context Protocol
**Section:** MCP Architecture
**Status:** ✅ Complete

---

## Objective

Understand what MCP is, how clients and servers communicate, and the three core server primitives — tools, resources, and prompts.

---

## What MCP Is

A communication layer that provides Claude with context and tools without requiring developers to manually write and maintain tool schemas and function implementations.

**Core architecture:**
```
MCP Client ↔ MCP Server (tools, resources, prompts)
```

**Problem solved:** Traditional approach requires developers to author tool schemas and functions for every service integration. For a complex service like GitHub, that's repositories, pull requests, issues, projects — significant ongoing maintenance burden.

**MCP solution:** Shifts tool definition and execution to a dedicated MCP server. Someone else authors the tools and packages them — your application just connects.

---

## MCP Clients

A communication interface between your server and the MCP server. Transport-agnostic — supports stdin/stdout, HTTP, WebSockets.

**Key message types:**

| Message | Direction | Purpose |
|---|---|---|
| `list_tools` request | Client → Server | Ask for available tools |
| `list_tools` result | Server → Client | Respond with tool list |
| `call_tool` request | Client → Server | Run tool with arguments |
| `call_tool` result | Server → Client | Return execution result |

**Full flow:**
1. User query arrives at server
2. Server asks MCP client for tools
3. Client sends `list_tools` to MCP server
4. Server sends query + tools to Claude
5. Claude requests tool execution
6. Client sends `call_tool` to MCP server
7. MCP server executes (e.g., GitHub API call)
8. Results flow back to Claude → user

**Client implementation:**
```python
list_tools()              # await self.session.list_tools()
call_tool(name, input)    # await self.session.call_tool(tool_name, tool_input)
```

> Wrap the client session in a larger class for resource cleanup management rather than using it directly.

---

## Defining Tools with MCP

The MCP Python SDK auto-generates JSON schemas from decorated functions — no manual schema writing.

```python
@mcp.tool(name="read_doc_contents", description="Reads document contents by ID")
def read_doc_contents(doc_id: str = Field(description="The document ID")) -> str:
    if doc_id not in docs:
        raise ValueError(f"Document {doc_id} not found")
    return docs[doc_id]
```

**Best practices:**
- Use `Field()` from pydantic for argument descriptions
- Validate existence before operations
- Raise `ValueError` with meaningful messages — Claude sees these and can retry with corrections

---

## Defining Resources

Resources expose data to clients for read operations. The **application** decides when to fetch them — not Claude.

**Two types:**
- **Direct/Static** — static URI: `docs://documents`
- **Templated** — parameterized: `docs://documents/{doc_id}`

```python
@mcp.resource(uri="docs://documents/{doc_id}", mime_type="application/json")
def get_document(doc_id: str) -> dict:
    return docs[doc_id]
```

**Client-side parsing:**
```python
result = await self.session.read_resource(AnyUrl(uri))
resource = result.contents[0]
if resource.mime_type == "application/json":
    return json.loads(resource.text)
return resource.text
```

---

## Defining Prompts

Pre-written, tested prompt templates that MCP servers expose to clients. The **user** triggers them — via slash commands, buttons, etc.

```python
@prompt(name="format", description="Rewrites document in markdown")
def format_document(doc_id: str) -> list:
    return [base.UserMessage(content=f"Read document {doc_id}, reformat to markdown, and save.")]
```

**Client functions:**
```python
list_prompts()                           # await self.session.list_prompts()
get_prompt(name, {"doc_id": "report"})  # await self.session.get_prompt(...)
```

**CLI pattern:** `/format` command → select document → server returns prompt → client sends to Claude → Claude uses tools to execute.

---

## The Three MCP Primitives — Summary

| Primitive | Controlled By | Purpose | Example |
|---|---|---|---|
| **Tools** | Model (Claude decides) | Add capabilities to Claude | JavaScript execution, API calls |
| **Resources** | Application (app decides) | Get data into app for UI or prompt augmentation | Document listings, Drive files |
| **Prompts** | User (triggered by action) | Predefined workflows | Chat starter buttons, slash commands |

> **Decision rule:** Need Claude capabilities → tools. Need app data → resources. Need user workflows → prompts.

---

## The MCP Inspector

An in-browser debugger for testing MCP servers during development.

```bash
mcp dev server_file.py
```

Navigate to the provided localhost URL → connect → test tools, resources, and prompts interactively before connecting to a full application.

---

## Key Takeaways

- MCP shifts tool creation burden from application developers to server maintainers
- The three primitives serve three different controllers: model, app, and user
- The Python SDK eliminates manual JSON schema writing entirely
- Resources are the right pattern for data access; tools are for actions
- The MCP Inspector is essential for development — use it before wiring up a full application

---

## Personal Notes

- The primitive control model (model/app/user) is the clearest mental model I've seen for deciding which MCP primitive to use — it maps directly to who initiates the action
- Resources for app-controlled data access is the pattern behind Claude's Google Drive integration — not a tool, because Claude doesn't decide when to fetch your files
- The MCP Inspector = Postman for MCP servers. Non-negotiable for development workflow.

---

## Follow-Up Questions

- Can a single MCP server expose all three primitive types simultaneously?
- How do MCP servers handle versioning when tool schemas change?
- Is there an official registry of publicly available MCP servers?
