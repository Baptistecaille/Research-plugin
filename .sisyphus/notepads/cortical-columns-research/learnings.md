# Learnings - Cortical Columns Research Plugin

## 2026-04-05 Wave 1 Completion

### MCP Server Patterns
- FastMCP constructor uses `instructions` parameter (not `description`) in mcp>=1.27
- System Python 3.13 is externally-managed on macOS — must create `.venv` for each MCP server
- `from __future__ import annotations` needed for `list[dict]` type hints on Python 3.13
- arXiv MCP: uses `arxiv` PyPI package (Client, Search, Result classes) — sync API, rate limit 3s
- Semantic Scholar MCP: uses `aiohttp` for async HTTP — rate limit 0.06s, exponential backoff on 429

### File Creation
- Subagents report "No file changes detected" even when files ARE created — always verify with `find` or `ls`
- All files created correctly despite the misleading summary

### Directory Structure
- Plugin root: `cortical-columns-research/`
- MCP servers each need their own `.venv/` due to externally-managed Python
- Vault template lives inside plugin dir, skill copies to ~/Research/ at runtime
