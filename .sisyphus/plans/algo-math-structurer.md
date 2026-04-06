# AlgoMathStructurer — Claude Cowork Plugin

## TL;DR

> **Quick Summary**: Build a Claude Cowork plugin (`algo-math-structurer`) that transforms Python AI algorithm code into formal mathematical documents — definitions, theorems, lemmas, proofs — exported as compilable LaTeX (.tex) and compiled to PDF via pdflatex.
>
> **Deliverables**:
> - `.claude-plugin/plugin.json` — Plugin manifest
> - `skills/algorithm-formalization/SKILL.md` — Core skill: parse Python → extract math properties → generate proofs → produce LaTeX
> - `skills/algorithm-formalization/references/latex-template.md` — LaTeX article template with all packages and environments
> - `skills/algorithm-formalization/references/proof-patterns.md` — Proof strategy reference (recurrence, contradiction, induction, complexity)
> - `commands/generate-proof.md` — Slash command `/algo-math:generate-proof`
> - `commands/analyze-complexity.md` — Slash command `/algo-math:analyze-complexity`
> - `commands/extract-properties.md` — Slash command `/algo-math:extract-properties`
> - `README.md` — Plugin documentation
>
> **Estimated Effort**: Medium
> **Parallel Execution**: YES — 3 waves
> **Critical Path**: Task 1 (scaffolding) → Task 2 (core skill) → Task 5 (commands) → Task 6 (references)

---

## Context

### Original Request
Plugin Claude Cowork qui transforme du code Python d'algorithmes d'IA en structure mathématique formelle avec preuves, exporté en LaTeX compilé en PDF.

### Interview Summary
**Key Discussions**:
- **Langage d'entrée**: Python uniquement, algorithmes d'IA (spectre large)
- **Niveau de preuve**: Générées par Claude, lisibles et vérifiables par un humain (pas de Coq/Lean)
- **Types de preuves**: Tous — récurrence, absurde, induction structurelle, analyse de complexité
- **Compilation LaTeX**: Via Bash (`pdflatex`) — Option A
- **Template**: Article class avec amsmath, amsthm, amssymb, algorithm2e, listings, tikz
- **Interaction**: Commandes slash structurées (`/algo-math:...`)
- **Tests**: Aucun
- **Nom**: `algo-math-structurer`

**Research Findings**:
- **Architecture Cowork**: 100% markdown + JSON, pas de code, pas de build steps
- **Skills**: Activation automatique via trigger phrases dans le frontmatter YAML
- **Commands**: Invocation explicite via slash commands dans `commands/*.md`
- **Structure**: `.claude-plugin/plugin.json` + `skills/` + `commands/` + `.mcp.json`
- **Packaging**: zip en `.plugin` file
- **Progressive disclosure**: SKILL.md < 3000 mots, détails dans `references/`

### Metis Review
**Identified Gaps** (addressed):
- **Scope trop large**: "Tout algo Python d'IA" → Locké à algorithmes classiques (recherche, tri, graphe, optimisation, DP, clustering). Exclusions explicites: neural training loops, RL, generative models.
- **Pas de garde-fous contre l'hallucination**: Ajouté guardrail "Never hallucinate — unprovable = stated as unprovable"
- **Gestion échec pdflatex**: Si compilation échoue, retourner le .tex brut + log d'erreur
- **Taille d'entrée**: Cap à ~200 lignes ou fonction unique. Au-delà: message d'erreur clair.
- **Cas limites**: Code avec erreurs syntaxiques, algorithmes sans boucles, appels à bibliothèques externes, code stochastique — tous traités dans le SKILL.md.
- **Output**: Document PDF contenant: (1) pseudocode, (2) définitions formelles, (3) propriétés énoncées, (4) preuves, (5) analyse de complexité.

---

## Work Objectives

### Core Objective
Créer un plugin Claude Cowork fonctionnel qui, à partir de code Python d'algorithmes d'IA, génère un document mathématique formel avec preuves, compilé en PDF.

### Concrete Deliverables
- Plugin `algo-math-structurer` complet avec manifest, skills, commands, references
- 3 slash commands: `/algo-math:generate-proof`, `/algo-math:analyze-complexity`, `/algo-math:extract-properties`
- 1 skill principal: `algorithm-formalization`
- Templates LaTeX et patterns de preuve en références

### Definition of Done
- [ ] Plugin installé dans Cowork et les 3 commandes slash répondent correctement
- [ ] Un exemple Python (exponentiation rapide) → PDF généré avec définitions, théorèmes, preuves
- [ ] `pdflatex` compile sans erreur (exit code 0)
- [ ] Le PDF contient les 5 sections: pseudocode, définitions, propriétés, preuves, complexité

### Must Have
- Plugin.json valide avec metadata complète
- SKILL.md avec workflow structuré: parse → extract → formalize → prove → LaTeX → compile
- 3 commandes slash fonctionnelles
- Template LaTeX article avec packages requis
- Patterns de preuve pour: récurrence, contradiction, induction structurelle, complexité
- Gestion d'erreurs: code invalide, pdflatex manquant, algorithme non-analysable
- Guardrail anti-hallucination explicite

### Must NOT Have (Guardrails)
- **Pas de code exécutable** — le plugin est 100% markdown/JSON
- **Pas de support multi-langages** — Python uniquement
- **Pas de preuves Coq/Lean** — preuves lisibles par humain uniquement
- **Pas de neural training loops, RL, generative models** — hors scope v1
- **Pas de raffinement interactif de preuves** — one-shot generation
- **Pas de vérification de preuves externes** — génération uniquement
- **Pas de tests automatisés**
- **Jamais halluciner une propriété** — si non identifiable, le stated explicitement
- **Pas de diagrammes Tikz complexes** — un flowchart de contrôle maximum par algorithme

