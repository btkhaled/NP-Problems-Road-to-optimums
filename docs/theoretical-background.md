# Theoretical Background — NP-Completeness and Reductions

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

## 1. Complexity Classes

### P (Polynomial Time)
Problems solvable by a deterministic Turing machine in time O(n^k) for some constant k.

### NP (Nondeterministic Polynomial Time)
Problems whose solutions can be verified by a deterministic Turing machine in polynomial time. Equivalently, problems solvable by a nondeterministic Turing machine in polynomial time.

### NP-hard
A problem H is NP-hard if every problem in NP can be reduced to H in polynomial time. NP-hard problems are at least as hard as the hardest problems in NP.

### NP-complete
A problem is NP-complete if it is both in NP and NP-hard. These are the "hardest" problems in NP.

**Relationship:** P ⊆ NP. Whether P = NP is the most important open problem in theoretical computer science (one of the Clay Millennium Problems).

---

## 2. Cook-Levin Theorem (1971)

**Statement:** The Boolean Satisfiability Problem (SAT) is NP-complete.

**Significance:**
- First problem proven to be NP-complete
- Provided the foundation for the entire theory of NP-completeness
- Established that any problem in NP can be reduced to SAT in polynomial time

**Proof sketch:**
1. SAT is in NP (a satisfying assignment can be verified in linear time)
2. For any problem in NP, construct a Boolean formula that simulates the computation of the nondeterministic Turing machine
3. The formula is satisfiable iff the Turing machine accepts

---

## 3. Karp's 21 NP-Complete Problems (1972)

Richard Karp published a landmark paper showing 21 problems from diverse areas (graph theory, combinatorics, logic, etc.) to be NP-complete via polynomial reductions from SAT.

### The 21 Problems

| # | Problem | Area |
|---|---|---|
| 1 | **SAT** (Satisfiability) | Logic |
| 2 | **3-SAT** (3-Conjunctive Normal Form SAT) | Logic |
| 3 | **CLIQUE** | Graph Theory |
| 4 | **VERTEX COVER** | Graph Theory |
| 5 | **HAMILTONIAN CYCLE** | Graph Theory |
| 6 | **TRAVELING SALESMAN PROBLEM** | Graph Theory |
| 7 | **SUBSET SUM** | Number Theory |
| 8 | **KNAPSACK** (0-1 Integer Programming) | Number Theory |
| 9 | SET PACKING | Combinatorics |
| 10 | SET COVER | Combinatorics |
| 11 | SET PARTITION | Combinatorics |
| 12 | HITTING SET | Combinatorics |
| 13 | FEEDBACK ARC SET | Graph Theory |
| 14 | FEEDBACK VERTEX SET | Graph Theory |
| 15 | DIRECTED HAMILTONIAN CIRCUIT | Graph Theory |
| 16 | GRAPH COLORING | Graph Theory |
| 17 | CLIQUE COVER | Graph Theory |
| 18 | EXACT COVER | Combinatorics |
| 19 | STEINER TREE | Graph Theory |
| 20 | MAX CUT | Graph Theory |
| 21 | MINIMUM EQUIVALENT DIGRAPH | Graph Theory |

**Three of Karp's 21 problems are addressed in this repository:**
- #2: 3-SAT
- #6: Traveling Salesman Problem
- #8: 0-1 Knapsack

---

## 4. Reductions Between Our Three Problems

### SAT ≤_p 3-SAT
**Method:** Every SAT clause can be transformed into CNF, then each clause can be converted to 3-CNF by adding auxiliary variables.

**Example:** Clause (a ∨ b) becomes (a ∨ b ∨ z) ∧ (a ∨ b ∨ ¬z) with a fresh variable z.

### 3-SAT ≤_p KNAPSACK
**Method:** Given a 3-CNF formula with n variables and m clauses, construct a knapsack instance with 2n + 2m items and a carefully chosen capacity. Each variable corresponds to two items (true/false assignment), each clause corresponds to two slack items. The construction ensures a knapsack solution of a target value exists iff the formula is satisfiable.

### 3-SAT ≤_p TSP
**Method:** Construct a graph with a vertex for each literal and special vertices for each clause. The TSP tour must visit all vertices exactly once. Edge costs are designed so that a tour of length ≤ threshold exists iff the formula is satisfiable. This is typically done via the SUBGRAPH problem or directly through gadget construction.

### TSP ≤_p KNAPSACK
**Method:** Encode TSP as an integer programming problem, then reduce to 0-1 knapsack. This uses the general reduction from ILP to knapsack, though it is less direct than SAT → KP.

---

## 5. Phase Transitions

Many NP-complete problems exhibit a phase transition where instances transition from almost-surely satisfiable to almost-surely unsatisfiable.

### 3-SAT Phase Transition
- Control parameter: clause/variable ratio (m/n)
- Threshold: ~4.2
- Below threshold: most instances satisfiable (underconstrained)
- Above threshold: most instances unsatisfiable (overconstrained)
- At threshold: hardest instances (near 50% satisfiable)

**Our instances:** UF250 has m/n = 1065/250 = **4.26**, just above the threshold, placing them in the hard region.

### KNAPSACK Phase Transition
- Correlated with the correlation between value and weight
- Uncorrelated: easier (greedy works well)
- Weakly correlated: moderate difficulty
- Strongly correlated: hardest (value = weight + constant offset)

**Our unsolved instances** (knapPI_3 family) are strongly correlated, explaining their difficulty.

### TSP Phase Transition
- Not as clearly defined as SAT
- Instance difficulty correlates with the distribution of city coordinates
- Random Euclidean instances are easier than structured instances
- The hardest instances are those from real-world applications (e.g., PCB routing)

---

## 6. Practical Implications

The three problems in this repository occupy a special position in the landscape of NP-complete problems:

| Property | 0-1 Knapsack | 3-SAT | TSP |
|---|---|---|---|
| **Karp's list** | ✓ (#8) | ✓ (#2) | ✓ (#6) |
| **Pseudo-polynomial?** | Yes (DP O(nW)) | No | No |
| **FPTAS?** | Yes | No (unless P=NP) | No (unless P=NP) |
| **Approximable?** | No (unless P=NP) | No (unless P=NP) | Metric: 1.5-approx |
| **Phase transition** | Correlation-based | Clause/variable ratio | Problem structure |
| **Our approach** | Exact (B&B) + Heuristic (GA) | Stochastic (WalkSAT) | Metaheuristic (GA+LS) |
| **Success rate** | 90% | 100% | 76% exact |

---

## References

- Cook, S. A. (1971). The complexity of theorem-proving procedures. *STOC*, 151–158.
- Karp, R. M. (1972). Reducibility among combinatorial problems. *Complexity of Computer Computations*, 85–103.
- Garey, M. R., & Johnson, D. S. (1979). *Computers and Intractability: A Guide to the Theory of NP-Completeness*. W. H. Freeman.
- Mitchell, D., Selman, B., & Levesque, H. (1992). Hard and easy distributions of SAT problems. *AAAI*, 459–465.
- Pisinger, D. (2005). Where are the hard knapsack problems? *Computers & Operations Research*, 32(9), 2271–2284.
