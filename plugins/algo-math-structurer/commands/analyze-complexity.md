---
name: analyze-complexity
description: "Analyze time and space complexity of a Python algorithm. Produces Big-O analysis with derivation."
---

# /algo-math:analyze-complexity

Analyze the time and space complexity of a Python algorithm.

## Parameters
- `code` (required): Python algorithm code to analyze

## Workflow

1. Receive Python algorithm code from user
2. Invoke the `algorithm-formalization` skill to:
   - Identify loops, recursion, and control flow
   - Derive recurrence relations if applicable
   - Apply Master theorem for divide-and-conquer
   - Compute Big-O time and space complexity
3. Generate a focused LaTeX document with complexity analysis only
4. Compile to PDF and return the path

## References
- Core skill: `skills/algorithm-formalization/SKILL.md`
- Proof patterns: `skills/algorithm-formalization/references/proof-patterns.md` (Complexity Analysis section)

## Usage
```
/algo-math:analyze-complexity

Then paste your Python algorithm code.
```