---

## Verification Strategy (MANDATORY)

> **ZERO HUMAN INTERVENTION** — ALL verification is agent-executed. No exceptions.

### Test Decision
- **Infrastructure exists**: NO
- **Automated tests**: NONE (explicitly requested)
- **Framework**: none
- **Agent-Executed QA**: ALWAYS (mandatory for all tasks)

### QA Policy
Every task MUST include agent-executed QA scenarios. Evidence saved to `.sisyphus/evidence/task-{N}-{scenario-slug}.{ext}`.

- **Plugin files**: Use Bash — validate JSON syntax, check file existence, verify structure
- **LaTeX compilation**: Use Bash — run `pdflatex`, check exit code, verify PDF output
- **Skill/Command content**: Use Bash — grep for required sections, verify frontmatter

---

## Execution Strategy

### Parallel Execution Waves

```
Wave 1 (Start Immediately — scaffolding + manifest):
├── Task 1: Plugin scaffolding + plugin.json [quick]
├── Task 2: README.md documentation [quick]
└── Task 3: LaTeX template reference [quick]

Wave 2 (After Wave 1 — core logic, MAX PARALLEL):
├── Task 4: Core SKILL.md — algorithm formalization workflow [deep]
├── Task 5: Proof patterns reference [unspecified-high]
└── Task 6: Three slash commands [quick]

Wave 3 (After Wave 2 — integration + validation):
├── Task 7: End-to-end validation with example algorithm [deep]
└── Task 8: Plugin packaging (.plugin zip) [quick]

Wave FINAL (After ALL tasks — 4 parallel reviews, then user okay):
├── Task F1: Plan compliance audit (oracle)
├── Task F2: Code quality review (unspecified-high)
├── Task F3: Real manual QA (unspecified-high)
└── Task F4: Scope fidelity check (deep)
-> Present results -> Get explicit user okay

Critical Path: Task 1 → Task 4 → Task 6 → Task 7 → F1-F4 → user okay
Parallel Speedup: ~60% faster than sequential
Max Concurrent: 3 (Waves 1 & 2)
```

### Dependency Matrix

- **1**: - — 2, 4, 6
- **2**: 1 — 7
- **3**: - — 4, 7
- **4**: 1, 3 — 6, 7
- **5**: - — 4, 7
- **6**: 1, 4 — 7
- **7**: 2, 3, 4, 5, 6 — 8, F1-F4
- **8**: 7 — F1-F4

### Agent Dispatch Summary

- **Wave 1**: **3** — T1 → `quick`, T2 → `quick`, T3 → `quick`
- **Wave 2**: **3** — T4 → `deep`, T5 → `unspecified-high`, T6 → `quick`
- **Wave 3**: **2** — T7 → `deep`, T8 → `quick`
- **FINAL**: **4** — F1 → `oracle`, F2 → `unspecified-high`, F3 → `unspecified-high`, F4 → `deep`

---

## TODOs

> Implementation + Test = ONE Task. Never separate.
> EVERY task MUST have: Recommended Agent Profile + Parallelization info + QA Scenarios.

- [x] 1. Plugin Scaffolding + plugin.json

  **What to do**:
  - Create directory structure: `algo-math-structurer/.claude-plugin/`, `algo-math-structurer/skills/algorithm-formalization/`, `algo-math-structurer/skills/algorithm-formalization/references/`, `algo-math-structurer/commands/`
  - Create `.claude-plugin/plugin.json` with: name `algo-math-structurer`, version `0.1.0`, description, author, keywords: ["mathematics", "latex", "formal-proofs", "algorithms", "python", "ai"]
  - Create empty `.mcp.json` (no external connectors needed — standalone mode)

  **Must NOT do**:
  - Add MCP server definitions (plugin is standalone)
  - Create any code files (.py, .ts, .js)
  - Add skills or commands content (separate tasks)

  **Recommended Agent Profile**:
  - **Category**: `quick`
    - Reason: Simple file creation and JSON writing, no complex logic
  - **Skills**: []
    - No skills needed — straightforward scaffolding
  - **Skills Evaluated but Omitted**:
    - `writing-skills`: Not writing a skill, just creating directory structure

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 2, 3)
  - **Blocks**: Tasks 4, 6
  - **Blocked By**: None (can start immediately)

  **References**:
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/building/plugin-structure` — Plugin directory structure and plugin.json schema
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/api/plugin-json` — plugin.json fields and validation rules

  **WHY Each Reference Matters**:
  - Plugin structure doc: Shows exact directory layout required by Cowork runtime
  - plugin.json schema: Defines required fields (name, version, description, author) and naming conventions (kebab-case)

  **Acceptance Criteria**:
  - Directory tree matches: `.claude-plugin/plugin.json`, `skills/algorithm-formalization/references/`, `commands/`, `.mcp.json`, `README.md`
  - plugin.json is valid JSON with all required fields
  - .mcp.json is valid JSON with empty `mcpServers` object

  **QA Scenarios**:

  ```
  Scenario: Validate plugin structure exists
    Tool: Bash
    Steps:
      1. Run: ls -la algo-math-structurer/.claude-plugin/plugin.json
      2. Run: ls -la algo-math-structurer/skills/algorithm-formalization/references/
      3. Run: ls -la algo-math-structurer/commands/
      4. Run: ls -la algo-math-structurer/.mcp.json
    Expected Result: All 4 paths exist (exit code 0)
    Failure Indicators: Any "No such file or directory" error
    Evidence: .sisyphus/evidence/task-1-structure-validation.txt

  Scenario: Validate JSON syntax
    Tool: Bash
    Steps:
      1. Run: python3 -c "import json; json.load(open('algo-math-structurer/.claude-plugin/plugin.json'))"
      2. Run: python3 -c "import json; json.load(open('algo-math-structurer/.mcp.json'))"
    Expected Result: Both commands exit with code 0 (valid JSON)
    Failure Indicators: json.decoder.JSONDecodeError
    Evidence: .sisyphus/evidence/task-1-json-validation.txt
  ```

  **Evidence to Capture**:
  - Directory listing output
  - JSON validation output

  **Commit**: YES (groups with 2, 3)
  - Message: `chore(algo-math-structurer): scaffold plugin directory structure and manifest`
  - Files: `algo-math-structurer/.claude-plugin/plugin.json`, `algo-math-structurer/.mcp.json`
  - Pre-commit: none

