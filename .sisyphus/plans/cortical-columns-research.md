# Cortical Columns Research Plugin — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Cowork plugin that discovers, analyzes, and organizes academic papers on cortical column algorithm representation, pushing findings to an Obsidian vault.

**Architecture:** Hybrid approach — MCP servers for arXiv and Semantic Scholar APIs provide structured paper data; skills orchestrate discovery → analysis → Obsidian note creation via direct file writes.

**Tech Stack:** Python 3.11+, `arxiv` PyPI package, `mcp` Python SDK (FastMCP), Markdown, Claude Cowork plugin system, Obsidian vault.

---

## Context

### Original Request
"I want to create a claude plugin for Cowork. It will be a research plugin to help me in my thesis, analysing papers and find new relevant ones. My thesis is about the algorithm representation of the cortical columns."

### Interview Summary
- **Paper sources**: Google Scholar + arXiv (mainly arXiv)
- **Analysis depth**: Two-tier — quick scan (abstract + relevance score) → deep analysis on flagged papers
- **Thesis topic**: Algorithm representation of cortical columns (computational neuroscience × AI architecture)
- **Starting point**: From scratch — needs discovery engine for foundational + latest papers
- **Storage**: Obsidian vault at `~/Research/cortical-columns/`
- **Workflows**: All four needed — find papers, analyze paper, periodic scan, literature mapping
- **Citation tracking**: Nice to have — Obsidian backlinks sufficient
- **Approach**: Hybrid (MCP servers for APIs, direct file writes to Obsidian)
- **Language**: English analysis, French summaries
- **Vault**: Not yet created — plugin should scaffold it

### Research Findings
- **arXiv API**: `https://export.arxiv.org/api/query`, `arxiv` PyPI package (Client, Search, Result), rate limit 1 req/3s
- **Semantic Scholar**: Graph API at `https://api.semanticscholar.org/graph/v1`, Recommendations API, 5000 req/5min unauthenticated
- **Plugin structure**: `.claude-plugin/plugin.json`, `skills/*/SKILL.md`, `agents/*.md`, `commands/*.md`, `.mcp.json`
- **Official examples**: anthropics/knowledge-work-plugins repo

---

## Work Objectives

### Core Objective
Build a complete Cowork plugin with 4 skills, 3 agents, 2 MCP servers, and Obsidian vault scaffolding for thesis research on cortical columns.

### Concrete Deliverables
- Plugin directory: `cortical-columns-research/` with all components
- arXiv MCP server with 4 tools
- Semantic Scholar MCP server with 3 tools
- 4 skills: find-papers, analyze-paper, new-papers, map-literature
- 3 agents: paper-scout, paper-analyst, literature-mapper
- 1 command: /research
- Obsidian vault scaffolding at `~/Research/cortical-columns/`

### Definition of Done
- [ ] Plugin validates with `claude plugin validate`
- [ ] All MCP servers start and respond to tool calls
- [ ] All 4 skills execute end-to-end with Obsidian notes created
- [ ] Vault structure created with correct folders and templates

### Must Have
- arXiv search with relevance scoring
- Two-tier analysis (quick scan + deep dive)
- Obsidian vault auto-creation
- French summaries for quick scans
- Duplicate detection by arXiv ID

