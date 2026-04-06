# algo-math-structurer

Transform Python algorithm code into formal mathematical documents with proofs, exported as LaTeX and compiled to PDF.

## Description

`algo-math-structurer` is a Claude Cowork plugin that bridges the gap between implementation and formal mathematics. It takes Python algorithm code - search, sort, graph, optimization, dynamic programming, clustering - and produces rigorously structured mathematical documents complete with definitions, theorems, proofs by recurrence, and complexity analysis.

The output is a LaTeX document compiled to PDF, suitable for academic papers, technical documentation, or educational materials.

## Skills

| Skill | Description |
|---|---|
| `algorithm-formalization` | Analyzes Python algorithm implementations and generates formal mathematical specifications: preconditions, postconditions, loop invariants, recurrence relations, and proofs. |

## Commands

| Command | Description |
|---|---|
| `/algo-math:generate-proof` | Full proof generation: reads Python code, produces complete LaTeX document with definitions, theorems, proofs, and complexity analysis. Compiles to PDF. |
| `/algo-math:analyze-complexity` | Big-O complexity analysis only. Extracts time and space complexity from the algorithm's control flow and recursion structure. |
| `/algo-math:extract-properties` | Extracts formal properties: preconditions, postconditions, loop invariants, and termination conditions. No proof generation. |

## Setup Instructions

1. **Install from marketplace**: In Claude Cowork, type `/plugin`, choose **Add marketplace**, and select this repository. Then install `algo-math-structurer`.

2. **Local development**: If you are testing the plugin directly from disk, copy the `algo-math-structurer/` directory into your Claude Cowork plugins folder:
   ```bash
   cp -r algo-math-structurer ~/.claude-cowork/plugins/
   ```

3. **Verify installation**: Restart Claude Cowork. The three `/algo-math:*` commands should appear in the command palette.

3. **LaTeX dependency**: Ensure `pdflatex` (or `xelatex`) is installed on your system for PDF compilation:
   ```bash
   # macOS
   brew install --cask mactex

   # Ubuntu/Debian
   sudo apt install texlive-full
   ```

## Packaging

- `algo-math-structurer/` is the source tree you edit locally.
- `algo-math-structurer.plugin` is the packaged archive ready to distribute or import.
- Both contain the same plugin contents, including `.claude-plugin/plugin.json`, `commands/`, `skills/`, and `.mcp.json`.

## Usage Examples

### Exponentiation Rapide

**Input** (`algorithms/exponentiation.py`):
```python
def puissance(a, n):
    if n == 0: return 1
    elif n % 2 == 0: return puissance(a * a, n // 2)
    else: return a * puissance(a * a, (n - 1) // 2)
```

**Run**: `/algo-math:generate-proof`

**Output** — PDF containing:

- **Definition**: Recursive binary exponentiation function $f: \mathbb{R} \times \mathbb{N} \to \mathbb{R}$
- **Theorem**: For all $a \in \mathbb{R}, n \in \mathbb{N}$, $f(a, n) = a^n$
- **Proof by recurrence**: Base case $n=0$, even case $n=2k$, odd case $n=2k+1$
- **Complexity analysis**: $O(\log n)$ time, $O(\log n)$ stack space

### Complexity Analysis Only

**Run**: `/algo-math:analyze-complexity` on a merge sort implementation.

**Output**: Time $O(n \log n)$, Space $O(n)$, with derivation from recurrence $T(n) = 2T(n/2) + O(n)$.

## Limitations

- **Language**: Python only. No TypeScript, Java, C++, or other languages.
- **Algorithm scope**: Classical algorithms — search, sort, graph traversal, optimization, dynamic programming, clustering.
- **Excluded**: Neural network training loops, reinforcement learning, generative models, heuristic black-box algorithms.
- **Proof depth**: Proofs are formal but not machine-checked. No Coq, Lean, or Isabelle output.
- **Interactive refinement**: No iterative proof refinement loop. Single-pass generation.

## Customization Tips

- **Extend algorithm coverage**: Add new algorithm patterns to the `algorithm-formalization` skill's reference library in `skills/algorithm-formalization/SKILL.md`.
- **Adjust proof style**: Modify the LaTeX template sections in the skill to change theorem formatting, notation conventions, or proof structure.
- **Add complexity templates**: For new algorithm families, add recurrence relation templates to the skill's complexity analysis section.
- **Custom LaTeX preamble**: Edit the skill's LaTeX preamble to include your institution's style, custom macros, or bibliography format.
