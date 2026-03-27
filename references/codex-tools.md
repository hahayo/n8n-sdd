# Cross-Platform Tool Mapping

This skill is designed to work across Claude Code, Codex, and Antigravity. Below are the tool equivalents for each platform.

## Phase 1-3: No Special Tools Required

The entire SDD process (questions, feasibility, summary) is conversational. No platform-specific tools are needed.

## Phase 2 Feasibility Check: MCP Tool Equivalents

Phase 2 uses `search_nodes` and `get_node` to verify node availability in real-time.

### Claude Code
```
search_nodes(query="service name")     → Find matching nodes
get_node(nodeType="nodes-base.xxx")    → Get node details
```

### Codex / Antigravity
If n8n-mcp is configured as an MCP server in the project:
- Use the same MCP tool names — they are protocol-level, not platform-specific
- The MCP server handles the actual n8n API calls regardless of which client invokes it

If n8n-mcp is NOT available:
- Phase 2 feasibility check relies on CONSTITUTION.md general capabilities only
- Recommend setting up n8n-mcp for accurate node verification

## File Operations (for saving summary)

| Action | Claude Code | Codex | Antigravity |
|--------|-------------|-------|-------------|
| Write file | Write tool | write_file | write_file |
| Read file | Read tool | read_file | read_file |

## Key Principle

The skill's core value is in the structured conversation (Phase 1 + 3). Phase 2 accuracy depends on `search_nodes` — without MCP tools, feasibility checking is limited to CONSTITUTION.md general knowledge.