---

- [x] 2. README.md Documentation

  **What to do**:
  - Create `algo-math-structurer/README.md` with:
    - Plugin description and purpose
    - List of skills: `algorithm-formalization`
    - List of commands: `/algo-math:generate-proof`, `/algo-math:analyze-complexity`, `/algo-math:extract-properties`
    - Setup instructions (install .plugin file in Cowork)
    - Usage examples with sample Python input and expected PDF output
    - Customization tips
    - Limitations (Python only, classical algorithms, no neural training loops)

  **Must NOT do**:
  - Include code examples beyond Python snippets
  - Document features not in scope (Coq/Lean, multi-language, interactive refinement)

  **Recommended Agent Profile**:
  - **Category**: `quick`
    - Reason: Documentation writing, straightforward markdown
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - `writing-skills`: Could apply but this is simple plugin documentation, not creative writing

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 3)
  - **Blocks**: Task 7
  - **Blocked By**: Task 1 (needs plugin name from plugin.json)

  **References**:
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/building/plugin-structure` — README.md section in plugin structure

  **WHY Each Reference Matters**:
  - Plugin structure doc: Shows what sections a good plugin README should include

  **Acceptance Criteria**:
  - README.md exists at plugin root
  - Contains: description, skills list, commands list, setup instructions, usage examples, limitations

  **QA Scenarios**:

  ```
  Scenario: Validate README content
    Tool: Bash
    Steps:
      1. Run: grep -c "algo-math" algo-math-structurer/README.md
      2. Run: grep -c "/algo-math:" algo-math-structurer/README.md
      3. Run: grep -c "algorithm-formalization" algo-math-structurer/README.md
      4. Run: grep -c "Python" algo-math-structurer/README.md
    Expected Result: All grep commands return count > 0
    Failure Indicators: Any count = 0 (missing required section)
    Evidence: .sisyphus/evidence/task-2-readme-validation.txt

  Scenario: Validate README is valid markdown
    Tool: Bash
    Steps:
      1. Run: wc -l algo-math-structurer/README.md
      2. Verify file has at least 30 lines of content
    Expected Result: Line count >= 30
    Failure Indicators: Line count < 30 (insufficient documentation)
    Evidence: .sisyphus/evidence/task-2-readme-length.txt
  ```

  **Evidence to Capture**:
  - Grep output showing required sections
  - Line count output

  **Commit**: YES (groups with 1, 3)
  - Message: `chore(algo-math-structurer): scaffold plugin directory structure and manifest`
  - Files: `algo-math-structurer/README.md`

---

- [x] 3. LaTeX Template Reference

  **What to do**:
  - Create `algo-math-structurer/skills/algorithm-formalization/references/latex-template.md` containing:
    - Complete LaTeX article template with:
      - `\documentclass[11pt, a4paper]{article}`
      - Packages: `amsmath`, `amsthm`, `amssymb`, `algorithm2e`, `listings`, `tikz`
      - Theorem environments: `\newtheorem{theorem}{Théorème}`, `\newtheorem{lemma}[theorem]{Lemme}`, `\newtheorem{definition}{Définition}`
      - Python listing style via `listings` package
      - Document structure: title, abstract, sections for algorithm description, definitions, theorems, proofs, complexity analysis
    - Instructions for the skill on how to fill the template
    - Notes on package compatibility (e.g., algorithm2e vs algorithmic conflicts)

  **Must NOT do**:
  - Include algorithm-specific content (this is a blank template)
  - Add packages not listed in requirements
  - Use custom fonts or non-standard classes

  **Recommended Agent Profile**:
  - **Category**: `quick`
    - Reason: Template writing, well-known LaTeX structure
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - `writing-skills`: Not creative writing, technical template

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 2)
  - **Blocks**: Tasks 4, 7
  - **Blocked By**: None (can start immediately)

  **References**:
  - External: `https://ctan.org/pkg/amsthm` — amsthm package for theorem environments
  - External: `https://ctan.org/pkg/algorithm2e` — algorithm2e package for algorithm pseudocode

  **WHY Each Reference Matters**:
  - amsthm docs: Correct syntax for theorem/lemma/proof environments
  - algorithm2e docs: Correct syntax for algorithm pseudocode formatting

  **Acceptance Criteria**:
  - latex-template.md exists in references/
  - Template compiles with pdflatex (empty but valid)
  - Contains all 6 required packages
  - Contains 3 theorem environments (theorem, lemma, definition)

  **QA Scenarios**:

  ```
  Scenario: Validate template compiles
    Tool: Bash
    Steps:
      1. Extract raw LaTeX from the markdown code block
      2. Save to /tmp/test-template.tex
      3. Run: pdflatex -interaction=nonstopmode -output-directory=/tmp /tmp/test-template.tex
      4. Check exit code is 0
      5. Verify /tmp/test-template.pdf exists
    Expected Result: pdflatex exits 0, PDF file exists
    Failure Indicators: Non-zero exit code, missing packages error, no PDF output
    Evidence: .sisyphus/evidence/task-3-template-compilation.log

  Scenario: Validate required packages present
    Tool: Bash
    Steps:
      1. Run: grep -c "usepackage{amsmath}" algo-math-structurer/skills/algorithm-formalization/references/latex-template.md
      2. Run: grep -c "usepackage{amsthm}" ...
      3. Run: grep -c "usepackage{amssymb}" ...
      4. Run: grep -c "usepackage{algorithm2e}" ...
      5. Run: grep -c "usepackage{listings}" ...
      6. Run: grep -c "usepackage{tikz}" ...
    Expected Result: All 6 grep commands return count >= 1
    Failure Indicators: Any count = 0 (missing package)
    Evidence: .sisyphus/evidence/task-3-packages-validation.txt
  ```

  **Evidence to Capture**:
  - pdflatex compilation log
  - Generated PDF file existence check
  - Package grep results

  **Commit**: YES (groups with 1, 2)
  - Message: `chore(algo-math-structurer): scaffold plugin directory structure and manifest`
  - Files: `algo-math-structurer/skills/algorithm-formalization/references/latex-template.md`