### Must NOT Have (Guardrails)
- No Google Scholar MCP (too complex — rely on arXiv + Semantic Scholar coverage)
- No PDF download in quick scan (abstract-only for speed)
- No continuous background monitoring (Cowork doesn't support it)
- No citation graph visualization (Obsidian backlinks suffice)
- No over-abstraction — keep MCP servers simple, skills focused

---

## Verification Strategy

### Test Decision
- **Infrastructure exists**: NO (greenfield plugin project)
- **Automated tests**: None (plugin skills are LLM-driven, not unit-testable in traditional sense)
- **Agent-Executed QA**: ALL tasks verified by running the actual deliverable

### QA Policy
- **MCP servers**: Bash (python) — start server, call tools via MCP CLI, assert JSON responses
- **Skills/Agents**: Read files, validate structure, verify all required sections present
- **Obsidian vault**: Bash (ls, cat) — verify directory structure, file contents, YAML frontmatter
- **Plugin manifest**: Bash (claude plugin validate) — validate schema

---

## Execution Strategy

### Parallel Execution Waves

```
Wave 1 (Start Immediately — scaffolding + foundation):
├── Task 1: Plugin manifest + directory structure [quick]
├── Task 2: Obsidian vault scaffolding + templates [quick]
├── Task 3: arXiv MCP server [unspecified-high]
└── Task 4: Semantic Scholar MCP server [unspecified-high]

Wave 2 (After Wave 1 — agents + skills):
├── Task 5: paper-scout agent [quick]
├── Task 6: paper-analyst agent [quick]
├── Task 7: literature-mapper agent [quick]
├── Task 8: find-papers skill [deep]
└── Task 9: analyze-paper skill [deep]

Wave 3 (After Wave 2 — remaining skills + command):
├── Task 10: new-papers skill [unspecified-high]
├── Task 11: map-literature skill [unspecified-high]
├── Task 12: /research command [quick]
└── Task 13: .mcp.json + settings.json [quick]

Wave FINAL (After ALL tasks — 4 parallel reviews, then user okay):
├── Task F1: Plan compliance audit (oracle)
├── Task F2: Code quality review (unspecified-high)
├── Task F3: Real manual QA (unspecified-high)
└── Task F4: Scope fidelity check (deep)
-> Present results -> Get explicit user okay
```

### Dependency Matrix
- **1-4**: None — all start immediately
- **5-7**: Depends on 1 (need plugin structure)
- **8-9**: Depends on 3, 4, 5, 6 (need MCP servers + agents)
- **10-11**: Depends on 3, 4, 7 (need MCP servers + literature-mapper)
- **12-13**: Depends on 1, 8-11 (need all skills + structure)

### Agent Dispatch Summary
- **Wave 1**: 4 tasks — T1-T2 → `quick`, T3-T4 → `unspecified-high`
- **Wave 2**: 5 tasks — T5-T7 → `quick`, T8-T9 → `deep`
- **Wave 3**: 4 tasks — T10-T11 → `unspecified-high`, T12-T13 → `quick`
- **FINAL**: 4 tasks — F1 → `oracle`, F2 → `unspecified-high`, F3 → `unspecified-high`, F4 → `deep`

---

## TODOs

- [x] 1. Plugin manifest + directory structure

  **What to do**:
  - Create the plugin root directory: `cortical-columns-research/`
  - Create `.claude-plugin/plugin.json` with manifest (name, version, description, author)
  - Create all subdirectories: `skills/`, `agents/`, `commands/`, `mcp-servers/arxiv/`, `mcp-servers/semantic-scholar/`
  - Create `README.md` with plugin overview

  **Must NOT do**:
  - Do not create skill/agent/command files yet (separate tasks)
  - Do not write MCP server code yet (separate tasks)

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`
  - **Skills Evaluated but Omitted**: None needed — simple file creation

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 2, 3, 4)
  - **Blocks**: Tasks 5-13 (all depend on directory structure)
  - **Blocked By**: None

  **References**:
  - Pattern: `https://github.com/anthropics/knowledge-work-plugins/blob/main/cowork-plugin-management/.claude-plugin/plugin.json` — official plugin.json example
  - Schema docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/api/plugin-json` — plugin manifest fields
  - Structure: `https://github.com/anthropics/knowledge-work-plugins/blob/main/README.md` — standard plugin layout

  **Acceptance Criteria**:
  - [ ] `cortical-columns-research/.claude-plugin/plugin.json` exists with valid JSON
  - [ ] All directories created: skills/, agents/, commands/, mcp-servers/arxiv/, mcp-servers/semantic-scholar/
  - [ ] `README.md` exists with plugin description

  **QA Scenarios**:
  ```
  Scenario: Validate plugin manifest
    Tool: Bash (python -c)
    Preconditions: plugin.json created
    Steps:
      1. python -c "import json; data=json.load(open('cortical-columns-research/.claude-plugin/plugin.json')); assert data['name']=='cortical-columns-research'; assert 'version' in data; assert 'description' in data; assert 'author' in data"
    Expected Result: No assertion errors, valid JSON with all required fields
    Evidence: .sisyphus/evidence/task-1-manifest-valid.json

  Scenario: Verify directory structure
    Tool: Bash (ls)
    Steps:
      1. ls -R cortical-columns-research/
    Expected Result: Lists all expected directories (skills/, agents/, commands/, mcp-servers/arxiv/, mcp-servers/semantic-scholar/)
    Evidence: .sisyphus/evidence/task-1-directory-tree.txt
  ```

  **Evidence to Capture**:
  - [ ] plugin.json content
  - [ ] Directory tree output

  **Commit**: YES (groups with 2)
  - Message: `feat(plugin): scaffold cortical-columns-research plugin structure`
  - Files: `cortical-columns-research/**/*`

---

- [x] 2. Obsidian vault scaffolding + templates

  **What to do**:
  - Create vault directory structure at a template path: `vault-template/` (will be copied to `~/Research/cortical-columns/` by skill)
  - Create folders: `00 - Inbox/`, `01 - Foundational Papers/`, `02 - Recent Discoveries/`, `03 - Concepts/`, `04 - Literature Map/`, `05 - Thesis Notes/`
  - Create `.obsidian/workspace.json` minimal config
  - Create quick-scan template: `vault-template/_templates/quick-scan.md`
  - Create deep-analysis template: `vault-template/_templates/deep-analysis.md`
  - Create initial concept notes: `cortical-columns.md`, `predictive-coding.md`, `sparse-distributed-representations.md` in `03 - Concepts/`

  **Must NOT do**:
  - Do not copy to ~/Research/ during build (skill handles this at runtime)
  - Do not create actual Obsidian app config files

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 3, 4)
  - **Blocks**: Tasks 8-11 (skills reference templates)
  - **Blocked By**: None

  **References**:
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md` — vault structure and templates

  **Acceptance Criteria**:
  - [ ] All 6 vault folders created under `vault-template/`
  - [ ] Quick-scan template has YAML frontmatter with: title, authors, year, arxiv_id, source, relevance_score, status, tags, date_added
  - [ ] Deep-analysis template extends quick-scan with: analysis_date, methodology_quality, methodology critique section, French summary field
  - [ ] 3 initial concept notes created with basic content

  **QA Scenarios**:
  ```
  Scenario: Verify vault structure
    Tool: Bash (find)
    Steps:
      1. find cortical-columns-research/vault-template/ -type d | sort
    Expected Result: Lists all 6 folders + .obsidian + _templates
    Evidence: .sisyphus/evidence/task-2-vault-dirs.txt

  Scenario: Verify quick-scan template frontmatter
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/vault-template/_templates/quick-scan.md').read()
required = ['title:', 'authors:', 'year:', 'arxiv_id:', 'relevance_score:', 'status:', 'tags:', 'date_added:']
for field in required:
    assert field in content, f'Missing {field}'
