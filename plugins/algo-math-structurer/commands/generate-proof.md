---
name: generate-proof
description: "Generate a complete mathematical proof document from Python algorithm code. Produces LaTeX compiled to PDF with definitions, theorems, proofs, and complexity analysis."
---

# /algo-math:generate-proof

Generate a complete mathematical proof document from Python algorithm code.

## Parameters
- `code` (required): Python algorithm code to analyze
- `focus` (optional): Focus area — "all" (default), "correctness", "termination", "complexity"
- `depth` (optional): Proof depth — "concise" (default), "detailed"

## Workflow

1. Receive Python algorithm code from user
2. Invoke the `algorithm-formalization` skill to:
   - Parse the code structure
   - Extract mathematical properties
   - Generate formal proofs
   - Produce LaTeX document
3. Compile LaTeX to PDF using pdflatex
4. Return the PDF path to the user

## References
- Core skill: `skills/algorithm-formalization/SKILL.md`
- LaTeX template: `skills/algorithm-formalization/references/latex-template.md`
- Proof patterns: `skills/algorithm-formalization/references/proof-patterns.md`

## Usage
```
/algo-math:generate-proof

Then paste your Python algorithm code.
```