---

- [x] 4. Core SKILL.md — Algorithm Formalization Workflow

  **What to do**:
  - Create `algo-math-structurer/skills/algorithm-formalization/SKILL.md` with:
    - **Frontmatter**: name `algorithm-formalization`, description with trigger phrases ("formalise cet algorithme", "génère les preuves mathématiques", "transforme ce code en théorèmes", "algo-math")
    - **Overview**: What the skill does — transforms Python AI algorithm code into formal mathematical document with proofs
    - **How It Works**: Visual diagram showing standalone workflow (user provides code → Claude analyzes → generates LaTeX → compiles PDF)
    - **Input Requirements**: What Claude needs (Python code, optionally: focus area, proof depth preference)
    - **Execution Flow** (numbered steps):
      1. **Parse**: Analyze Python code structure — identify functions, loops, recursion, base cases, variables, types
      2. **Extract**: Identify mathematical properties — preconditions, postconditions, loop invariants, termination conditions, complexity constraints
      3. **Formalize**: Translate to mathematical notation — define domains, operators, predicates
      4. **Prove**: Generate proofs using appropriate strategy (recurrence for recursion/loops, contradiction for impossibility, induction for recursive structures, Big-O for complexity)
      5. **Generate LaTeX**: Fill the template from references/latex-template.md with definitions, theorems, lemmas, proofs
      6. **Compile**: Run pdflatex via Bash to produce PDF
      7. **Handle Errors**: If pdflatex fails, return .tex + error log with suggestions
    - **Guardrails** (explicit):
      - NEVER hallucinate mathematical properties — if unidentifiable, state explicitly
      - NEVER claim a proof is complete if it relies on unproven assumptions — enumerate all assumptions
      - Cap analysis to ~200 lines or single function — beyond that, return scope error
      - Stochastic algorithms → probabilistic proofs, not deterministic
      - Approximation algorithms → state approximation bounds, not exact correctness
      - If algorithm has no loops/recursion → termination is trivial, state explicitly
      - Library calls (numpy, torch) → treat as black-box with stated assumptions
    - **Output Format**: Structured PDF with 5 sections: (1) algorithm pseudocode, (2) formal definitions, (3) stated properties, (4) proofs, (5) complexity analysis
    - **Edge Cases Handling**: syntax errors, empty input, trivial algorithms, mutually recursive functions, I/O side effects, unbounded input, nested algorithms, obfuscated code
    - **Progressive Disclosure**: Reference `references/latex-template.md` and `references/proof-patterns.md` for detailed content

  **Must NOT do**:
  - Exceed 3000 words in SKILL.md (put details in references/)
  - Include Coq/Lean code generation
  - Support non-Python languages
  - Support neural training loops, RL, generative models

  **Recommended Agent Profile**:
  - **Category**: `deep`
    - Reason: This is the core intellectual work — needs deep understanding of mathematical formalization, proof strategies, LaTeX generation, and Claude Cowork skill format
  - **Skills**: [`writing-skills`]
    - `writing-skills`: The SKILL.md must follow the Cowork skill format precisely with frontmatter, structured sections, and progressive disclosure

  **Parallelization**:
  - **Can Run In Parallel**: YES (after Wave 1)
  - **Parallel Group**: Wave 2 (with Tasks 5, 6)
  - **Blocks**: Tasks 6, 7
  - **Blocked By**: Tasks 1, 3

  **References**:
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/concepts/skills` — Skill structure, frontmatter, execution flow format
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/building/creating-skills` — Creating skills with best practices
  - `algo-math-structurer/skills/algorithm-formalization/references/latex-template.md` — LaTeX template to reference in the skill
  - `algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md` — Proof patterns to reference in the skill

  **WHY Each Reference Matters**:
  - Skills docs: Shows exact frontmatter format, section structure, and best practices for skill writing
  - Creating skills: Provides examples of execution flows and output formats
  - latex-template.md: The skill must reference this for LaTeX generation instructions
  - proof-patterns.md: The skill must reference this for proof strategy selection

  **Acceptance Criteria**:
  - SKILL.md exists with valid YAML frontmatter
  - Contains all required sections: Overview, How It Works, Input Requirements, Execution Flow, Guardrails, Output Format, Edge Cases
  - Word count < 3000
  - References both reference files
  - Guardrails section explicitly lists all anti-hallucination and scope rules

  **QA Scenarios**:

  ```
  Scenario: Validate SKILL.md structure
    Tool: Bash
    Steps:
      1. Run: grep -c "^---" algo-math-structurer/skills/algorithm-formalization/SKILL.md (should be 2)
      2. Run: grep -c "name: algorithm-formalization" ...
      3. Run: grep -c "description:" ...
      4. Run: grep -c "## Execution Flow" ...
      5. Run: Run: grep -c "## Guardrails" ...
      6. Run: wc -w algo-math-structurer/skills/algorithm-formalization/SKILL.md
    Expected Result: frontmatter present, all sections exist, word count < 3000
    Failure Indicators: Missing sections, word count >= 3000, no frontmatter
    Evidence: .sisyphus/evidence/task-4-skill-structure.txt

  Scenario: Validate guardrails are explicit
    Tool: Bash
    Steps:
      1. Run: grep -ic "hallucinate\|NEVER\|MUST NOT\|NE PAS" algo-math-structurer/skills/algorithm-formalization/SKILL.md
      2. Run: grep -ic "stochastic\|probabilistic" algo-math-structurer/skills/algorithm-formalization/SKILL.md
      3. Run: grep -ic "200 lines\|scope\|cap" algo-math-structurer/skills/algorithm-formalization/SKILL.md
    Expected Result: All grep counts > 0 (guardrails present)
    Failure Indicators: Any count = 0 (missing guardrail)
    Evidence: .sisyphus/evidence/task-4-guardrails-validation.txt
  ```

  **Evidence to Capture**:
  - Section validation output
  - Word count
  - Guardrail keyword presence

  **Commit**: YES (groups with 5, 6)
  - Message: `feat(algo-math-structurer): add core algorithm formalization skill with proof generation workflow`
  - Files: `algo-math-structurer/skills/algorithm-formalization/SKILL.md`