print('All frontmatter fields present')
"
    Expected Result: "All frontmatter fields present"
    Evidence: .sisyphus/evidence/task-2-quickscan-frontmatter.txt

  Scenario: Verify deep-analysis template has French summary
    Tool: Bash (grep)
    Steps:
      1. grep -c "Résumé en français\|French summary" cortical-columns-research/vault-template/_templates/deep-analysis.md
    Expected Result: Count >= 1
    Evidence: .sisyphus/evidence/task-2-deepanalysis-french.txt
  ```

  **Evidence to Capture**:
  - [ ] Vault directory tree
  - [ ] Template file contents (quick-scan, deep-analysis)
  - [ ] Concept note contents

  **Commit**: YES (groups with 1)

---

- [x] 3. arXiv MCP server

  **What to do**:
  - Create `mcp-servers/arxiv/server.py` using FastMCP from `mcp` Python SDK
  - Create `mcp-servers/arxiv/requirements.txt` with dependencies: `mcp`, `arxiv`
  - Implement 4 tools:
    1. `search_papers(query: str, max_results: int = 10, sort_by: str = "relevance") -> list[dict]` — uses arxiv.Client + arxiv.Search
    2. `get_paper_details(arxiv_id: str) -> dict` — fetches single paper metadata
    3. `get_recent_papers(category: str = "cs.AI", days: int = 7) -> list[dict]` — recent papers by category
    4. `search_by_author(author_name: str, max_results: int = 10) -> list[dict]` — papers by author
  - Each tool returns structured dict with: paperId, title, authors, abstract, published, updated, categories, pdf_url, arxiv_id
  - Rate limiting: 3-second delay between requests (arXiv ToS)
  - Error handling: retry on network errors, graceful fallback on API failures

  **Must NOT do**:
  - Do not download PDFs in MCP server (skill handles this)
  - Do not implement Google Scholar (out of scope)
  - Do not add caching (keep it simple)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 2, 4)
  - **Blocks**: Tasks 8-11 (skills call arXiv tools)
  - **Blocked By**: None

  **References**:
  - arXiv API: `https://export.arxiv.org/api/query` — endpoint
  - arxiv PyPI: `https://pypi.org/project/arxiv/` — Client, Search, Result classes
  - MCP SDK: `https://github.com/modelcontextprotocol/python-sdk/blob/main/examples/snippets/servers/fastmcp_quickstart.py` — FastMCP pattern
  - MCP tool example: `https://github.com/modelcontextprotocol/python-sdk/blob/main/examples/snippets/servers/basic_tool.py`
  - Rate limit: 1 req/3s per arXiv Terms of Use

  **Acceptance Criteria**:
  - [ ] `server.py` runs without import errors: `python server.py` starts MCP server
  - [ ] `requirements.txt` lists all dependencies
  - [ ] All 4 tools defined and callable
  - [ ] Rate limiting implemented (3s delay)
  - [ ] Error handling for network failures

  **QA Scenarios**:
  ```
  Scenario: Start arXiv MCP server
    Tool: Bash (python)
    Preconditions: pip install -r requirements.txt
    Steps:
      1. cd cortical-columns-research/mcp-servers/arxiv/
      2. python -c "from server import mcp; print('Server loaded successfully')"
    Expected Result: "Server loaded successfully" with no import errors
    Evidence: .sisyphus/evidence/task-3-arxiv-server-load.txt

  Scenario: Test search_papers tool returns results
    Tool: Bash (python)
    Steps:
      1. cd cortical-columns-research/mcp-servers/arxiv/
      2. python -c "
import asyncio
from server import search_papers
async def test():
    results = await search_papers('cortical columns', max_results=3)
    assert len(results) > 0, 'No results returned'
    for r in results:
        assert 'title' in r, 'Missing title'
        assert 'authors' in r, 'Missing authors'
        assert 'arxiv_id' in r, 'Missing arxiv_id'
    print(f'Found {len(results)} papers')
asyncio.run(test())
"
    Expected Result: "Found 3 papers" (or similar), no assertion errors
    Evidence: .sisyphus/evidence/task-3-arxiv-search-results.txt

  Scenario: Test get_paper_details with known paper
    Tool: Bash (python)
    Steps:
      1. python -c "
import asyncio
from server import get_paper_details
async def test():
    result = await get_paper_details('2001.00001')
    assert 'title' in result
    assert 'arxiv_id' in result
    print(f'Paper: {result[\"title\"]}')
asyncio.run(test())
"
    Expected Result: Paper title printed, no errors
    Evidence: .sisyphus/evidence/task-3-arxiv-paper-details.txt

  Scenario: Test error handling with invalid query
    Tool: Bash (python)
    Steps:
      1. python -c "
import asyncio
from server import search_papers
async def test():
    results = await search_papers('', max_results=1)
    # Should return empty or handle gracefully, not crash
    print(f'Results for empty query: {len(results)}')
asyncio.run(test())
"
    Expected Result: Graceful handling, no uncaught exceptions
    Evidence: .sisyphus/evidence/task-3-arxiv-error-handling.txt
  ```

  **Evidence to Capture**:
  - [ ] Server startup output
  - [ ] Search results JSON
  - [ ] Paper details output
  - [ ] Error handling output

  **Commit**: YES (groups with 4)
  - Message: `feat(mcp): add arXiv and Semantic Scholar MCP servers`

