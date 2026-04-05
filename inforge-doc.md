# 🚀 Setting Up InsForge MCP with Claude (Debugging Journey)

## 👩‍💻 Context

I was trying to use **InsForge MCP (`fetch-docs`)** inside Claude Code, but it wasn’t working even though:

- MCP was configured in `.mcp.json` / `~/.claude.json`  
- Claude kept saying tools weren’t loaded  
- Restarting didn’t fix it

This doc records what broke, why, and what finally made it stable.

***

## ❌ Initial Errors

### 1. MCP Tools Not Available

Claude kept showing something like:

> “InsForge MCP server is configured but tools aren't loaded”

Even though the MCP server entry looked correct.

***

### 2. Backend Connection Error

When running:

```bash
npx -y @insforge/mcp@latest
```

I got:

```text
ECONNREFUSED http://localhost:7130/api/health
```

👉 Meaning: MCP backend not reachable (local server not running or wrong port).

**Checks to run:**

```bash
# Is anything listening on 7130?
lsof -i :7130

# Can I hit the health endpoint?
curl -v http://localhost:7130/api/health
```

If those fail, MCP can’t be reached, so Claude won’t see tools.

***

### 3. VS Code Blocked by macOS

Error:

```text
“Visual Studio Code is damaged and can’t be opened”
```

✔️ Fixed using:

```bash
sudo xattr -rd com.apple.quarantine /Applications/Visual\ Studio\ Code.app
```

macOS quarantine flags can silently block CLIs and editors that manage MCP config.

***

## 🔍 Root Cause Discovery

Even after fixing environment issues, MCP was still not loading because the config lived only under the **project** section:

```json
"projects": {
  "/Users/devanshi/ias-management-2": {
    "mcpServers": {
      "insforge": { ... }
    }
  }
}
```

❌ Problems discovered:

- Project-level MCP entries were not **consistently** loaded on every Claude start.  
- Any **invalid JSON** anywhere in `~/.claude.json` caused Claude to silently ignore parts of the config, including MCP.

***

## ✅ Final Fix

### Step 1: Move MCP to Global Config

Edited:

```bash
nano ~/.claude.json
```

Moved the MCP configuration to the **top-level** `mcpServers` object.

### Step 2: Add Global MCP Config

```json
{
  "mcpServers": {
    "insforge": {
      "type": "http",
      "url": "https://au446azm.us-east.insforge.app"
    }
  },
  "numStartups": 2,
  "installMethod": "native",
  "autoUpdates": false,
  "tipsHistory": {
    "new-user-warmup": 1
  }
}
```

👉 This makes InsForge available across all projects, not just one path.

### ⚠️ Important Lesson: JSON Must Be Perfect

- One missing `}` or `]` → Claude silently ignores parts of the file.  
- No trailing commas, no stray characters.  
- Always validate JSON before restarting Claude:

```bash
cat ~/.claude.json | jq . > /dev/null
# If this prints an error, fix JSON before trying again
```

***

### Step 3: Restart Claude Completely

```bash
pkill -f claude
claude
```

(Or quit Claude Desktop fully and reopen.)

A full restart was necessary so it re-reads the updated `~/.claude.json`.

***

## 🧪 Verification

### 1. CLI-Level Check

From the terminal:

```bash
claude mcp list
```

Expected to show:

```text
insforge: https://au446azm.us-east.insforge.app (HTTP) - ✗/✓
```

Note: The CLI health check may sometimes show a ✗ even when tools work inside chats; focus on in-chat behavior.

### 2. In-Chat Tool Check

Inside Claude:

```text
What MCP tools are available?
```

✅ Output included:

```text
insforge__fetch-docs
```

And running:

```text
fetch-docs (docType: "instructions")
```

returned proper InsForge documentation.

***

## 🧠 Key Learnings

### 1. MCP Scope Matters

| Scope     | Behavior                                                  |
|----------|------------------------------------------------------------|
| Global   | ✅ Always loaded for any folder / project                  |
| Project  | ⚠️ Can be brittle, easier to break or silently ignored     |

**Rule of thumb:**

- Put **core MCPs (like InsForge)** in global `mcpServers`.  
- Use project-level overrides only when you truly need different servers per repo.

***

### 2. JSON Must Be Perfect

- No missing brackets  
- No trailing commas  
- Validate via `jq` before restarting  
- Broken JSON = config ignored with **no explicit error message**

***

### 3. Claude’s Failure Mode

- Invalid or partial config is often **silent**.  
- Tools just don’t appear, without a clear error.  
- Always explicitly ask:

  ```text
  What MCP tools are available?
  ```

to verify tool registration after any config change.

***

### 4. Correct Workflow with InsForge

Before writing any feature:

```text
fetch-docs (docType: ...)
```

Useful `docType` values:

- `db-sdk`  
- `auth-sdk`  
- `storage-sdk`  
- `functions-sdk`  
- `ai-integration-sdk`  
- `real-time`

Then:

1. Read the SDK docs returned by `fetch-docs`.  
2. Let Claude propose schema / infra changes using those docs.  
3. Approve / edit changes, then run in your codebase.

***

## 🔁 Final Working Flow

1. Ensure global MCP config in `~/.claude.json` is valid.  
2. Restart Claude fully.  
3. Ask “What MCP tools are available?” and confirm `insforge__fetch-docs` is listed.  
4. Call `fetch-docs` with the right `docType`.  
5. Use the returned docs to drive implementation with Claude.

***

## 💯 Outcome

- MCP fully working ✅  
- InsForge integrated at a **global** level ✅  
- Claude + backend workflow ready for feature work ✅  
- Clear checklist for debugging future MCP issues ✅

***

## 🧩 Summary

This wasn’t just a setup issue — it involved:

- macOS security (quarantine)  
- Local MCP process health (`ECONNREFUSED`)  
- JSON correctness (`~/.claude.json`)  
- Difference between project vs global `mcpServers`  
- Understanding how Claude loads MCP and how it fails silently

👉 End result: a **repeatable** setup and debugging pattern for InsForge MCP with Claude.