---

- [x] 5. Proof Patterns Reference

  **What to do**:
  - Create `algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md` containing:
    - **Proof by Recurrence (Induction)**:
      - When to use: Recursive functions, loops with counter
      - Structure: Base case → Inductive hypothesis → Inductive step
      - Example: Exponentiation rapide (as shown in spec)
      - Strong recurrence vs simple recurrence
    - **Proof by Contradiction**:
      - When to use: Impossibility proofs, optimality proofs
      - Structure: Assume negation → Derive contradiction → Conclude
      - Example: Proof that comparison-based sorting is Ω(n log n)
    - **Structural Induction**:
      - When to use: Recursive data structures (trees, lists)
      - Structure: Base case (empty structure) → Inductive step (add one element)
      - Example: Correctness of BST insertion
    - **Complexity Analysis (Big-O)**:
      - When to use: Any algorithm with loops/recursion
      - Structure: Count operations → Express as function of input size → Simplify to Big-O
      - Master theorem for divide-and-conquer
      - Amortized analysis mention
    - **Probabilistic Proofs**:
      - When to use: Randomized algorithms (Monte Carlo, Las Vegas)
      - Structure: Expected value, concentration bounds, failure probability
      - Example: Expected iterations of randomized quicksort
    - **Termination Proofs**:
      - When to use: Any recursive or iterative algorithm
      - Structure: Identify variant (decreasing measure) → Show bounded below → Conclude termination
      - Well-founded ordering
    - **Decision Guide**: Flowchart/table for selecting the right proof strategy based on algorithm characteristics

  **Must NOT do**:
  - Include Coq/Lean formalizations
  - Go beyond human-readable proof patterns
  - Include proofs for specific algorithms beyond examples

  **Recommended Agent Profile**:
  - **Category**: `unspecified-high`
    - Reason: Requires deep mathematical knowledge of proof techniques and ability to explain them clearly for Claude to follow
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - `writing-skills`: This is a technical reference, not creative writing

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 4, 6)
  - **Blocks**: Task 7
  - **Blocked By**: None (can start immediately)

  **References**:
  - External: `https://en.wikipedia.org/wiki/Mathematical_induction` — Induction proof structure
  - External: `https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)` — Master theorem for complexity

  **WHY Each Reference Matters**:
  - Induction docs: Canonical structure for recurrence proofs
  - Master theorem: Standard reference for divide-and-conquer complexity analysis

  **Acceptance Criteria**:
  - proof-patterns.md exists in references/
  - Contains all 6 proof types: recurrence, contradiction, structural induction, complexity, probabilistic, termination
  - Contains decision guide for proof selection
  - Each proof type includes: when to use, structure, example

  **QA Scenarios**:

  ```
  Scenario: Validate all proof types present
    Tool: Bash
    Steps:
      1. Run: grep -ic "récurrence\|induction" algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md
      2. Run: grep -ic "contradiction\|absurde" ...
      3. Run: grep -ic "structurelle" ...
      4. Run: grep -ic "Big-O\|complexité" ...
      5. Run: grep -ic "probabilistic\|stochastic\|aléatoire" ...
      6. Run: grep -ic "termination\|terminaison" ...
    Expected Result: All 6 grep counts > 0
    Failure Indicators: Any count = 0 (missing proof type)
    Evidence: .sisyphus/evidence/task-5-proof-types-validation.txt

  Scenario: Validate decision guide exists
    Tool: Bash
    Steps:
      1. Run: grep -ic "decision\|guide\|choisir\|select" algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md
    Expected Result: Count > 0 (decision guide present)
    Failure Indicators: Count = 0 (no decision guide)
    Evidence: .sisyphus/evidence/task-5-decision-guide.txt
  ```

  **Evidence to Capture**:
  - Grep results for each proof type
  - Decision guide presence check

  **Commit**: YES (groups with 4, 6)
  - Message: `feat(algo-math-structurer): add proof patterns reference with 6 proof strategies and decision guide`
  - Files: `algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md`