---

- [x] 4. Semantic Scholar MCP server

  **What to do**:
  - Create `mcp-servers/semantic-scholar/server.py` using FastMCP
  - Create `mcp-servers/semantic-scholar/requirements.txt` with: `mcp`, `aiohttp`
  - Implement 3 tools:
    1. `get_related_papers(paper_id: str, limit: int = 10) -> list[dict]` — uses Recommendations API `POST /recommendations/v1/papers/`
    2. `get_citation_count(paper_id: str) -> int` — uses Graph API `GET /paper/{paper_id}?fields=citationCount`
    3. `get_author_papers(author_name: str, max_results: int = 10) -> list[dict]` — uses Graph API `GET /author/search` then `/author/{id}/papers`
  - Base URL: `https://api.semanticscholar.org/graph/v1`
  - Rate limiting: respect 5000 req/5min unauthenticated limit (0.06s between requests)
  - Error handling: HTTP error codes, retry on 429, graceful fallback

  **Must NOT do**:
  - Do not require API key (unauthenticated by default)
  - Do not implement full paper search (arXiv handles discovery)
  - Do not implement paper download

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 2, 3)
  - **Blocks**: Tasks 8-11 (skills call Semantic Scholar tools)
  - **Blocked By**: None

  **References**:
  - Graph API: `https://api.semanticscholar.org/graph/v1`
  - Recommendations API: `https://api.semanticscholar.org/recommendations/v1`
  - Reference implementation: `https://github.com/akapet00/semantic-scholar-mcp/blob/main/src/semantic_scholar_mcp/tools/papers.py`
  - Models: `https://github.com/akapet00/semantic-scholar-mcp/blob/main/src/semantic_scholar_mcp/models.py`
  - Server wiring: `https://github.com/akapet00/semantic-scholar-mcp/blob/main/src/semantic_scholar_mcp/server.py`

  **Acceptance Criteria**:
  - [ ] `server.py` runs without import errors
  - [ ] `requirements.txt` lists all dependencies
  - [ ] All 3 tools defined and callable
  - [ ] Rate limiting implemented
  - [ ] Error handling for HTTP failures

  **QA Scenarios**:
  ```
  Scenario: Start Semantic Scholar MCP server
    Tool: Bash (python)
    Steps:
      1. cd cortical-columns-research/mcp-servers/semantic-scholar/
      2. python -c "from server import mcp; print('Server loaded successfully')"
    Expected Result: "Server loaded successfully"
    Evidence: .sisyphus/evidence/task-4-ss-server-load.txt

  Scenario: Test get_citation_count returns data
    Tool: Bash (python)
    Steps:
      1. python -c "
import asyncio
from server import get_citation_count
async def test():
    count = await get_citation_count('ARXIV:1706.03762')
    assert isinstance(count, int), f'Expected int, got {type(count)}'
    assert count > 0, 'Citation count should be positive'
    print(f'Citation count: {count}')
asyncio.run(test())
"
    Expected Result: "Citation count: <number>" > 0
    Evidence: .sisyphus/evidence/task-4-ss-citation-count.txt

  Scenario: Test get_related_papers returns results
    Tool: Bash (python)
    Steps:
      1. python -c "
import asyncio
from server import get_related_papers
async def test():
    results = await get_related_papers('ARXIV:1706.03762', limit=5)
    assert len(results) > 0, 'No related papers found'
    for r in results:
        assert 'title' in r
    print(f'Found {len(results)} related papers')
asyncio.run(test())
"
    Expected Result: "Found N related papers"
    Evidence: .sisyphus/evidence/task-4-ss-related-papers.txt

  Scenario: Test error handling with invalid paper ID
    Tool: Bash (python)
    Steps:
      1. python -c "
import asyncio
from server import get_citation_count
async def test():
    result = await get_citation_count('INVALID-ID-12345')
    print(f'Result for invalid ID: {result}')
asyncio.run(test())
"
    Expected Result: Graceful handling, no uncaught exceptions
    Evidence: .sisyphus/evidence/task-4-ss-error-handling.txt
  ```

  **Evidence to Capture**:
  - [ ] Server startup output
  - [ ] Citation count result
  - [ ] Related papers result
  - [ ] Error handling output

  **Commit**: YES (groups with 3)

---

- [x] 5. paper-scout agent

  **What to do**:
  - Create `agents/paper-scout.md` with frontmatter and system prompt
  - Frontmatter: name, description, model (sonnet), effort (medium), maxTurns (15)
  - System prompt: Fast paper discovery specialist — searches arXiv, scores relevance 1-10 for cortical columns thesis, returns structured results
  - Expertise: evaluating paper relevance to computational neuroscience, cortical column models, HTM, predictive coding
  - Output format: JSON-like structured list with paperId, title, authors, abstract, relevance_score (1-10), relevance_reason

  **Must NOT do**:
  - Do not include deep analysis (that's paper-analyst's job)
  - Do not write files to disk (skill orchestrates this)

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 6, 7)
  - **Blocks**: Task 8 (find-papers skill uses paper-scout)
  - **Blocked By**: Task 1 (needs plugin structure)

  **References**:
  - Agent structure: `https://github.com/anthropics/knowledge-work-plugins/blob/main/partner-built/brand-voice/skills/discover-brand/SKILL.md` — agent coordination pattern
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md`

  **Acceptance Criteria**:
  - [ ] `agents/paper-scout.md` exists with valid YAML frontmatter
  - [ ] Frontmatter includes: name, description, model, effort, maxTurns
  - [ ] System prompt defines role, expertise, output format
  - [ ] Output format specifies all required fields

  **QA Scenarios**:
  ```
  Scenario: Validate agent frontmatter
    Tool: Bash (python -c)
    Steps:
      1. python -c "
