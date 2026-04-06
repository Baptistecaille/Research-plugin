---
name: new-papers
description: Scan for new papers in relevant arXiv categories since last check, quick-scan for relevance, and push findings to Obsidian
---

## When to Use This Skill

Use this skill to discover new papers published since your last scan in categories relevant to cortical column research.

**Trigger phrases:**
- "What's new since last time?"
- "Scan for new papers"
- "Check for new publications"
- "Any new papers in my field?"
- "Quoi de neuf dans la littérature ?"

## Workflow

### Step 1: Check Last Scan Timestamp

1. Look for `~/Research/cortical-columns/04 - Literature Map/last-scan.md`
2. If it exists, read the timestamp to determine days since last scan
3. If it doesn't exist (first run): inform the user "Première analyse — je vais scanner les 7 derniers jours." and use 7 days as default

### Step 2: Ensure Obsidian Vault Exists

Check if `~/Research/cortical-columns/` exists. If not:
1. Create the vault structure at `~/Research/cortical-columns/` with all required folders
2. Create all folders: `00 - Inbox/`, `01 - Foundational Papers/`, `02 - Recent Discoveries/`, `03 - Concepts/`, `04 - Literature Map/`, `05 - Thesis Notes/`
3. Confirm vault creation to the user in French: "Votre vault Obsidian a été créé avec succès."

### Step 3: Search arXiv for Recent Papers

For each relevant arXiv category, use Claude's web search to find recent papers:
1. Search arxiv.org for recent papers in each category (cs.AI, cs.NE, q-bio.NC, cs.LG, cs.CL, stat.ML)
2. Use web search with category-specific queries like "arxiv cs.AI new papers this week" or fetch from `https://arxiv.org/list/{category}/recent`
3. Combine all results, remove duplicates by arXiv ID

### Step 4: Quick Scan & Relevance Scoring

For each unique paper:
1. Evaluate relevance directly using Claude's AI capabilities
2. Score each paper 1-10 based on relevance to cortical column research
3. Filter: keep only papers with score >= 7

### Step 5: Check for Duplicates in Vault

Before creating any note:
1. Scan `00 - Inbox/`, `01 - Foundational Papers/`, and `02 - Recent Discoveries/` for existing notes
2. Check by arXiv ID in the YAML frontmatter
3. If duplicate found: skip creation, note that paper already exists

### Step 6: Create Obsidian Notes

For each unique, relevant paper (score >= 7):
1. Create a markdown file in `~/Research/cortical-columns/02 - Recent Discoveries/`
2. Filename format: `{arxiv_id}-{slugified-title}.md`
3. Use the quick-scan template format:
   - YAML frontmatter with: title, authors, year, arxiv_id, source, relevance_score, status, tags, date_added
   - Abstract section
   - Quick Assessment section (key contribution, relevance to thesis, should deep-dive)
   - Related Papers section with Obsidian backlinks
4. Add a "Source: periodic scan" tag

### Step 7: Update Last Scan Timestamp

1. Create or update `~/Research/cortical-columns/04 - Literature Map/last-scan.md`
2. Content:
```markdown
# Last Scan

**Timestamp**: {current ISO timestamp}
**Categories scanned**: cs.AI, cs.NE, q-bio.NC, cs.LG, cs.CL, stat.ML
**Results**: {count} new papers found
```

### Step 8: Output Summary to User

Present findings with a **French summary** (résumé en français):
- "J'ai trouvé X nouveaux articles pertinents depuis le {last_scan_date}."
- List each paper with: title, category, relevance score, and brief reason
- If no new papers: "Aucun nouvel article pertinent depuis votre dernière analyse."

## Error Handling

- **arXiv search unavailable**: "L'API arXiv n'est pas disponible actuellement."
- **No new papers**: "Aucun nouvel article pertinent trouvé dans les catégories scannées."
- **First run**: Use 7-day default, inform user

## Relevant arXiv Categories

- **cs.AI**: Artificial Intelligence
- **cs.NE**: Neural and Evolutionary Computing
- **q-bio.NC**: Neurons and Cognition (computational neuroscience)
- **cs.LG**: Machine Learning
- **cs.CL**: Computation and Language (NLP)
- **stat.ML**: Machine Learning (statistics)

## Output Format

The skill outputs:
1. Markdown notes in `02 - Recent Discoveries/` folder
2. Updated last-scan timestamp in `04 - Literature Map/last-scan.md`
3. A French summary message to the user
