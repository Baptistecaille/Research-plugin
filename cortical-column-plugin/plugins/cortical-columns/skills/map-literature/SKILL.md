---
name: map-literature
description: Analyze existing Obsidian research vault, identify gaps in cortical column literature, and suggest papers to fill them
---

## When to Use This Skill

Use this skill to evaluate the completeness of your existing research collection and discover gaps in your literature review.

**Trigger phrases:**
- "Map my literature"
- "Analyze my research vault"
- "What gaps do I have in my literature?"
- "Evaluate my paper collection"

## Workflow

### Step 1: Read Existing Vault Contents

1. Scan all folders in `~/Research/cortical-columns/`:
   - `00 - Inbox/` — papers awaiting review
   - `01 - Foundational Papers/` — confirmed relevant papers
   - `02 - Recent Discoveries/` — papers from periodic scans
   - `03 - Concepts/` — concept notes
2. For each paper note, extract:
   - Title, authors, arXiv ID, tags
   - Relevance score
   - Topic/subtopic from tags and content
3. Build a topic map: count papers per subtopic

### Step 2: Categorize by Subtopic

Organize papers into these subtopics:
1. Cortical column anatomy (Mountcastle, minicolumns)
2. Hierarchical Temporal Memory (HTM)
3. Predictive coding
4. Sparse Distributed Representations (SDRs)
5. Thousand Brains Theory
6. Canonical microcircuit
7. Neuroscience-inspired AI
8. State space models (Mamba, RWKV)
9. Continual learning

### Step 3: Perform Gap Analysis

Claude performs the gap analysis directly using its AI capabilities:
- Analyze the organized paper collection across all subtopics
- Identify underrepresented or missing subtopics
- Assess the depth and breadth of current coverage
- Suggest specific papers or search queries to fill identified gaps
- Return:
  - Current coverage assessment
  - Identified gaps
  - Suggested papers to add
  - Recommended search queries

### Step 4: Generate Gap Report

Create or update `~/Research/cortical-columns/04 - Literature Map/gaps.md`:
1. Include the full gap analysis
2. Sections: Current Coverage, Identified Gaps, Suggested Papers, Recommended Search Queries, Synthesis Notes
3. Add timestamp: `**Last updated**: {current date}`

### Step 5: Handle Empty Vault

If the vault has no paper notes (first run):
1. Inform the user: "Votre vault est vide. Je vais créer une analyse initiale des domaines clés à couvrir."
2. Generate a "starter gap analysis" listing all key subtopics with foundational papers that should be collected
3. Save to `gaps.md` as a baseline

### Step 6: Output Summary to User

Present the gap analysis with:
- Overview: "Votre collection couvre X sous-domaines avec Y articles."
- Top 3 gaps identified
- Top 3 suggested papers to add
- Recommended next action (e.g., "Lancez /find-papers avec la query '[suggested query]'")

## Error Handling

- **Vault not found**: "Vault Obsidian introuvable. Lancez d'abord /find-papers pour le créer."
- **Empty vault**: Generate starter analysis (see Step 5)
- **No gaps found**: "Votre collection semble bien couverte. Excellent travail !"

## Output Format

The skill outputs:
1. Gap report in `04 - Literature Map/gaps.md`
2. A summary message to the user with key findings and recommended actions
