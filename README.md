# n8n-sdd

給 AI 程式助手使用的 n8n 工作流需求分析 skill。

用結構化對話把自動化想法變成可執行的規格書，然後直接建立工作流。

## 功能說明

這個 skill 引導使用者（包括零技術背景的新手）完成 3 階段流程：

1. **Phase 1：理解需求** — 互動式 Q&A，一次問一個問題
2. **Phase 2：可行性檢查** — 透過 `search_nodes` 即時驗證節點 + 能力確認
3. **Phase 3：輸出摘要** — 結構化規格書，可直接用於工作流生成

## 前置條件

搭配 [n8n-mcp](https://github.com/nerding-io/n8n-mcp-server) 使用效果最佳，可即時查詢節點資料。未設定 n8n-mcp 時，Phase 2 可行性檢查會退回到一般能力知識判斷。

---

## 安裝方式

### Claude Code（CLI / 桌面版 / 網頁版）

**方案 A：專案 skill（推薦）**

Clone 到專案的 `.claude/skills/` 目錄：

```bash
cd your-project
mkdir -p .claude/skills
git clone https://github.com/hahayo/n8n-sdd.git .claude/skills/n8n-sdd
```

Claude Code 會自動發現 `.claude/skills/*/SKILL.md` 中的 skill。

**方案 B：跨專案共用 skill**

Clone 到全域 Claude skills 目錄：

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/hahayo/n8n-sdd.git ~/.claude/skills/n8n-sdd
```

然後在專案的 `CLAUDE.md` 中引用：

```markdown
## Skills
- 參見 ~/.claude/skills/n8n-sdd/SKILL.md 用於 n8n 需求分析
```

### Codex App（codex.openai.com）

1. 在你的 repository 中建立 skill 目錄：

```bash
mkdir -p .codex/skills/n8n-sdd
```

2. 將 skill 檔案複製進去：

```
.codex/skills/n8n-sdd/
  SKILL.md
  CONSTITUTION.md
  references/
    codex-tools.md
```

3. 在 `AGENTS.md`（Codex 的 `CLAUDE.md` 等價物）中引用：

```markdown
## Skills

### n8n SDD - 需求分析
當使用者描述自動化需求時，遵循 `.codex/skills/n8n-sdd/SKILL.md` 中的流程。
參考 `.codex/skills/n8n-sdd/CONSTITUTION.md` 進行 n8n 能力檢查。
工具對照表見 `.codex/skills/n8n-sdd/references/codex-tools.md`。
```

4. 若使用 n8n-mcp，請在 Codex 環境設定或 MCP 設定中配置。

### Codex CLI

1. Clone 或複製 skill 檔案到專案中：

```bash
mkdir -p .codex/skills
git clone https://github.com/hahayo/n8n-sdd.git .codex/skills/n8n-sdd
```

2. 加入 `AGENTS.md`：

```markdown
## Skills

### n8n SDD
當使用者描述自動化需求時，遵循 `.codex/skills/n8n-sdd/SKILL.md`。
```

3. 設定 n8n-mcp 的 MCP 配置（如可用）：

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["-y", "@nerding-io/n8n-mcp-server"],
      "env": {
        "N8N_API_URL": "https://your-n8n-instance.com/api/v1",
        "N8N_API_KEY": "your-api-key"
      }
    }
  }
}
```

### Antigravity

1. 將 skill 檔案加入專案：

```
.antigravity/skills/n8n-sdd/
  SKILL.md
  CONSTITUTION.md
  references/
    codex-tools.md
```

2. 在專案指令檔中引用：

```markdown
## Skills

### n8n SDD - 需求分析
當使用者描述自動化需求時，遵循
`.antigravity/skills/n8n-sdd/SKILL.md` 中的結構化流程。
```

3. 如環境支援，將 n8n-mcp 設定為 MCP 伺服器。

### 其他 AI 助手（Cursor、Windsurf 等）

本 skill 是純 Markdown，任何能讀取指令檔的 AI 助手都可以使用：

1. 將 skill 檔案複製到專案中
2. 在助手的 instruction/rules 檔中引用 `SKILL.md`
3. 助手會自動遵循結構化 Q&A 流程

---

## n8n-mcp 設定

Phase 2 的即時節點驗證需要設定 [n8n-mcp](https://github.com/nerding-io/n8n-mcp-server)：

### 環境變數

| 變數 | 說明 |
|------|------|
| `N8N_API_URL` | 你的 n8n 實例 API 網址（例如 `https://n8n.example.com/api/v1`） |
| `N8N_API_KEY` | 從 n8n 設定 > API 取得的 API 金鑰 |

### MCP 設定範例（`.mcp.json`）

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["-y", "@nerding-io/n8n-mcp-server"],
      "env": {
        "N8N_API_URL": "https://your-n8n-instance.com/api/v1",
        "N8N_API_KEY": "your-api-key"
      }
    }
  }
}
```

### 主要使用的 MCP 工具

| 工具 | 用途 |
|------|------|
| `search_nodes` | 用關鍵字搜尋節點（800+ 節點，含社群節點） |
| `get_node` | 取得節點詳情、操作與屬性 |
| `validate_node` | 驗證節點配置 |

---

## 檔案結構

```
n8n-sdd/
  SKILL.md              # 主要 skill 定義（Phase 1-3 流程）
  CONSTITUTION.md       # n8n 能力、限制與最佳實踐
  references/
    codex-tools.md      # 跨平台工具對照表
```

## 使用方式

安裝完成後，當使用者描述自動化需求時 skill 會自動觸發：

- 「我想自動化每日銷售報表發送到 Slack」
- 「幫我建一個監控 Gmail 並自動建立 Todoist 任務的工作流」
- 「每天早上從 Google Sheets 拉資料，發送摘要」
- 「幫我做一個客服聊天機器人」

Skill 會引導對話完成需求收集、可行性檢查和規格書生成。

## 授權

MIT