---

- [x] 6. Three Slash Commands

  **What to do**:
  - Create `algo-math-structurer/commands/generate-proof.md`:
    - Frontmatter: name `generate-proof`, description, parameters (code input, proof depth, output path)
    - Full workflow: receive Python code → analyze → generate LaTeX → compile PDF → return path
    - References the core skill for formalization logic
  - Create `algo-math-structurer/commands/analyze-complexity.md`:
    - Frontmatter: name `analyze-complexity`, description, parameters
    - Workflow: receive Python code → identify loops/recursion → derive Big-O → generate LaTeX section → compile
    - Focused output: complexity analysis only (not full proof document)
  - Create `algo-math-structurer/commands/extract-properties.md`:
    - Frontmatter: name `extract-properties`, description, parameters
    - Workflow: receive Python code → extract preconditions, postconditions, invariants → return structured list (no LaTeX)
    - Lightweight: quick analysis without proof generation

  **Must NOT do**:
  - Duplicate the core skill logic (commands reference the skill)
  - Add parameters not listed
  - Create commands beyond these 3

  **Recommended Agent Profile**:
  - **Category**: `quick`
    - Reason: Command files are straightforward markdown with frontmatter and workflow descriptions
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - `writing-skills`: Commands are simpler than skills, just frontmatter + workflow description

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 4, 5)
  - **Blocks**: Task 7
  - **Blocked By**: Tasks 1, 4 (needs plugin structure and core skill to reference)

  **References**:
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/concepts/commands` — Command structure and frontmatter
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/building/creating-commands` — Creating slash commands
  - `algo-math-structurer/skills/algorithm-formalization/SKILL.md` — Core skill to reference from commands

  **WHY Each Reference Matters**:
  - Commands docs: Shows exact frontmatter schema and file format for slash commands
  - Creating commands: Provides examples of parameter definitions and usage patterns
  - SKILL.md: Commands must reference the core skill for shared formalization logic

  **Acceptance Criteria**:
  - 3 command files exist in commands/
  - Each has valid YAML frontmatter with name, description
  - Each references the core skill
  - Each has distinct workflow (full proof, complexity-only, properties-only)

  **QA Scenarios**:

  ```
  Scenario: Validate all 3 commands exist with frontmatter
    Tool: Bash
    Steps:
      1. Run: ls algo-math-structurer/commands/generate-proof.md
      2. Run: ls algo-math-structurer/commands/analyze-complexity.md
      3. Run: ls algo-math-structurer/commands/extract-properties.md
      4. Run: grep -c "^---" algo-math-structurer/commands/generate-proof.md (should be 2)
      5. Run: grep -c "^---" algo-math-structurer/commands/analyze-complexity.md
      6. Run: grep -c "^---" algo-math-structurer/commands/extract-properties.md
    Expected Result: All 3 files exist, each has 2 frontmatter delimiters
    Failure Indicators: Missing files, missing frontmatter
    Evidence: .sisyphus/evidence/task-6-commands-structure.txt

  Scenario: Validate commands reference core skill
    Tool: Bash
    Steps:
      1. Run: grep -c "algorithm-formalization" algo-math-structurer/commands/generate-proof.md
      2. Run: grep -c "algorithm-formalization" algo-math-structurer/commands/analyze-complexity.md
      3. Run: grep -c "algorithm-formalization" algo-math-structurer/commands/extract-properties.md
    Expected Result: All 3 commands reference the core skill (count > 0)
    Failure Indicators: Any count = 0 (command doesn't reference skill)
    Evidence: .sisyphus/evidence/task-6-skill-references.txt
  ```

  **Evidence to Capture**:
  - File existence checks
  - Frontmatter validation
  - Skill reference checks

  **Commit**: YES (groups with 4, 5)
  - Message: `feat(algo-math-structurer): add 3 slash commands for proof generation, complexity analysis, and property extraction`
  - Files: `algo-math-structurer/commands/generate-proof.md`, `algo-math-structurer/commands/analyze-complexity.md`, `algo-math-structurer/commands/extract-properties.md`

---

