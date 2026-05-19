# Claude Ecosystem Overview (May 2026)

---

## 1. Claude Code  

### Latest Features & Versioning  
- The current stable CLI is **Claude Code v2.1.143** (May 2026) which adds stronger plugin dependency handling, projected context-cost display, and many Windows/macOS stability fixes【9】.  
- Recent releases (v2.1.39, v2.1.96, v2.1.129-141) focused on terminal rendering speed, fatal-error visibility, process-leak fixes, and extensive UI/VS Code integration polish【6】【8】【10】.

### Core Capabilities  
| Feature | Description |
|---|---|
| **Agentic coding** - sub-agents, custom slash commands, and tool loops are built-in. |
| **Prompt-engineering helpers** - `/context`, `/effort`, `/model` shortcuts; auto-detect workspace files. |
| **Security reviews** - `/security-review` command and GitHub-Actions integration for automated code security checks【7】. |
| **Plugin marketplace** - install third-party plugins or private registries via `/plugin marketplace add …`【14】【15】. |
| **Hooks** - deterministic lifecycle hooks (e.g., `PreToolUse`, `Stop`) for policy enforcement【39】【40】. |

### IDE & CLI Integrations  

| Platform | Installation & Invocation |
|---|---|
| **VS Code** (official extension) | `code --install-extension anthropic.claude-code` then open the Claude panel via **Cmd/Ctrl + Shift + P → “Claude Code: Open”**. The extension embeds the full Claude Code UI and shares settings with the CLI【14】【15】. |
| **JetBrains IDEs** (IntelliJ, PyCharm, etc.) | Install the **Claude Code** plugin from the JetBrains Marketplace, then run `claude` in the IDE’s integrated terminal or use the `/ide` command to link an external terminal session【16】. |
| **GitHub Codespaces** | After installing Claude Code locally (`npm i -g @anthropic/claude-code`), launch a Codespace, run `claude` in the terminal, and use the VS Code extension to interact inside the cloud workspace【12】. |
| **Terminal (CLI)** | Basic usage: `claude --model claude-opus-4-7 --max-tokens 1024 "Explain this function"`【4】. Model selection can be changed with `--model` flag; the latest Opus 4.7 model provides “step-change” reasoning for complex coding tasks【1】【7】. |

---

## 2. Claude Skills / Agent Skills  

### How Skills Work  
- A **Skill** is a folder containing a `SKILL.md` front-matter file that declares metadata (name, description, allowed-tools, model overrides) and the full instruction body. The front-matter is loaded into Claude’s system prompt; the body is streamed only when Claude decides the skill is relevant【25】【28】.  
- Permissions are enforced via the `allowed-tools` list; Claude will request user approval before executing any tool outside that list【25】.  
- Skills can be invoked **explicitly** (`/skill-name …`) or **implicitly** when Claude’s reasoning matches the skill’s description. Event-based triggers (e.g., `/hook …`) can also fire a skill via the hook system【41】.

### Packaging & Distribution  
| Packaging | Details |
|---|---|
| **Plugin/Marketplace JSON** | Each skill lives under a marketplace repo with a `.claude-plugin/marketplace.json` that lists plugins; individual skill folders contain a `plugin.json` and the `SKILL.md` file【35】【36】. |
| **GitHub Marketplace** | Official Anthropic marketplace (`anthropics/claude-code`) hosts ~600 pre-built skills; private repos can serve as custom marketplaces via the same JSON format【34】【37】. |
| **Docker / npm** | Skills may ship executable scripts (Python, Bash) that the skill’s `SKILL.md` can call; distribution can be via public npm packages or Docker images referenced in the plugin manifest【35】. |

### Minimal Skill Example  

```markdown
---
name: add-error-handling
description: "Add consistent try/catch blocks to all .py files in src/."
allowed-tools: "Bash(grep:), Bash(sed:), Write"
model: claude-opus-4-7
---
For each Python file under `src/`:
1. Insert a `try:` block around the main function body.
2. Add an `except Exception as e:` clause that logs `e`.
```

Invoke from Claude Code:  

```
/add-error-handling
```  

(Claude will load the skill, request permission to run `Bash`/`Write`, then apply the changes.)【25】【28】  

---

## 3. Connectors  

### Official Connectors (MCP Servers)  

| Connector | What Claude Can Access | Auth / Config |
|---|---|---|
| **Filesystem** | Read/write files, list directories | No auth needed for local server; optional token for remote deployment【44】 |
| **GitHub** | Repos, issues, PRs, file diffs | GitHub PAT stored in environment variable `GITHUB_TOKEN`【44】 |
| **PostgreSQL / SQLite** | Execute SQL queries, read tables | DB credentials in a YAML `mcp.yaml` file; TLS optional【45】 |
| **Google Drive / Docs** | List files, read/write Docs | OAuth 2.0 token (`GOOGLE_TOKEN`) passed to the MCP server【44】 |
| **Vector Stores (e.g., Pinecone, Milvus)** | Vector search, upsert | API key in env (`PINECONE_API_KEY`) and endpoint URL in config【45】 |

### Installation & Configuration (example: filesystem MCP)  

