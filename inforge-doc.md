# 🚀 Setting Up InsForge MCP with Claude (Debugging Journey)

## 👩‍💻 Context

I was trying to use **InsForge MCP (****fetch-docs****)** inside Claude Code, but it wasn’t working even though:

* MCP was configured in `.mcp.json`
* Claude kept saying tools weren’t loaded
* Restarting didn’t fix it

---

## ❌ Initial Errors

### 1. MCP Tools Not Available

Claude kept showing:

> “InsForge MCP server is configured but tools aren't loaded”

---

### 2. Backend Connection Error

When running:

```bash
npx -y @insforge/mcp@latest
```

Got:

```
ECONNREFUSED http://localhost:7130/api/health
```

👉 Meaning: MCP backend not reachable

---

### 3. VS Code Blocked by macOS

Error:

```
“Visual Studio Code is damaged and can’t be opened”
```

✔️ Fixed using:

```bash
sudo xattr -rd com.apple.quarantine /Applications/Visual\ Studio\ Code.app
```

---

## 🔍 Root Cause Discovery

Even after fixing environment issues:

👉 MCP was still not loading because:

* Config existed inside:

```json
"projects": {
  "/Users/devanshi/ias-management-2": {
    "mcpServers": { ... }
  }
}
```

❌ Problem:

* **Project-level MCP is not reliably auto-loaded by Claude**

---

## ✅ Final Fix

### Step 1: Move MCP to Global Config

Edited:

```bash
nano ~/.claude.json
```

---

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

---

### ⚠️ Important Lesson

* JSON must be **valid**
* Missing `}` → whole config ignored silently

---

### Step 3: Restart Claude

```bash
pkill -f claude
claude
```

---

## 🧪 Verification

Inside Claude:

```
What MCP tools are available?
```

✅ Output:

```
insforge__fetch-docs
```

---

## 🎉 Success

Finally able to run:

```
fetch-docs (docType: "instructions")
```

And got proper InsForge documentation.

---

## 🧠 Key Learnings

### 1. MCP Scope Matters

| Scope   | Behavior       |
| ------- | -------------- |
| Global  | ✅ Always loads |
| Project | ❌ Unreliable   |

---

### 2. JSON Must Be Perfect

* No missing brackets
* No trailing commas
* Proper nesting

---

### 3. Claude Fails Silently

* Invalid config → ignored without error
* Always verify tools manually

---

### 4. Correct Workflow with InsForge

Before writing any feature:

```
fetch-docs (docType: ...)
```

Examples:

* `db-sdk`
* `auth-sdk`
* `storage-sdk`
* `functions-sdk`
* `ai-integration-sdk`
* `real-time`

---

## 🔁 Final Working Flow

1. Start Claude
2. Ensure MCP tools exist
3. Call `fetch-docs`
4. Then write implementation

---

## 💯 Outcome

* MCP fully working ✅
* InsForge integrated ✅
* Claude + backend workflow ready ✅

---

## 🚀 Next Steps

* Build features using SDK + MCP combo
* Always fetch docs before implementation
* Use MCP for schema / infra changes

---

## 🧩 Summary

This wasn’t just a setup issue — it involved:

* macOS security (quarantine)
* CLI tool debugging
* JSON correctness
* MCP architecture understanding

👉 End result: Fully working AI-assisted backend system

---