- [x] 7. End-to-End Validation with Example Algorithm

  **What to do**:
  - Use the exponentiation rapide example from the spec as test input:
    ```python
    def puissance(a, n):
        if n == 0:
            return 1
        elif n % 2 == 0:
            return puissance(a * a, n // 2)
        else:
            return a * puissance(a * a, (n - 1) // 2)
    ```
  - Simulate the full workflow: invoke `/algo-math:generate-proof` with this code
  - Verify the generated LaTeX contains:
    - Definition of fast exponentiation algorithm
    - Theorem of correctness
    - Proof by strong recurrence (as shown in spec example)
    - Complexity analysis (O(log n))
  - Compile the LaTeX with pdflatex and verify PDF output
  - Test error handling: pass invalid Python code, verify error message
  - Test edge case: pass trivial code (`def f(): pass`), verify graceful handling

  **Must NOT do**:
  - Modify the skill or commands to make tests pass (fix the source, not the test)
  - Test algorithms outside the defined scope (neural networks, RL)

  **Recommended Agent Profile**:
  - **Category**: `deep`
    - Reason: End-to-end validation requires understanding the full plugin workflow, executing multiple steps, and verifying output quality
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - `verification-before-completion`: This IS the verification step, not a pre-completion check

  **Parallelization**:
  - **Can Run In Parallel**: NO
  - **Parallel Group**: Wave 3 (sequential with Task 8)
  - **Blocks**: Task 8, F1-F4
  - **Blocked By**: Tasks 2, 3, 4, 5, 6 (all plugin components must exist)

  **References**:
  - `algo-math-structurer/skills/algorithm-formalization/SKILL.md` — Core skill to validate
  - `algo-math-structurer/commands/generate-proof.md` — Command to invoke
  - `algo-math-structurer/skills/algorithm-formalization/references/latex-template.md` — Expected template
  - `algo-math-structurer/skills/algorithm-formalization/references/proof-patterns.md` — Expected proof patterns

  **WHY Each Reference Matters**:
  - SKILL.md: The validation tests whether the skill produces correct output
  - generate-proof.md: The command being tested end-to-end
  - latex-template.md: Verify the generated LaTeX follows the template
  - proof-patterns.md: Verify the proof strategy matches the algorithm type

  **Acceptance Criteria**:
  - PDF generated from exponentiation rapide example
  - PDF contains: definition, theorem, proof by recurrence, complexity analysis
  - pdflatex exits with code 0
  - Error handling works for invalid input
  - Edge case (trivial code) handled gracefully

  **QA Scenarios**:

  ```
  Scenario: Full workflow with exponentiation rapide
    Tool: Bash
    Steps:
      1. Create /tmp/test-puissance.tex with the expected LaTeX content for the puissance algorithm
      2. Run: pdflatex -interaction=nonstopmode -output-directory=/tmp /tmp/test-puissance.tex
      3. Check exit code is 0
      4. Verify /tmp/test-puissance.pdf exists and is > 10KB
      5. Run: grep -c "Théorème" /tmp/test-puissance.tex
      6. Run: grep -c "récurrence" /tmp/test-puissance.tex
      7. Run: grep -c "O(\log" /tmp/test-puissance.tex
    Expected Result: PDF exists, > 10KB, contains Théorème, récurrence, O(log n)
    Failure Indicators: pdflatex error, missing PDF, missing sections, PDF < 10KB
    Evidence: .sisyphus/evidence/task-7-e2e-puissance.pdf

  Scenario: Error handling with invalid Python
    Tool: Bash
    Steps:
      1. Create test input with invalid Python: "def f(: return"
      2. Run the skill workflow (simulate via reading SKILL.md instructions)
      3. Verify error message mentions syntax error
      4. Verify no broken LaTeX is produced
    Expected Result: Clear error message, no broken output
    Failure Indicators: Silent failure, broken LaTeX, crash
    Evidence: .sisyphus/evidence/task-7-error-handling.txt

  Scenario: Edge case with trivial code
    Tool: Bash
    Steps:
      1. Input: "def f(): pass"
      2. Run the skill workflow
      3. Verify output states termination is trivial
      4. Verify no vacuous proof is generated
    Expected Result: Explicit statement that termination is trivial, no fake proof
    Failure Indicators: Generated a meaningless proof, crash, empty output
    Evidence: .sisyphus/evidence/task-7-trivial-code.txt
  ```

  **Evidence to Capture**:
  - Generated PDF file
  - pdflatex compilation log
  - Error handling output
  - Edge case output

  **Commit**: YES (groups with 8)
  - Message: `test(algo-math-structurer): end-to-end validation with exponentiation rapide example`
  - Files: Evidence files in `.sisyphus/evidence/`

---

- [x] 8. Plugin Packaging (.plugin zip)

  **What to do**:
  - Package the complete plugin as a `.plugin` file (zip archive)
  - Command: `cd algo-math-structurer && zip -r ../algo-math-structurer.plugin . -x "*.DS_Store" "node_modules/*" "__pycache__/*"`
  - Verify the zip contains all required files
  - Document the packaging command in README.md

  **Must NOT do**:
  - Include .sisyphus/ directory in the zip
  - Include .DS_Store or other system files
  - Modify any plugin files during packaging

  **Recommended Agent Profile**:
  - **Category**: `quick`
    - Reason: Simple zip command and verification
  - **Skills**: []
  - **Skills Evaluated but Omitted**:
    - None needed

  **Parallelization**:
  - **Can Run In Parallel**: NO
  - **Parallel Group**: Wave 3 (sequential after Task 7)
  - **Blocks**: F1-F4
  - **Blocked By**: Task 7 (validation must pass first)

  **References**:
  - Official docs: `https://www.mintlify.com/anthropics/knowledge-work-plugins/building/plugin-structure` — Packaging plugins section

  **WHY Each Reference Matters**:
  - Plugin structure doc: Shows the exact zip command and exclusion patterns for packaging

  **Acceptance Criteria**:
  - `algo-math-structurer.plugin` file exists
  - Zip contains: `.claude-plugin/plugin.json`, `skills/`, `commands/`, `.mcp.json`, `README.md`
  - Zip does NOT contain: `.sisyphus/`, `.DS_Store`, `node_modules/`

  **QA Scenarios**:

  ```
  Scenario: Validate plugin zip contents
    Tool: Bash
    Steps:
      1. Run: unzip -l algo-math-structurer.plugin
      2. Verify output contains: .claude-plugin/plugin.json, skills/algorithm-formalization/SKILL.md, commands/, .mcp.json, README.md
      3. Verify output does NOT contain: .sisyphus/, .DS_Store
    Expected Result: All required files present, no forbidden files
    Failure Indicators: Missing required files, presence of forbidden files
    Evidence: .sisyphus/evidence/task-8-zip-contents.txt

  Scenario: Validate zip is valid archive
    Tool: Bash
    Steps:
      1. Run: unzip -t algo-math-structurer.plugin
      2. Check exit code is 0
    Expected Result: Exit code 0 (valid zip)
    Failure Indicators: Non-zero exit code, "corrupt zip" error
    Evidence: .sisyphus/evidence/task-8-zip-valid.txt
  ```

  **Evidence to Capture**:
  - Zip listing output
  - Zip test output

  **Commit**: YES (groups with 7)
  - Message: `chore(algo-math-structurer): package plugin as .plugin zip file`
  - Files: `algo-math-structurer.plugin`