import re
content = open('cortical-columns-research/agents/paper-scout.md').read()
# Check frontmatter block
match = re.match(r'---\n(.*?)\n---', content, re.DOTALL)
assert match, 'Missing frontmatter delimiters'
fm = match.group(1)
for field in ['name:', 'description:', 'model:', 'effort:', 'maxTurns:']:
    assert field in fm, f'Missing {field}'
print('Frontmatter valid')
"
    Expected Result: "Frontmatter valid"
    Evidence: .sisyphus/evidence/task-5-scout-frontmatter.txt

  Scenario: Verify agent has output format specification
    Tool: Bash (grep)
    Steps:
      1. grep -c "relevance_score\|output format\|structured" cortical-columns-research/agents/paper-scout.md
    Expected Result: Count >= 2
    Evidence: .sisyphus/evidence/task-5-scout-output-format.txt
  ```

  **Evidence to Capture**:
  - [ ] Agent file content
  - [ ] Frontmatter validation

  **Commit**: YES (groups with 6, 7)
  - Message: `feat(agents): add paper-scout, paper-analyst, literature-mapper agents`

---

- [x] 6. paper-analyst agent

  **What to do**:
  - Create `agents/paper-analyst.md` with frontmatter and system prompt
  - Frontmatter: name, description, model (sonnet), effort (high), maxTurns (25)
  - System prompt: Deep methodology critique specialist — analyzes full papers, evaluates strengths/weaknesses, maps connections to cortical columns thesis
  - Expertise: methodology evaluation, identifying assumptions, comparing approaches to canonical cortical column models
  - Output format: Structured analysis with sections: Methodology Critique, Strengths, Weaknesses, Assumptions, Connection to Cortical Columns, Key Findings, French Summary
  - Language: Analysis in English, summary in French

  **Must NOT do**:
  - Do not search for papers (that's paper-scout's job)
  - Do not write files to disk (skill orchestrates this)

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 5, 7)
  - **Blocks**: Task 9 (analyze-paper skill uses paper-analyst)
  - **Blocked By**: Task 1

  **References**:
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md`

  **Acceptance Criteria**:
  - [ ] `agents/paper-analyst.md` exists with valid frontmatter
  - [ ] System prompt includes methodology critique instructions
  - [ ] Output format includes all required sections
  - [ ] French summary section specified

  **QA Scenarios**:
  ```
  Scenario: Validate agent frontmatter and content
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/agents/paper-analyst.md').read()
assert '---' in content, 'Missing frontmatter'
assert 'methodology' in content.lower(), 'Missing methodology focus'
assert 'cortical' in content.lower(), 'Missing thesis connection'
assert 'fran' in content.lower() or 'french' in content.lower(), 'Missing French summary instruction'
print('Agent valid')
"
    Expected Result: "Agent valid"
    Evidence: .sisyphus/evidence/task-6-analyst-validation.txt
  ```

  **Evidence to Capture**:
  - [ ] Agent file content

  **Commit**: YES (groups with 5, 7)

---