```bash
# Install the reference MCP server (Python package)
pip install anthropic-mcp-server   # (official SDK)

# Create a simple config (mcp.yaml)
cat > mcp.yaml <<EOF
server:
  type: filesystem
  root: /home/user/project
auth:
  token: "my-secret-token"
EOF

# Run the server locally
anthropic-mcp --config mcp.yaml --port 8000
```

Claude Code can now call the server with `file:///` URIs. For remote deployment, run the same server behind an HTTPS reverse proxy and supply the same token in the client config. Detailed steps are in the MCP docs【47】.

---

## 4. MCP (Model Context Protocol)  

### What MCP Is  
MCP is a **JSON-RPC 2.0** protocol that lets Claude discover and invoke external tools and resources at runtime. The model first asks the server for its **tool list** and **resource list**, then issues calls like `tools/exec` or `resources/read` during a turn【47】.

### Deployment Modes  

| Mode | Characteristics |
|---|---|
| **Local (stdio) server** | Launched as a child process; communication occurs over stdin/stdout, ideal for desktop apps (e.g., Claude Code on macOS)【47】. |
| **Remote HTTP server** | Listens on a TCP port with HTTPS; Claude sends JSON-RPC over HTTP, enabling cloud-hosted connectors (e.g., SaaS APIs)【44】. |

### Handshake & Safety  
1. **Handshake** - Claude sends `server/info` → server returns supported tools/resources.  
2. **Authorization** - Servers should enforce **TLS** and **Bearer-token** authentication; the token is passed in the `Authorization: Bearer <token>` header for HTTP, or as a command-line `--auth-token` flag for stdio servers【44】.  
3. **Sandboxing** - Run the MCP server in a restricted container or user namespace to limit file-system exposure; only expose the minimal `root` directory or specific DB credentials.

### Minimal HTTP MCP Example  

```python
# server.py (using the reference Python SDK)
from anthropic_mcp import MCPServer, FileSystemTool

server = MCPServer(port=8080, token="secure123")
server.register_tool(FileSystemTool(root="/data/project"))
server.start
```

Client request (Claude’s side is automatic, but a manual test can be done with `curl`):

```bash
curl -X POST  \
  -H "Authorization: Bearer secure123" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"resources/read","params":{"uri":"file:///data/project/readme.md"},"id":1}'
```

The response contains the file contents without consuming model context tokens. Full spec and security guidelines are in the official MCP documentation【47】【44】.

---

## 5. Claude Agents & Agent SDK  

### Available Capabilities  

| Capability | Description |
|---|---|
| **Sub-agents** - agents can spawn child agents for isolated tasks (e.g., a code-generation sub-agent). |
| **Lifecycle Hooks** - events such as `PreToolUse`, `PostToolUse`, `Stop`, `SessionStart` let you enforce policies or inject context【39】【40】. |
| **Checkpoints & Background Tasks** - agents can persist state between turns and run long-running background jobs via the `TaskCreate` / `TaskCompleted` flow【39】. |
| **Autonomy Levels** - `adaptive` (default for Opus 4.7), `enabled` with token budget, or `disabled` to force user approval【26】. |
| **Tool Use & Memory** - built-in tools (bash, read/write, web-fetch) and optional vector-store memory via MCP resources. |
| **Orchestration** - chain agents using the **Agent SDK** or combine with **hooks** to build event-driven workflows (e.g., run a security-review skill after each PR)【41】. |

### Getting Started (Python SDK “Hello-World”)  

```python
from anthropic import Anthropic
from anthropic.agent import Agent, Hook

client = Anthropic(api_key="YOUR_ANTHROPIC_KEY")

def log_stop(event):
    print("Agent finished:", event["reason"])

# Register a Stop hook
stop_hook = Hook(event="Stop", command="python -c 'import sys, json; print(json.dumps({\"decision\":\"allow\"}))'")
agent = Agent(
    client=client,
    model="claude-sonnet-4-6",
    hooks={"Stop": [stop_hook]}
)

response = agent.run("Write a short Python function that returns the factorial of n.")
print(response.content.text)
```

The `Stop` hook runs a tiny Python script that always returns an “allow” decision, demonstrating how hooks can programmatically approve tool calls. The SDK documentation, sample projects, and the latest release notes (April 2026 Opus 4.7 launch) are available on the Claude API docs site【4】【26】.

### Building a Custom Agent  

1. **Install the SDK** - `pip install anthropic-sdk` (or use the official client library).  
2. **Define hooks** in a JSON or Python dict (see `/hooks` docs).  
3. **Create sub-agents** via `agent.spawn_subagent(...)` for isolated workspaces.  
4. **Configure autonomy** with `thinking: {type:"adaptive"}` for Opus 4.7 or explicit budgets for Sonnet/Haiku【26】.

Full SDK guide: [1]【28】.  

---

**Quick Reference Links**  

- Claude Code release notes (v2.1.143): [2]【9】  
- VS Code extension marketplace: [3]【14】  
- Agent Skills overview: [1]【28】  
- MCP specification: [4]【47】  
- Official Agent SDK docs: [1]【28】

These sections capture the current state of Claude across Code, Skills, Connectors, MCP, and Agents, with concise examples and direct links to the authoritative Anthropic resources.

---

### Sources
- [1] https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- [2] https://code.claude.com/docs/en/changelog
- [3] https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code
- [4] https://www.anthropic.com/news/model-context-protocol
