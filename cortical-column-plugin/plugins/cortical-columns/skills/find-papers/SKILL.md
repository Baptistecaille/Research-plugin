---
name: find-papers
description: Search arXiv for papers relevant to cortical column research, quick-scan for relevance, and push findings to Obsidian vault
---

## When to Use This Skill

Use this skill when you want to discover new academic papers for your thesis research on cortical columns.

**Trigger phrases:**
- "Find papers about [topic]"
- "Search for papers on [topic]"
- "Find relevant papers"
- "Discover papers about [topic]"

## Workflow

### Step 1: Ensure Obsidian Vault Exists

Check if `~/Research/cortical-columns/` exists. If not:
1. Create the vault structure at `~/Research/cortical-columns/` with all required folders
2. Create all folders: `00 - Inbox/`, `01 - Foundational Papers/`, `02 - Recent Discoveries/`, `03 - Concepts/`, `04 - Literature Map/`, `05 - Thesis Notes/`
3. Confirm vault creation to the user in French: "Votre vault Obsidian a été créé avec succès."

### Step 2: Search arXiv

Use Claude's web search to find papers on arXiv:
1. Search arxiv.org for papers matching the user's query
2. Search for up to 20 results sorted by relevance
3. If no results, try broader search terms and inform the user
4. If results are empty after retries, suggest alternative search queries

### Step 3: Quick Scan & Relevance Scoring

For each paper returned:
1. Evaluate relevance directly using Claude's AI capabilities
2. Score each paper 1-10 based on relevance to cortical column research
3. Filter: keep only papers with score >= 7

### Step 4: Enrich with Citation Data

For each filtered paper (score >= 7):
1. Look up citation count by fetching data from semanticscholar.org
2. Optionally find related papers on Semantic Scholar (limit 3)
3. If Semantic Scholar data is unavailable, continue without citation metrics

### Step 5: Check for Duplicates

Before creating any note:
1. Scan `00 - Inbox/`, `01 - Foundational Papers/`, and `02 - Recent Discoveries/` for existing notes
2. Check by arXiv ID in the YAML frontmatter
3. If duplicate found: skip creation, note that paper already exists

### Step 6: Create Obsidian Notes

For each unique, relevant paper (score >= 7):
1. Create a markdown file in `~/Research/cortical-columns/00 - Inbox/`
2. Filename format: `{arxiv_id}-{slugified-title}.md`
3. Use the quick-scan template format:
   - YAML frontmatter with: title, authors, year, arxiv_id, source, relevance_score, status, tags, date_added
   - Abstract section
   - Quick Assessment section (key contribution, relevance to thesis, should deep-dive)
   - Related Papers section with Obsidian backlinks
4. Add tags based on paper categories and relevance reasoning

### Step 7: Output Summary to User

Present findings to the user with a **French summary**:
- "J'ai trouvé X articles pertinents sur [topic] parmi Y résultats recherchés."
- List each paper with: title, relevance score (X/10), and brief reason
- Indicate which papers are recommended for deep analysis
- Provide the Obsidian vault path where notes were created

## Error Handling

- **arXiv search unavailable**: "L'API arXiv n'est pas disponible actuellement. Veuillez réessayer plus tard."
- **No relevant papers found**: "Aucun article pertinent trouvé pour '[topic]'. Essayez des termes de recherche plus larges."
- **Vault creation failed**: "Impossible de créer le vault Obsidian. Vérifiez les permissions du dossier ~/Research/."
- **Rate limited**: Wait and retry. Inform user: "Limite de requêtes atteinte, nouvelle tentative dans quelques secondes."

## Output Format

The skill outputs:
1. Markdown notes in `00 - Inbox/` folder of the Obsidian vault
2. A French summary message to the user listing findings
