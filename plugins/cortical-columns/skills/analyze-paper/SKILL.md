---
name: analyze-paper
description: Perform deep methodology analysis of a specific paper with thesis connections, creating a detailed Obsidian note
---

## When to Use This Skill

Use this skill when you have a specific paper (arXiv link, PDF, or paper ID) that you want to analyze in depth for your thesis research.

**Trigger phrases:**
- "Analyze this paper: [link]"
- "Deep analysis of [paper title/arXiv ID]"
- "Analyze [arxiv link]"
- "Review this paper's methodology"

## Workflow

### Step 1: Extract Paper Identifier

From the user's input, extract:
- arXiv ID (e.g., "2001.00001" or "1706.03762")
- Or arXiv URL (e.g., "https://arxiv.org/abs/2001.00001")
- Or paper title for lookup

### Step 2: Fetch Paper Metadata

Fetch paper metadata directly from arXiv:
1. If an arXiv URL or ID is provided, fetch metadata from `https://arxiv.org/abs/{arxiv_id}` or `https://arxiv.org/html/{arxiv_id}`
2. If only a title is provided, search arxiv.org for the paper and fetch the top match
3. If metadata not found, inform the user: "Impossible de trouver les métadonnées de cet article."

### Step 3: Check for Existing Analysis (Duplicate Detection)

Before creating a new note:
1. Scan `~/Research/cortical-columns/` for existing notes with matching arXiv ID in their YAML frontmatter
2. Check `00 - Inbox/`, `01 - Foundational Papers/`, `02 - Recent Discoveries/`
3. Match by `arxiv_id` field in frontmatter — this is the canonical duplicate key
4. If an analysis already exists:
   - Inform the user: "Une analyse existe déjà pour cet article : [path to existing note]"
   - Offer to update it instead of creating a duplicate
   - If user confirms update, proceed with Step 4 using the existing file path

### Step 4: Perform Deep Methodology Analysis

Claude performs the deep methodology analysis directly using its AI capabilities:
- Analyze the paper's full text (from arXiv HTML or abstract)
- Provide structured analysis with:
  - **Methodology Critique**: Evaluate research design, methods, validity
  - **Strengths**: What the paper does well
  - **Weaknesses**: Limitations, flaws, gaps
  - **Assumptions**: Explicit and implicit assumptions
  - **Connection to Cortical Columns**: How this relates to the thesis topic
  - **Key Findings**: Main contributions and results
  - **Related Work**: Connections to other papers in the field
  - **French summary**: Concise résumé en français

### Step 5: Enrich with Citation Data

1. Look up citation count by fetching data from semanticscholar.org
2. Find related papers on Semantic Scholar (limit 5)
3. Use related papers to create Obsidian backlinks in the note
4. If Semantic Scholar is unavailable, continue without citation data and note: "Données de citation non disponibles."

### Step 6: Create Deep Analysis Note

Create a markdown file in `~/Research/cortical-columns/00 - Inbox/`:
1. Filename: `{arxiv_id}-{slugified-title}-analysis.md`
2. Include YAML frontmatter with: title, authors, year, arxiv_id, source, relevance_score, status (analyzed), tags, date_added, analysis_date, methodology_quality
3. Fill in all sections from the analysis output
4. Add backlinks to related papers: `[[{arxiv_id}-{slugified-title}]]`

### Step 7: Create Backlinks

For each related paper found via Semantic Scholar:
1. Check if the related paper already has a note in the vault (scan by arXiv ID in frontmatter)
2. If it exists, add a backlink to the current note: `[[{related-arxiv-id}-{related-slugified-title}]]`
3. If possible, also append a backlink to the related paper's note pointing back to this new analysis (bidirectional linking)
4. This creates bidirectional linking in Obsidian's graph view

### Step 8: Output Summary to User

Present the analysis with:
- **English**: Full methodology critique and findings
- **French summary**: "Résumé : Cet article de [authors] propose [key contribution]. Sa méthodologie est [quality assessment]. Il est pertinent pour votre thèse car [connection reason]."
- Link to the created Obsidian note

## Error Handling

- **Paper not found**: "Impossible de trouver l'article '[identifier]'. Vérifiez l'ID arXiv ou le titre."
- **Semantic Scholar unavailable**: Continue without citation data, note: "Données de citation non disponibles."
- **Note creation failed**: "Impossible de créer la note. Vérifiez les permissions du vault Obsidian."
- **Duplicate detected**: "Une analyse existe déjà pour cet article : [path]. Voulez-vous la mettre à jour ?"

## Output Format

The skill outputs:
1. A deep-analysis markdown note in `00 - Inbox/` folder
2. Backlinks added to related paper notes
3. A summary message to the user (English analysis + French summary)
