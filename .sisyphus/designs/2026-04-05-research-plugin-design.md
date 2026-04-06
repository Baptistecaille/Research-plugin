# Research Plugin Design — Cortical Columns Thesis

**Date**: 2026-04-05
**Status**: Approved
**Target**: Claude Cowork Plugin

---

## Summary

A Cowork plugin that helps with thesis research on the algorithm representation of cortical columns. It discovers papers on arXiv and Google Scholar, performs two-tier analysis (quick scan → deep dive), organizes findings in an Obsidian vault, and identifies literature gaps.

---

## Architecture

### Plugin Structure

```
cortical-columns-research/
├── .claude-plugin/
│   └── plugin.json              # Manifest
├── skills/
│   ├── find-papers/
│   │   ├── SKILL.md
│   │   └── templates/quick-scan.md
│   ├── analyze-paper/
│   │   ├── SKILL.md
│   │   └── templates/deep-analysis.md
│   ├── new-papers/
│   │   └── SKILL.md
│   └── map-literature/
│       └── SKILL.md
├── agents/
│   ├── paper-scout.md
│   ├── paper-analyst.md
│   └── literature-mapper.md
├── commands/
│   └── research.md
├── mcp-servers/
│   ├── arxiv/
│   │   └── server.py
│   └── semantic-scholar/
│       └── server.py
├── .mcp.json
├── settings.json
└── README.md
```

### Approach: Hybrid (Approach 3)

- **MCP servers** for arXiv API + Semantic Scholar API (structured, reliable data)
- **Direct file writes** to Obsidian vault at `~/Research/cortical-columns/`
- Obsidian auto-detects new `.md` files — no MCP connector needed for Obsidian

---

## Obsidian Vault Structure

```
cortical-columns/
├── .obsidian/                    # Obsidian workspace config
├── 00 - Inbox/                   # New papers awaiting review
├── 01 - Foundational Papers/     # Key papers confirmed as relevant
├── 02 - Recent Discoveries/      # New papers from periodic scans
├── 03 - Concepts/                # Concept notes (cortical columns, HTM, etc.)
├── 04 - Literature Map/          # Gap analysis & synthesis notes
└── 05 - Thesis Notes/            # User's own writing & connections
```

### Paper Note Templates

**Quick Scan** — frontmatter with relevance score, abstract, key contribution, relevance to thesis, related paper links.

**Deep Analysis** — extended frontmatter with methodology quality, full methodology critique, strengths/weaknesses, connection to cortical columns, key findings, French summary.

---

## MCP Servers

### arXiv MCP
- `search_papers(query, max_results, sort_by)` — search with relevance sorting
- `get_paper_details(arxiv_id)` — full metadata, abstract, authors, categories
- `download_pdf(arxiv_id, output_path)` — fetch PDF for analysis
- `get_recent_papers(category, days)` — new papers in cs.AI, cs.NE, q-bio.NC, etc.

### Semantic Scholar MCP
- `get_related_papers(paper_id, limit)` — find similar papers
- `get_citation_count(paper_id)` — citation count
- `get_authors_papers(author_name)` — other work by same authors

---

## Workflow Data Flow

```
User: /find-papers "cortical columns predictive coding"
  → Skill: find-papers/SKILL.md orchestrates
  → Agent: paper-scout (fast discovery)
  → MCP: arxiv.search_papers(query) → 20 results
  → Quick scan each → relevance score 1-10
  → Filter: score ≥ 7 → proceed
  → MCP: semantic-scholar.get_related_papers() → enrich
  → Write: .md notes in 00 - Inbox/
  → Output: French summary to user + list of papers
```

---

## Error Handling

| Scenario | Behavior |
|----------|----------|
| arXiv API rate limit | Retry with backoff, French notification |
| PDF not available | Abstract-only analysis, flag in note |
| Semantic Scholar down | Continue without citation data |
| Obsidian vault not found | Auto-create with full structure |
| Duplicate paper | Update existing note, don't duplicate |
| No relevant papers | Suggest broader search terms |
| Network unavailable | Graceful French error message |

---

## Language Strategy

- **Deep analysis**: English (technical depth, matches paper language)
- **Quick summaries/notifications**: French (user preference)
- **Obsidian notes**: English (academic standard, searchable)
- **French summary field**: Included in deep analysis template

---

## Skills & Commands

| Skill | Command | Purpose |
|-------|---------|---------|
| find-papers | `/find-papers` | Search → quick scan → push to Obsidian |
| analyze-paper | `/analyze-paper` | PDF/arXiv link → deep analysis → Obsidian note |
| new-papers | `/new-papers` | Periodic scan for new papers since last check |
| map-literature | `/map-literature` | Gap analysis of existing Obsidian vault |
| — | `/research` | Entry point showing all available workflows |

## Agents

| Agent | Model | Purpose |
|-------|-------|---------|
| paper-scout | sonnet | Fast paper discovery & relevance scoring |
| paper-analyst | sonnet | Deep methodology critique & thesis connections |
| literature-mapper | sonnet | Vault analysis, gap identification, synthesis |