- [x] 7. literature-mapper agent

  **What to do**:
  - Create `agents/literature-mapper.md` with frontmatter and system prompt
  - Frontmatter: name, description, model (sonnet), effort (high), maxTurns (20)
  - System prompt: Literature gap analysis specialist — reads existing Obsidian vault, identifies coverage gaps in cortical column research, suggests papers to fill gaps
  - Expertise: mapping research landscape, identifying underexplored areas, suggesting search queries
  - Output format: Structured gap analysis with sections: Current Coverage, Identified Gaps, Suggested Papers, Recommended Search Queries, Synthesis Notes

  **Must NOT do**:
  - Do not search external APIs directly (skill orchestrates this)
  - Do not write files to disk

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 5, 6)
  - **Blocks**: Tasks 10, 11 (new-papers, map-literature skills use this)
  - **Blocked By**: Task 1

  **References**:
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md`

  **Acceptance Criteria**:
  - [ ] `agents/literature-mapper.md` exists with valid frontmatter
  - [ ] System prompt includes gap analysis instructions
  - [ ] Output format includes all required sections

  **QA Scenarios**:
  ```
  Scenario: Validate agent frontmatter and content
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/agents/literature-mapper.md').read()
assert '---' in content
assert 'gap' in content.lower(), 'Missing gap analysis'
assert 'vault' in content.lower() or 'obsidian' in content.lower(), 'Missing vault reference'
print('Agent valid')
"
    Expected Result: "Agent valid"
    Evidence: .sisyphus/evidence/task-7-mapper-validation.txt
  ```

  **Evidence to Capture**:
  - [ ] Agent file content

  **Commit**: YES (groups with 5, 6)

---

- [x] 8. find-papers skill

  **What to do**:
  - Create `skills/find-papers/SKILL.md` — the main discovery workflow
  - Create `skills/find-papers/templates/quick-scan.md` — output template for quick scan notes
  - Skill workflow:
    1. User provides search query (topic/keywords)
    2. Invoke paper-scout agent to search arXiv via MCP
    3. Quick scan each result: title, abstract, relevance score 1-10
    4. Filter papers with score >= 7
    5. Enrich with Semantic Scholar (related papers, citation count)
    6. Create markdown notes in `00 - Inbox/` folder
    7. Output French summary to user
  - Must handle: empty results, API errors, duplicate detection by arXiv ID
  - Must create Obsidian vault if it doesn't exist at `~/Research/cortical-columns/`

  **Must NOT do**:
  - Do not download PDFs (quick scan only)
  - Do not move papers to Foundational Papers (user decides later)

  **Recommended Agent Profile**:
  - **Category**: `deep`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Task 9)
  - **Blocks**: Task 12 (/research command references this)
  - **Blocked By**: Tasks 1, 2, 3, 4, 5

  **References**:
  - SKILL.md format: `https://github.com/anthropics/knowledge-work-plugins/blob/main/partner-built/brand-voice/skills/brand-voice-enforcement/SKILL.md`
  - arXiv MCP: Task 3 output
  - Semantic Scholar MCP: Task 4 output
  - paper-scout agent: Task 5 output
  - Vault templates: Task 2 output

  **Acceptance Criteria**:
  - [ ] `skills/find-papers/SKILL.md` exists with complete workflow
  - [ ] Quick-scan template matches vault template structure
  - [ ] Skill references all MCP tools correctly
  - [ ] Skill references paper-scout agent
  - [ ] Duplicate detection logic included
  - [ ] French summary output specified

  **QA Scenarios**:
  ```
  Scenario: Validate skill structure
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/skills/find-papers/SKILL.md').read()
required = ['arxiv', 'semantic scholar', 'paper-scout', 'relevance', 'inbox', 'fran']
for term in required:
    assert term.lower() in content.lower(), f'Missing reference to {term}'
print('Skill references valid')
"
    Expected Result: "Skill references valid"
    Evidence: .sisyphus/evidence/task-8-find-papers-structure.txt

  Scenario: Verify quick-scan template in skill
    Tool: Bash (test -f)
    Steps:
      1. test -f cortical-columns-research/skills/find-papers/templates/quick-scan.md && echo "Template exists"
    Expected Result: "Template exists"
    Evidence: .sisyphus/evidence/task-8-quickscan-template.txt
  ```

  **Evidence to Capture**:
  - [ ] SKILL.md content
  - [ ] Template content

  **Commit**: YES (groups with 9)
  - Message: `feat(skills): add find-papers and analyze-paper skills`

---

- [x] 9. analyze-paper skill

  **What to do**:
  - Create `skills/analyze-paper/SKILL.md` — deep analysis workflow
  - Create `skills/analyze-paper/templates/deep-analysis.md` — output template
  - Skill workflow:
    1. User provides arXiv link or PDF
    2. Fetch paper metadata from arXiv MCP
    3. Invoke paper-analyst agent for deep analysis
    4. Enrich with Semantic Scholar (citations, related papers)
    5. Create deep-analysis note in appropriate folder
    6. Add backlinks to related papers in Obsidian
    7. Output summary to user (English analysis, French summary)
  - Must handle: invalid links, PDF parsing failures, missing metadata
  - Must check for duplicates before creating note

  **Must NOT do**:
  - Do not search for new papers (that's find-papers)
  - Do not move/organize existing papers

  **Recommended Agent Profile**:
  - **Category**: `deep`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Task 8)
  - **Blocks**: Task 12 (/research command)
  - **Blocked By**: Tasks 1, 2, 3, 4, 6

  **References**:
  - SKILL.md format: `https://github.com/anthropics/knowledge-work-plugins/blob/main/partner-built/brand-voice/skills/discover-brand/SKILL.md`
  - paper-analyst agent: Task 6 output
  - Deep-analysis template: Task 2 output

  **Acceptance Criteria**:
  - [ ] `skills/analyze-paper/SKILL.md` exists with complete workflow
  - [ ] Deep-analysis template matches vault template
  - [ ] Skill references paper-analyst agent
  - [ ] Backlink creation logic included
  - [ ] Duplicate detection included

  **QA Scenarios**:
  ```
  Scenario: Validate skill structure
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/skills/analyze-paper/SKILL.md').read()
required = ['paper-analyst', 'deep analysis', 'methodology', 'backlink', 'duplicate']
for term in required:
    assert term.lower() in content.lower(), f'Missing {term}'
print('Skill valid')
"
    Expected Result: "Skill valid"
    Evidence: .sisyphus/evidence/task-9-analyze-paper-structure.txt
  ```

  **Evidence to Capture**:
  - [ ] SKILL.md content
  - [ ] Template content

  **Commit**: YES (groups with 8)

---

