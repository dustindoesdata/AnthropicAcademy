# Model Context Protocol (MCP)

**Course:** Building with the Claude API
**Section:** MCP
**Status:** ✅ Complete

---

## Objective

Understand what MCP is, how clients and servers communicate, how to define tools/resources/prompts, and how to build a working MCP client-server system.

---

## What MCP Is

A communication layer providing Claude with context and tools without requiring developers to write and maintain tool schemas and functions manually.

**Problem solved:** Building a GitHub chatbot would require implementing tools for repositories, pull requests, issues, and projects — significant developer effort. MCP shifts that burden to the server maintainer.

**Architecture:**
```
MCP Client ↔ MCP Server (contains tools, resources, prompts)
```

**Who creates MCP servers?** Anyone — often service providers create official implementations (AWS, GitHub, etc.).

**MCP vs. tool use:** They are complementary. MCP handles *who* does the work (server vs. developer). Both still involve tools underneath.

---

## MCP Client

A communication interface between your server and the MCP server. Transport-agnostic — can use stdio, HTTP, or WebSockets.

**Key message types:**
| Message | Direction | Purpose |
|---|---|---|
| `list_tools` request | Client → Server | Ask for available tools |
| `list_tools` result | Server → Client | Respond with tool list |
| `call_tool` request | Client → Server | Ask server to run tool |
| `call_tool` result | Server → Client | Return tool execution result |

**Key functions:**
```python
list_tools()   # await self.session.list_tools()
call_tool()    # await self.session.call_tool(tool_name, tool_input)
```

**Typical full flow:**
1. User queries server
2. Server requests tool list from MCP client
3. Client sends `list_tools` to MCP server
4. Server responds with tool list
5. Server sends query + tools to Claude
6. Claude requests tool execution
7. Server asks client to run tool
8. Client sends `call_tool` to MCP server
9. MCP server executes (e.g., GitHub API call)
10. Results flow back through chain to user

---

## Defining Tools with MCP

The MCP Python SDK auto-generates JSON schemas from Python function definitions using decorators — no manual JSON schema writing.

```python
@mcp.tool(name="read_doc_contents", description="Reads document contents by ID")
def read_doc_contents(doc_id: str = Field(description="The document ID")) -> str:
    if doc_id not in docs:
        raise ValueError(f"Document {doc_id} not found")
    return docs[doc_id]
```

**Required imports:** `Field` from pydantic, `mcp` package for server and tool decorators.

---

## Defining Resources

Resources expose data to clients for read operations — used proactively (when a document is @mentioned) vs. tools which are called reactively.

**Two resource types:**
- **Direct** — static URI: `"docs://documents"`
- **Templated** — parameterized URI: `"docs://documents/{doc_id}"`

```python
@mcp.resource(uri="docs://documents/{doc_id}", mime_type="application/json")
def get_document(doc_id: str) -> dict:
    return docs[doc_id]
```

**MIME types** hint to the client about data format:
- `application/json` — structured data
- `text/plain` — plain text

**Client-side parsing:**
```python
if resource.mime_type == "application/json":
    return json.loads(resource.text)
return resource.text
```

---

## Defining Prompts

Pre-defined, tested prompt templates that MCP servers expose to client applications.

**Purpose:** Server authors create high-quality, evaluated prompts tailored to their domain — better results than user-generated ad-hoc prompts.

```python
@mcpserver.prompt(name="format_document", description="Reformats a document to markdown")
def format_document(document_id: str) -> list[base.UserMessage]:
    return [base.UserMessage(content=f"Read document {document_id} using tools, reformat to markdown, and save.")]
```

**Client integration:** Prompts appear as slash command autocomplete options, prompt user for required parameters, then execute the pre-built workflow.

**Client functions:**
```python
list_prompts()                              # await self.session.list_prompts()
get_prompt(prompt_name, arguments_dict)     # await self.session.get_prompt(...)
```

---

## The MCP Inspector

An in-browser debugger for testing MCP servers without connecting to a full application.

```bash
mcp dev server_file.py
```

Navigate to the provided URL → connect → test tools, resources, and prompts interactively.

---

## Key Takeaways

- MCP shifts integration burden from application developers to server maintainers
- Tools = reactive (Claude decides when to call); Resources = proactive (client fetches when needed); Prompts = pre-defined templates
- The Python SDK eliminates manual JSON schema writing — decorators handle it
- MCP Inspector is the fastest way to verify a server implementation before connecting it to an application

---

## Personal Notes

- MCP is the connectors layer I've been building manually — this is the standardized version
- Resources vs. Tools distinction is important: resources are data access, tools are actions — same separation of concerns I use in API design
- The MCP Inspector is the equivalent of Postman for MCP servers — essential for development workflows

---

## Follow-Up Questions

- Can MCP servers be deployed remotely (not just localhost)?
- Is there an official MCP server registry for finding community-built servers?
- How does MCP handle authentication for servers that wrap protected APIs?
