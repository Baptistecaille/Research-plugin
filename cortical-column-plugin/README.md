# Cortical Columns Research

Claude Cowork marketplace plugin for thesis research on the algorithm representation of cortical columns.

## Install

In Claude Cowork, type `/plugin`, select **Add marketplace**, enter your repository name.

Then select the `cortical-columns` plugin to install.

## Plugin

### cortical-columns

Research companion for cortical columns thesis with 4 workflows:

| Skill | Description |
|-------|-------------|
| `/find-papers` | Search arXiv for relevant papers, quick-scan for relevance, push to Obsidian |
| `/analyze-paper` | Deep methodology analysis of specific papers with thesis connections |
| `/new-papers` | Periodic scan for new papers in relevant arXiv categories |
| `/map-literature` | Gap analysis of your existing Obsidian research vault |

```
/find-papers "cortical columns predictive coding"
/analyze-paper "https://arxiv.org/abs/2001.00001"
/new-papers
/map-literature
```

## Structure

```
cortical-columns-research/
├── .claude-plugin/marketplace.json
└── plugins/
    └── cortical-columns/
        ├── .claude-plugin/plugin.json
        ├── commands/
        │   └── research.md
        └── skills/
            ├── find-papers/
            │   ├── SKILL.md
            │   └── templates/quick-scan.md
            ├── analyze-paper/
            │   ├── SKILL.md
            │   └── templates/deep-analysis.md
            ├── new-papers/
            │   └── SKILL.md
            └── map-literature/
                └── SKILL.md
```

## Features

- **Two-tier analysis**: Quick scan (abstract + relevance score) → deep analysis on flagged papers
- **Obsidian integration**: Auto-creates vault at `~/Research/cortical-columns/` with organized folders
- **French summaries**: Quick takeaways in French, technical analysis in English
- **Duplicate detection**: Prevents duplicate paper notes by arXiv ID
- **Citation enrichment**: Pulls citation data from Semantic Scholar

## Requirements

- Claude Cowork (Pro, Max, Team, or Enterprise plan)
- Obsidian (for viewing research notes)

## License

MIT
