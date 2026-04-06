# Proof Patterns Reference

Decision guide and proof structures for algorithm formalization.

---

## Decision Guide: Which Proof Strategy?

| Algorithm Characteristic | Proof Strategy |
|--------------------------|---------------|
| Recursive function with base case | Recurrence (Induction) |
| Loop with counter/iterator | Recurrence on loop iterations |
| Recursive data structure (tree, list) | Structural Induction |
| Impossibility or optimality claim | Proof by Contradiction |
| Time/space complexity question | Big-O Analysis |
| Randomized algorithm | Probabilistic Proof |
| Any algorithm | Termination Proof |

---

## 1. Proof by Recurrence (Induction)

**When to use:** Recursive functions, loops with counter, divide-and-conquer algorithms.

**Structure:**
1. **Base case:** Verify property holds for smallest input (n=0, n=1, empty structure)
2. **Inductive hypothesis:** Assume property holds for all k < n
3. **Inductive step:** Show property holds for n using the hypothesis

**Strong vs Simple:**
- Simple recurrence: P(n) → P(n+1)
- Strong recurrence: P(k) for all k < n → P(n)

**Example — Exponentiation Rapide:**
```
Theorem: For all a ∈ ℝ, n ∈ ℕ, puissance(a, n) = a^n.

Proof by strong recurrence on n:
- Base case (n=0): puissance(a, 0) = 1 = a^0. ✓
- Inductive step: Assume true for all k < n.
  - If n = 2m (even): puissance(a, 2m) = puissance(a², m) = (a²)^m = a^(2m) = a^n. ✓
  - If n = 2m+1 (odd): puissance(a, 2m+1) = a × puissance(a², m) = a × a^(2m) = a^(2m+1) = a^n. ✓
```

---

## 2. Proof by Contradiction

**When to use:** Impossibility proofs, optimality proofs, lower bounds.

**Structure:**
1. Assume the negation of what you want to prove
2. Derive a logical contradiction
3. Conclude the original statement must be true

**Example — Comparison Sort Lower Bound:**
```
Theorem: Any comparison-based sorting algorithm requires Ω(n log n) comparisons.

Proof by contradiction:
Assume there exists a comparison sort using o(n log n) comparisons.
The decision tree has at most 2^(o(n log n)) leaves.
But there are n! possible permutations, requiring n! leaves.
By Stirling's approximation: n! = Ω((n/e)^n) = 2^Ω(n log n).
Contradiction: 2^(o(n log n)) < 2^Ω(n log n) for large n.
Therefore, Ω(n log n) comparisons are necessary.
```

---

## 3. Structural Induction

**When to use:** Recursive data structures (trees, lists, graphs), algorithms that process them.

**Structure:**
1. **Base case:** Property holds for empty/minimal structure
2. **Inductive step:** If property holds for substructures, it holds for the constructed structure

**Example — BST Insertion Correctness:**
```
Theorem: Inserting a value into a BST preserves the BST property.

Proof by structural induction on the tree:
- Base case (empty tree): Insertion creates single-node tree. BST property holds trivially.
- Inductive step: Assume insertion preserves BST property for left and right subtrees.
  - If value < root: insert into left subtree. By IH, left subtree remains BST.
    Since value < root, BST property at root is preserved.
  - If value ≥ root: insert into right subtree. By IH, right subtree remains BST.
    Since value ≥ root, BST property at root is preserved.
```

---

## 4. Complexity Analysis (Big-O)

**When to use:** Any algorithm with loops or recursion.

**Structure:**
1. Identify the basic operation (comparison, assignment, recursive call)
2. Count operations as function of input size n
3. Simplify to Big-O notation (drop constants, keep dominant term)

**Common Patterns:**
- Single loop: O(n)
- Nested loops: O(n²)
- Divide-and-conquer: Use Master Theorem T(n) = a·T(n/b) + f(n)
  - If f(n) = O(n^(log_b(a) - ε)): T(n) = Θ(n^(log_b(a)))
  - If f(n) = Θ(n^(log_b(a))): T(n) = Θ(n^(log_b(a)) · log n)
  - If f(n) = Ω(n^(log_b(a) + ε)): T(n) = Θ(f(n))

**Example — Merge Sort:**
```
T(n) = 2T(n/2) + O(n)  (divide + merge)
a=2, b=2, f(n) = O(n)
log_b(a) = log_2(2) = 1
f(n) = Θ(n^1) = Θ(n^(log_b(a))) → Case 2
T(n) = Θ(n log n)
```

---

## 5. Probabilistic Proofs

**When to use:** Randomized algorithms (Monte Carlo, Las Vegas), algorithms with random choices.

**Structure:**
1. Define the random variable of interest (running time, correctness probability)
2. Compute expected value E[X]
3. Apply concentration bounds (Markov, Chebyshev, Chernoff) if needed
4. State failure probability

**Example — Randomized Quicksort:**
```
Theorem: Expected running time of randomized quicksort is O(n log n).

Proof:
Let X be the total number of comparisons.
X = Σ_{i<j} X_{ij} where X_{ij} = 1 if elements i,j are compared.
P(X_{ij} = 1) = 2/(j-i+1) (i and j compared iff one is first pivot in their range)
E[X] = Σ_{i<j} 2/(j-i+1) ≤ 2n · Σ_{k=1}^{n} 1/k = 2n · H_n = O(n log n)
```

---

## 6. Termination Proofs

**When to use:** Any recursive or iterative algorithm.

**Structure:**
1. Identify a **variant** (measure that decreases each step)
2. Show the variant is **bounded below** (usually by 0)
3. Show the variant **strictly decreases** each iteration/call
4. Conclude: since a well-founded ordering cannot decrease infinitely, the algorithm terminates

**Example — Euclidean Algorithm:**
```
Theorem: gcd(a, b) terminates for all a, b ∈ ℕ, b > 0.

Proof:
Variant: the second argument b.
- b ∈ ℕ, so b ≥ 0 (bounded below)
- Each recursive call: gcd(b, a mod b), and a mod b < b (strictly decreases)
- By well-founded ordering on ℕ, the sequence must terminate.
```
