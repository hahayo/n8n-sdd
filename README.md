# n8n-sdd

n8n workflow requirements analysis skill for AI coding assistants.

Turn automation ideas into actionable specs through structured conversation, then build the workflow.

## What it does

This skill guides users (including beginners with zero technical background) through a 3-phase process:

1. **Phase 1: Understand Requirements** - Interactive Q&A, one question at a time
2. **Phase 2: Feasibility Check** - Real-time node verification via `search_nodes` + capability check
3. **Phase 3: Output Summary** - Structured spec ready for workflow generation

## Prerequisites

This skill works best with [n8n-mcp](https://github.com/nerding-io/n8n-mcp-server) configured for real-time node lookup. Without it, Phase 2 feasibility checking falls back to general capability knowledge only.

---

## Installation

### Claude Code (CLI / Desktop / Web)

**Option A: As a project skill (recommended)**

Clone into your project's `.claude/skills/` directory:

```bash
cd your-project
mkdir -p .claude/skills
git clone https://github.com/hahayo/n8n-sdd.git .claude/skills/n8n-sdd
```

Claude Code auto-discovers skills in `.claude/skills/*/SKILL.md`.

**Option B: As a shared skill across projects**

Clone to your global Claude skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/hahayo/n8n-sdd.git ~/.claude/skills/n8n-sdd
```

Then reference it in your project's `CLAUDE.md`:

```markdown
## Skills
- See ~/.claude/skills/n8n-sdd/SKILL.md for n8n requirements analysis
```

### Codex App (codex.openai.com)

1. In your repository, create the skill directory:

```bash
mkdir -p .codex/skills/n8n-sdd
```

2. Copy the skill files into it:

```
.codex/skills/n8n-sdd/
  SKILL.md
  CONSTITUTION.md
  references/
    codex-tools.md
```

3. Reference in your `AGENTS.md` (Codex's equivalent of `CLAUDE.md`):

```markdown
## Skills

### n8n SDD - Requirements Analysis
When the user describes an automation need, follow the process in `.codex/skills/n8n-sdd/SKILL.md`.
Reference `.codex/skills/n8n-sdd/CONSTITUTION.md` for n8n capability checks.
See `.codex/skills/n8n-sdd/references/codex-tools.md` for tool mapping.
```

4. If using n8n-mcp, configure it in your Codex environment settings or MCP config.

### Codex CLI

1. Clone or copy skill files into your project:

```bash
mkdir -p .codex/skills
git clone https://github.com/hahayo/n8n-sdd.git .codex/skills/n8n-sdd
```

2. Add to `AGENTS.md`:

```markdown
## Skills

### n8n SDD
Follow `.codex/skills/n8n-sdd/SKILL.md` when users describe automation needs.
```

3. Configure n8n-mcp in your MCP config (if available):

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

1. Add skill files to your project:

```
.antigravity/skills/n8n-sdd/
  SKILL.md
  CONSTITUTION.md
  references/
    codex-tools.md
```

2. Reference in your project instructions file:

```markdown
## Skills

### n8n SDD - Requirements Analysis
When the user describes an automation need, follow the structured process in
`.antigravity/skills/n8n-sdd/SKILL.md`.
```

3. Configure n8n-mcp as an MCP server if your environment supports it.

### Other AI Assistants (Cursor, Windsurf, etc.)

The skill is plain Markdown — it works with any AI assistant that reads instruction files:

1. Copy the skill files into your project
2. Reference `SKILL.md` in your assistant's instruction/rules file
3. The assistant will follow the structured Q&A process

---

## n8n-mcp Configuration

For real-time node verification in Phase 2, configure [n8n-mcp](https://github.com/nerding-io/n8n-mcp-server):

### Environment Variables

| Variable | Description |
|----------|-------------|
| `N8N_API_URL` | Your n8n instance API URL (e.g., `https://n8n.example.com/api/v1`) |
| `N8N_API_KEY` | API key from n8n Settings > API |

### MCP Config Example (`.mcp.json`)

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

### Key MCP Tools Used

| Tool | Purpose |
|------|---------|
| `search_nodes` | Find nodes by keyword (800+ nodes including community) |
| `get_node` | Get node details, operations, and properties |
| `validate_node` | Validate node configuration |

---

## File Structure

```
n8n-sdd/
  SKILL.md              # Main skill definition (Phase 1-3 process)
  CONSTITUTION.md       # n8n capabilities, limitations, and best practices
  references/
    codex-tools.md      # Cross-platform tool mapping
```

## Usage

Once installed, the skill triggers when users describe automation needs:

- "I want to automate daily sales reports to Slack"
- "Build me a workflow that monitors Gmail and creates Todoist tasks"
- "Every morning, pull data from Google Sheets and send a summary"
- "Help me create a customer support chatbot"

The skill will guide the conversation through requirements gathering, feasibility checking, and spec generation.

## License

MIT