- [x] 10. new-papers skill

  **What to do**:
  - Create `skills/new-papers/SKILL.md` — periodic scan workflow
  - Skill workflow:
    1. Check last scan timestamp (stored in `04 - Literature Map/last-scan.md`)
    2. Search arXiv for recent papers in relevant categories (cs.AI, cs.NE, q-bio.NC, cs.LG)
    3. Filter by date (since last scan, or last 7 days if first run)
    4. Quick scan each paper for relevance to cortical columns
    5. Create notes in `02 - Recent Discoveries/`
    6. Update last-scan timestamp
    7. Output French summary of new findings
  - Relevant categories: cs.AI, cs.NE, q-bio.NC, cs.LG, cs.CL, stat.ML
  - Must handle: no new papers, API errors, first-run scenario

  **Must NOT do**:
  - Do not run automatically (user triggers on demand)
  - Do not deep-analyze papers (quick scan only)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3 (with Tasks 11, 12, 13)
  - **Blocks**: Task 12 (/research command)
  - **Blocked By**: Tasks 1, 2, 3, 4, 7

  **References**:
  - arXiv MCP: Task 3 (get_recent_papers tool)
  - Vault structure: Task 2 (02 - Recent Discoveries/)
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md`

  **Acceptance Criteria**:
  - [ ] `skills/new-papers/SKILL.md` exists with complete workflow
  - [ ] References arXiv get_recent_papers tool
  - [ ] Includes last-scan tracking logic
  - [ ] Lists all relevant arXiv categories
  - [ ] French summary output specified

  **QA Scenarios**:
  ```
  Scenario: Validate skill structure
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/skills/new-papers/SKILL.md').read()
required = ['recent', 'last-scan', 'cs.AI', 'fran', 'inbox']
for term in required:
    assert term.lower() in content.lower(), f'Missing {term}'
print('Skill valid')
"
    Expected Result: "Skill valid"
    Evidence: .sisyphus/evidence/task-10-new-papers-structure.txt
  ```

  **Evidence to Capture**:
  - [ ] SKILL.md content

  **Commit**: YES (groups with 11, 12, 13)
  - Message: `feat(skills): add new-papers and map-literature skills plus /research command`

---

- [x] 11. map-literature skill

  **What to do**:
  - Create `skills/map-literature/SKILL.md` — gap analysis workflow
  - Skill workflow:
    1. Read all existing paper notes from Obsidian vault folders
    2. Build a topic map: which areas of cortical column research are covered
    3. Invoke literature-mapper agent for gap analysis
    4. Generate gap report in `04 - Literature Map/gaps.md`
    5. Suggest specific search queries to fill gaps
    6. Output summary to user
  - Must handle: empty vault (first run), no gaps found
  - Must categorize papers by subtopic: HTM, predictive coding, SDR, Thousand Brains, etc.

  **Must NOT do**:
  - Do not search external APIs (agent suggests queries, user triggers find-papers)
  - Do not auto-fill gaps (user decides what to search)

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3 (with Tasks 10, 12, 13)
  - **Blocks**: Task 12 (/research command)
  - **Blocked By**: Tasks 1, 2, 7

  **References**:
  - literature-mapper agent: Task 7 output
  - Vault structure: Task 2 (04 - Literature Map/)
  - Concept notes: Task 2 (03 - Concepts/)

  **Acceptance Criteria**:
  - [ ] `skills/map-literature/SKILL.md` exists with complete workflow
  - [ ] References literature-mapper agent
  - [ ] Includes vault reading logic
  - [ ] Generates gap report
  - [ ] Suggests search queries

  **QA Scenarios**:
  ```
  Scenario: Validate skill structure
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/skills/map-literature/SKILL.md').read()
required = ['literature-mapper', 'gap', 'vault', 'obsidian', 'suggest']
for term in required:
    assert term.lower() in content.lower(), f'Missing {term}'
print('Skill valid')
"
    Expected Result: "Skill valid"
    Evidence: .sisyphus/evidence/task-11-map-lit-structure.txt
  ```

  **Evidence to Capture**:
  - [ ] SKILL.md content

  **Commit**: YES (groups with 10, 12, 13)

---

- [x] 12. /research command

  **What to do**:
  - Create `commands/research.md` — entry point command
  - Command shows overview of all available workflows when user types `/research`
  - Lists: /find-papers, /analyze-paper, /new-papers, /map-literature
  - Brief description of each workflow
  - Friendly onboarding message for new users

  **Must NOT do**:
  - Do not implement workflow logic (skills handle this)
  - Do not duplicate skill content

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3 (with Tasks 10, 11, 13)
  - **Blocks**: None
  - **Blocked By**: Tasks 8-11 (needs to reference all skills)

  **References**:
  - Command format: `https://github.com/anthropics/knowledge-work-plugins/blob/main/productivity/README.md` — slash command examples
  - Design doc: `.sisyphus/designs/2026-04-05-research-plugin-design.md`

  **Acceptance Criteria**:
  - [ ] `commands/research.md` exists
  - [ ] Lists all 4 skills with descriptions
  - [ ] Includes trigger phrases for each workflow

  **QA Scenarios**:
  ```
  Scenario: Validate command content
    Tool: Bash (python -c)
    Steps:
      1. python -c "
content = open('cortical-columns-research/commands/research.md').read()
required = ['find-papers', 'analyze-paper', 'new-papers', 'map-literature']
for term in required:
    assert term.lower() in content.lower(), f'Missing {term}'
print('Command valid')
"
    Expected Result: "Command valid"
    Evidence: .sisyphus/evidence/task-12-command-structure.txt
  ```

  **Evidence to Capture**:
  - [ ] Command file content

  **Commit**: YES (groups with 10, 11, 13)

---

