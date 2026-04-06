---
name: algorithm-formalization
description: "Transform Python algorithm code into formal mathematical documents with proofs. Trigger with 'formalise cet algorithme', 'génère les preuves mathématiques', 'transforme ce code en théorèmes', 'algo-math', 'prove this algorithm'."
---

# Algorithm Formalization

Transform Python algorithm code into formal mathematical documents with definitions, theorems, proofs, and complexity analysis - compiled to PDF via pdflatex.

## How It Works

1. **Parse** → Analyze Python code structure (functions, loops, recursion, base cases)
2. **Extract** → Identify mathematical properties (preconditions, postconditions, invariants, termination, complexity)
3. **Formalize** → Translate to mathematical notation (domains, operators, predicates)
4. **Prove** → Generate proofs using appropriate strategy (see `references/proof-patterns.md`)
5. **Generate LaTeX** → Fill template from `references/latex-template.md`
6. **Compile** → Run `pdflatex -interaction=nonstopmode document.tex`
7. **Handle Errors** → If pdflatex fails, return .tex + error log

## Input Requirements

**Required:** Python algorithm code (function or module)
**Optional:** Focus area (correctness, termination, complexity, all), proof depth (concise, detailed)

## Execution Flow

### Step 1: Parse
Analyze Python code: functions, parameters, return values, control flow (loops, recursion, conditionals), base cases, variable types.

### Step 2: Extract
Identify: preconditions, postconditions, loop invariants, termination conditions, complexity constraints.

### Step 3: Formalize
Define domains (ℕ, ℤ, ℝ), operators, functions, predicates using standard mathematical notation.

### Step 4: Prove
Consult `references/proof-patterns.md` for the decision guide. Select proof strategy:
- Recurrence/Induction → recursive functions, loops
- Contradiction → impossibility, optimality
- Structural Induction → recursive data structures
- Big-O → complexity bounds
- Probabilistic → randomized algorithms
- Termination → variant functions, well-founded ordering

### Step 5: Generate LaTeX
Fill `references/latex-template.md` with: algorithm pseudocode, definitions (`definition` env), theorems (`theorem` env), lemmas (`lemma` env), proofs (`proof` env), complexity analysis.

### Step 6: Compile
```bash
pdflatex -interaction=nonstopmode document.tex
```
Run twice for references.

### Step 7: Handle Errors
If pdflatex fails: return raw .tex + compilation log + suggested fixes.

## Guardrails

**NEVER hallucinate mathematical properties.** If unidentifiable, state: "No [property] identified."

**NEVER claim proof complete with unproven assumptions.** Enumerate ALL assumptions explicitly.

**Cap analysis to ~200 lines or single function.** Beyond: "Input exceeds analysis scope."

**Stochastic algorithms → probabilistic proofs**, NOT deterministic.

**Approximation algorithms → state bounds**, NOT exact correctness.

**No loops/recursion → trivial termination.** State explicitly. No vacuous proof.

**Library calls (numpy, torch) → black-box** with stated assumptions.

**Python only.** Reject other languages.

**Classical algorithms only.** Reject neural training, RL, generative models.

## Output Format

PDF with 5 sections:
1. **Algorithme** — Pseudocode
2. **Définitions** — Formal definitions
3. **Théorèmes** — Stated properties
4. **Preuves** — Formal proofs
5. **Analyse de Complexité** — Time/space complexity

## Edge Cases

| Case | Handling |
|------|----------|
| Syntax errors | Report, best-effort on parseable portions |
| Empty/trivial input | "Input does not contain an analyzable algorithm." |
| No loops/recursion | Termination trivial, skip vacuous proof |
| Mutually recursive | Analyze as unit, joint termination proof |
| I/O side effects | Excluded from formal model |
| External libraries | Black-box with stated assumptions |
| Unbounded input | Termination assumes finite input |
| Nested algorithms | Analyze outer, inner as assumptions |
| Obfuscated code | Normalize variable names |

## References

- **LaTeX Template**: `references/latex-template.md`
- **Proof Patterns**: `references/proof-patterns.md`