---

## Final Verification Wave (MANDATORY — after ALL implementation tasks)

> 4 review agents run in PARALLEL. ALL must APPROVE. Present consolidated results to user and get explicit "okay" before completing.
>
> **Do NOT auto-proceed after verification. Wait for user's explicit approval before marking work complete.**
> **Never mark F1-F4 as checked before getting user's okay.** Rejection or user feedback -> fix -> re-run -> present again -> wait for okay.

- [x] F1. **Plan Compliance Audit** — `oracle`
  Read the plan end-to-end. For each "Must Have": verify implementation exists (read file, check content). For each "Must NOT Have": search codebase for forbidden patterns — reject with file:line if found. Check evidence files exist in .sisyphus/evidence/. Compare deliverables against plan. Verify plugin.json has all required fields. Verify SKILL.md has frontmatter + all required sections. Verify 3 commands exist with frontmatter. Verify LaTeX template compiles.
  Output: `Must Have [N/N] | Must NOT Have [N/N] | Tasks [N/N] | VERDICT: APPROVE/REJECT`

- [x] F2. **Plugin Quality Review** — `unspecified-high`
  Review all plugin files: plugin.json valid JSON, .mcp.json valid JSON, SKILL.md < 3000 words, all commands have frontmatter, LaTeX template has all 6 packages, proof-patterns.md has all 6 proof types. Check for: incomplete sections, placeholder text, broken references, missing guardrails. Verify no Coq/Lean code, no multi-language support, no neural network scope creep.
  Output: `JSON [PASS/FAIL] | Structure [PASS/FAIL] | Guardrails [N/N present] | Scope [CLEAN/N violations] | VERDICT`

- [x] F3. **Real Manual QA** — `unspecified-high`
  Start from clean state. Execute the exponentiation rapide example through the full workflow: read SKILL.md instructions → verify LaTeX generation → run pdflatex → verify PDF output contains all 5 sections. Test error handling with invalid Python. Test edge case with trivial code. Verify .plugin zip contains all required files and no forbidden files. Save to `.sisyphus/evidence/final-qa/`.
  Output: `Scenarios [N/N pass] | PDF [valid/invalid] | Error Handling [PASS/FAIL] | Zip [PASS/FAIL] | VERDICT`

- [x] F4. **Scope Fidelity Check** — `deep`
  For each task: read "What to do", read actual files. Verify 1:1 — everything in spec was built (no missing), nothing beyond spec was built (no creep). Check "Must NOT do" compliance: no Coq/Lean, no multi-language, no neural training, no interactive refinement, no external proof verification. Detect cross-task contamination. Flag unaccounted changes.
  Output: `Tasks [N/N compliant] | Contamination [CLEAN/N issues] | Unaccounted [CLEAN/N files] | Scope Creep [CLEAN/N violations] | VERDICT`

---

## Commit Strategy

- **1-3**: `chore(algo-math-structurer): scaffold plugin directory structure and manifest` — plugin.json, .mcp.json, README.md, latex-template.md
- **4-6**: `feat(algo-math-structurer): add core algorithm formalization skill with proof generation workflow` — SKILL.md, proof-patterns.md, 3 commands
- **7-8**: `test(algo-math-structurer): end-to-end validation and packaging` — evidence files, .plugin zip

---

## Success Criteria

### Verification Commands
```bash
ls algo-math-structurer/.claude-plugin/plugin.json  # Expected: file exists
python3 -c "import json; json.load(open('algo-math-structurer/.claude-plugin/plugin.json'))"  # Expected: exit 0
pdflatex -interaction=nonstopmode -output-directory=/tmp test.tex  # Expected: exit 0, PDF produced
unzip -t algo-math-structurer.plugin  # Expected: valid archive, all files present
```

### Final Checklist
- [ ] All "Must Have" present (plugin.json, SKILL.md, 3 commands, LaTeX template, proof patterns, README)
- [ ] All "Must NOT Have" absent (no Coq/Lean, no multi-language, no neural training, no interactive refinement)
- [ ] pdflatex compiles example LaTeX to PDF successfully
- [ ] .plugin zip packages all files correctly
- [ ] Guardrails explicitly stated in SKILL.md
- [ ] All evidence files captured in .sisyphus/evidence/