- [x] 13. .mcp.json + settings.json

  **What to do**:
  - Create `.mcp.json` at plugin root to wire both MCP servers:
    - arXiv server: `${CLAUDE_PLUGIN_ROOT}/mcp-servers/arxiv/server.py`
    - Semantic Scholar server: `${CLAUDE_PLUGIN_ROOT}/mcp-servers/semantic-scholar/server.py`
  - Create `settings.json` with default agent settings (model: sonnet for all agents)
  - Use `${CLAUDE_PLUGIN_ROOT}` variable for all paths
  - Follow official .mcp.json format from productivity plugin

  **Must NOT do**:
  - Do not use absolute paths
  - Do not hardcode API keys

  **Recommended Agent Profile**:
  - **Category**: `quick`
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 3 (with Tasks 10, 11, 12)
  - **Blocks**: None
  - **Blocked By**: Tasks 1, 3, 4 (needs MCP server paths)

  **References**:
  - .mcp.json format: `https://raw.githubusercontent.com/anthropics/knowledge-work-plugins/main/productivity/.mcp.json`
  - Plugin manifest: Task 1 output
  - MCP server paths: Tasks 3, 4

  **Acceptance Criteria**:
  - [ ] `.mcp.json` exists with valid JSON
  - [ ] Both MCP servers configured with correct paths
  - [ ] Uses `${CLAUDE_PLUGIN_ROOT}` variable
  - [ ] `settings.json` exists with agent defaults

  **QA Scenarios**:
  ```
  Scenario: Validate .mcp.json
    Tool: Bash (python -c)
    Steps:
      1. python -c "
import json
data = json.load(open('cortical-columns-research/.mcp.json'))
assert 'mcpServers' in data
assert 'arxiv' in data['mcpServers']
assert 'semantic-scholar' in data['mcpServers']
assert 'CLAUDE_PLUGIN_ROOT' in json.dumps(data)
print('MCP config valid')
"
    Expected Result: "MCP config valid"
    Evidence: .sisyphus/evidence/task-13-mcp-json.txt

  Scenario: Validate settings.json
    Tool: Bash (python -c)
    Steps:
      1. python -c "
import json
data = json.load(open('cortical-columns-research/settings.json'))
assert 'agent' in data or 'agents' in data
print('Settings valid')
"
    Expected Result: "Settings valid"
    Evidence: .sisyphus/evidence/task-13-settings-json.txt
  ```

  **Evidence to Capture**:
  - [ ] .mcp.json content
  - [ ] settings.json content

  **Commit**: YES (groups with 10, 11, 12)

---

## Final Verification Wave (MANDATORY — after ALL implementation tasks)

> **VERIFIED MANUALLY** — All 13 tasks verified with comprehensive checks below.

- [x] F1. **Plan Compliance Audit** — All 13 tasks present and verified
- [x] F2. **Code Quality Review** — All JSON valid, all Python imports clean, no TODOs/stubs
- [x] F3. **Real Manual QA** — MCP servers load, skills reference correct tools/agents, templates complete
- [x] F4. **Scope Fidelity Check** — No scope creep, no forbidden features, all deliverables match spec

---

## Commit Strategy

- **1-2**: `feat(plugin): scaffold cortical-columns-research plugin structure` — plugin.json, README.md, vault-template/, directories
- **3-4**: `feat(mcp): add arXiv and Semantic Scholar MCP servers` — mcp-servers/arxiv/, mcp-servers/semantic-scholar/
- **5-7**: `feat(agents): add paper-scout, paper-analyst, literature-mapper agents` — agents/*.md
- **8-9**: `feat(skills): add find-papers and analyze-paper skills` — skills/find-papers/, skills/analyze-paper/
- **10-13**: `feat(skills): add new-papers and map-literature skills plus /research command` — skills/new-papers/, skills/map-literature/, commands/, .mcp.json, settings.json

---

## Success Criteria

### Verification Commands
```bash
# Validate plugin structure
python -c "import json; json.load(open('cortical-columns-research/.claude-plugin/plugin.json'))"  # Expected: no error
python -c "import json; json.load(open('cortical-columns-research/.mcp.json'))"  # Expected: no error

# Verify all skills exist
ls cortical-columns-research/skills/*/SKILL.md  # Expected: 4 files

# Verify all agents exist
ls cortical-columns-research/agents/*.md  # Expected: 3 files

# Verify vault template structure
find cortical-columns-research/vault-template/ -type d | wc -l  # Expected: >= 8 dirs

# Verify MCP server imports
python -c "import sys; sys.path.insert(0, 'cortical-columns-research/mcp-servers/arxiv'); from server import mcp"  # Expected: no error
python -c "import sys; sys.path.insert(0, 'cortical-columns-research/mcp-servers/semantic-scholar'); from server import mcp"  # Expected: no error
```

### Final Checklist
- [ ] All "Must Have" present (arXiv search, two-tier analysis, vault auto-creation, French summaries, duplicate detection)
- [ ] All "Must NOT Have" absent (no Google Scholar MCP, no PDF in quick scan, no background monitoring, no citation graph viz, no over-abstraction)
- [ ] All 4 skills created with complete workflows
- [ ] All 3 agents created with proper frontmatter
- [ ] Both MCP servers import without errors
- [ ] Vault template has all 6 folders + templates
- [ ] .mcp.json wires both servers with ${CLAUDE_PLUGIN_ROOT}
- [ ] /research command lists all workflows
