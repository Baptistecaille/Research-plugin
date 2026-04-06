---
name: extract-properties
description: "Extract formal properties from Python algorithm code: preconditions, postconditions, invariants, termination conditions."
---

# /algo-math:extract-properties

Extract formal mathematical properties from Python algorithm code.

## Parameters
- `code` (required): Python algorithm code to analyze

## Workflow

1. Receive Python algorithm code from user
2. Invoke the `algorithm-formalization` skill to:
   - Parse the code structure
   - Extract preconditions (input constraints, type requirements)
   - Extract postconditions (output guarantees)
   - Extract loop invariants
   - Extract termination conditions
3. Return a structured list of properties (no LaTeX generation)
4. This is a lightweight analysis — no proof generation

## References
- Core skill: `skills/algorithm-formalization/SKILL.md`

## Usage
```
/algo-math:extract-properties

Then paste your Python algorithm code.
```
