# Model Context Protocol: Advanced Topics

**Course:** Model Context Protocol: Advanced Topics
**Section:** All Sections
**Status:** ✅ Complete

---

## Objective

Master advanced MCP implementation patterns — sampling, notifications, roots, JSON message types, and transport mechanisms (STDIO and StreamableHTTP) for production MCP server development.

---

## Core MCP Features

### Sampling

A technique allowing MCP servers to **request LLM text generation from clients** instead of accessing LLMs directly.

**Architecture:**
```
Server creates message request
→ Client receives via sampling callback
→ Client calls LLM
→ Client returns generated text to server
```

**Why it matters:**
- Shifts LLM access responsibility from server to client
- Eliminates API key requirements from servers
- Prevents unauthorized token usage on public servers
- Removes authentication and cost management complexity from server implementations

**Primary use case:** Publicly accessible MCP servers that need LLM capabilities without direct LLM access or associated costs and security concerns.

---

### Log and Progress Notifications

Real-time feedback during tool execution — purely for UX enhancement, optional.

**Server side:**
- Tool functions receive a `context` argument automatically as the last parameter
- `context.info()` — sends log messages to client
- `context.report_progress()` — sends progress updates to client

**Client side:**
- Create a callback for logging statements
- Create a separate callback for progress updates
- Pass logging callback to client session
- Pass progress callback to `call_tool` function

**Key benefit:** Prevents user confusion about stalled or failed tool calls during long-running operations.

---

### Roots

A codified way for users to grant server access to specific files and folders.

**The problem without roots:** User says "convert bikin.mp4" but Claude can't locate the file without a full path. Requiring full paths is inconvenient.

**Solution — three tools:**
1. `ConvertVideo` — the original tool
2. `ReadDirectory` — lists files/folders in a directory
3. `ListRoots` — returns available roots

**Root** = a file or folder the user grants permission to access beforehand (via command line args when starting the server).

**Critical requirement:** Tools must check that accessed files/folders are within granted roots using an `is_path_allowed()` function. The MCP SDK does **not** automatically enforce root restrictions — the server developer must implement access checks manually.

**Two main benefits:**
1. **Permission control** — limits server access to authorized areas only
2. **Autonomous discovery** — Claude can search through available roots to find files without the user providing full paths

---

## Transports and Communication

### JSON Message Types

MCP communication = JSON messages between clients and servers.

**Message categories:**

| Category | Behavior | Examples |
|---|---|---|
| Request/Result pairs | Always come together | `call_tool_request` + `call_tool_result` |
| Notifications | Events that don't need responses | `progress_notification`, `logging_message_notification` |

**Message direction:**
- **Client messages** — sent by MCP client to server
- **Server messages** — sent by MCP server to client

> Key insight: Servers can send messages **to** clients. This bidirectional capability becomes the critical limitation in StreamableHTTP transport.

Schema defined in TypeScript (`schema.ts`) in the MCP spec repository — type descriptions only, not executable code.

---

### The STDIO Transport

Client launches server as a separate process and communicates via standard input/output streams.

**Mechanism:**
- Client writes to server's stdin, reads from server's stdout
- Server writes to stdout, reads from stdin

**Required initialization sequence:**
1. Initialize request (client → server)
2. Initialize result (server → client)
3. Initialized notification (client → server, no response required)

**Advantages:** Full bidirectional communication — either party can initiate requests at any time.

**Limitation:** Only works when client and server run on the **same physical machine**.

---

### The StreamableHTTP Transport

HTTP-based transport enabling remote server hosting — servers can be publicly accessible at URLs like `mcpserver.com`.

**Core problem:** MCP requires server-to-client requests (sampling, notifications, logging) but HTTP naturally supports only client-to-server requests.

**Workaround:** Uses Server-Sent Events (SSE) to allow servers to stream messages to clients.

**Session ID:** Random identifier assigned during initialization, included in all subsequent requests as an HTTP header.

**Initialization flow:**
1. Client sends initialize request
2. Server responds with result + MCP session ID header
3. Client sends initialized notification with session ID
4. Client optionally makes GET request with session ID to establish SSE connection

**Two SSE connection types:**

| Connection | Purpose |
|---|---|
| Long-lived SSE | Server-initiated requests (sampling, notifications) |
| Short-lived SSE | Specific tool call responses — auto-closes after result |

**Message routing:**
- Progress notifications → long-lived SSE connection
- Logging messages + tool results → short-lived SSE connection

---

### Stateless HTTP & JSON Response Flags

Two flags that significantly change StreamableHTTP server behavior:

**`stateless=true`** — Use when horizontally scaling across multiple instances with a load balancer.

Effects:
- No session IDs assigned
- Server cannot track individual clients
- GET SSE pathway disabled — server cannot send requests to client
- Eliminates sampling, progress logging, resource subscriptions
- No client initialization required

**`json_response=true`** — Disables streaming responses on POST requests.

Effects:
- POST responses return final result as plain JSON only
- No intermediate streaming messages
- No progress or log statements during execution
- Client waits for complete tool execution before receiving any response

> **Critical rule:** Use the same transport in development as planned for production. Applications that work locally with STDIO will fail when deployed with HTTP if server-to-client messaging is required.

---

## Key Takeaways

- Sampling shifts LLM cost and authentication responsibility from server to client — the right design for public MCP servers
- Root restrictions must be manually enforced — the SDK does not do this automatically
- STDIO transport = full bidirectional, same machine only; StreamableHTTP = remote hosting, with messaging constraints
- SSE is the workaround that enables server-to-client communication over HTTP
- Both stateless and JSON response flags break server-to-client messaging — understand their trade-offs before deploying

---

## Personal Notes

- The sampling architecture is a clean inversion of the typical LLM integration pattern — the server stays dumb, the client owns the intelligence layer. This is the right design for multi-tenant or public-facing MCP servers
- Root restrictions being developer-enforced (not SDK-enforced) is a security footgun — worth building a reusable `is_path_allowed()` utility and treating it as non-optional boilerplate
- The STDIO vs. StreamableHTTP trade-off maps directly to local vs. cloud deployment decisions I make in every data project — same principle, different stack

---

## Follow-Up Questions

- Can sampling be combined with streaming responses, or does it require full response before returning to the server?
- Is there a recommended pattern for implementing root access checks that's reusable across multiple tools?
- What's the recommended approach for handling StreamableHTTP in a serverless deployment (e.g., AWS Lambda)?

---

## Additional Links

- 🏅 [Certificate of Completion](http://verify.skilljar.com/c/oepzsprwhw8u)