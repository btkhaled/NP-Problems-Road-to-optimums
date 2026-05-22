# The NP-Hard Triangle: SAT, Knapsack, and TSP — A Comparative Empirical Study

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

# Abstract

This paper presents a comparative empirical study of three fundamental NP-hard problems: Boolean 3-Satisfiability (3-SAT), the 0-1 Knapsack Problem, and the Traveling Salesman Problem. Each problem is tackled with a distinct hybrid algorithmic approach: a WalkSAT + GSAT stochastic local search for SAT3, a Genetic Algorithm + best-first Branch-and-Bound for Knapsack, and a Genetic Algorithm + 2-opt/3-opt local search for TSP.

Across 155 benchmark instances from standard libraries (SATLIB UF250, Pisinger's KP collection, and TSPLIB), the combined framework achieves a **94.2% overall success rate** (146/155), including a perfect score on SAT3 (100/100), strong performance on Knapsack (27/30, 90%), and 19/25 exact optimums on TSP with near-optimal solutions for the remainder.

**Keywords:** NP-complete problems, 3-SAT, 0-1 Knapsack, Traveling Salesman, WalkSAT, Branch-and-Bound, Genetic Algorithm, empirical comparison.

---

## 1. Introduction

The theory of NP-completeness, established by Cook (1971) and Karp (1972), identifies a large class of combinatorial problems that are widely believed to require exponential time in the worst case. Three of the most well-studied NP-complete problems are:

1. **3-SAT** (Karp #2): Given a Boolean formula in conjunctive normal form with exactly three literals per clause, does there exist a satisfying assignment?
2. **0-1 Knapsack** (Karp #8): Given items with values and weights, and a capacity, select a subset maximizing total value without exceeding capacity.
3. **TSP** (Karp #6): Given cities and pairwise distances, find the shortest tour visiting each city exactly once.

Despite their shared NP-complete status, these three problems have fundamentally different structural properties that make them amenable to different algorithmic approaches. This study explores these differences empirically by applying hybrid solvers to standard benchmark libraries.

---

## 2. The Three Problems: A Structural Comparison

### 2.1 Constraints and Search Space

| Property | 3-SAT | 0-1 Knapsack | TSP |
|---|---|---|---|
| Decision variables | n = 250 Boolean | n = 4–10,000 binary | n = 51–575 ordered |
| Constraints | m = 1065 clauses | 1 (capacity) | n (degree ≥ 2) |
| Solution representation | Truth assignment | Bitstring | Permutation |
| Search space size | 2^n | 2^n | (n-1)!/2 |
| NP-complete for | Any m/n > 0 | General case | General case |

### 2.2 Algorithmic Implications

**SAT3** has many tight constraints but a simple Boolean search space. Stochastic local search excels here because:
- Flipping a single variable has a predictable effect on clause satisfaction
- The make/break heuristic provides strong guidance
- The search landscape has many solutions (at ratio 4.26)

**0-1 Knapsack** has a single constraint but the objective matters. Exact methods work well because:
- The fractional bound provides strong pruning
- The value-to-weight ratio ordering is highly informative
- The problem is pseudo-polynomial (can be solved in O(nW) via DP)

**TSP** has an astronomically large search space with a complex objective function:
- No simple bound exists (unlike knapsack)
- Local search neighborhoods (2-opt, 3-opt) are well-understood
- Edge-based representations preserve useful structure

---

## 3. Our Approach

### 3.1 SAT3: WalkSAT + GSAT Hybrid

The SAT3 solver combines WalkSAT's stochastic noise strategy with GSAT's greedy scoring:

```
1. Initialize with majority-polarity heuristic
2. While not satisfiable and iterations < max:
   a. Pick a random unsatisfied clause C
   b. For each variable in C, compute make/break scores
   c. If any variable has break = 0 → flip it (greedy)
   d. Else with prob p: flip random variable in C (walk)
   e. Else with prob 1-p: flip lowest-break variable (greedy)
3. If stalled: adaptive restart with targeted perturbation
```

**Key innovation:** Incremental clause-state tracking reduces per-flip cost from O(m) to O(degree(x)). This enables millions of flips per second.

**Result:** 100/100 instances solved. Average runtime: 5.34s.

### 3.2 0-1 Knapsack: GA + Branch-and-Bound

Two complementary solvers:

**Genetic Algorithm** (heuristic):
- Greedy seeding via longest-neighbor construction
- Bitstring representation, uniform crossover, bit-flip mutation
- inverted_opt4 local improvement
- Population size 50, 100 generations

**Branch-and-Bound** (exact):
- Items sorted by value-to-weight ratio
- Fractional Dantzig bound via binary search on cumulative arrays
- Best-first exploration (max-heap priority queue)
- Include/exclude branching with bound pruning

**Result:** 27/30 instances solved. All 3 unsolved cases are strongly-correlated PI_3 family.

### 3.3 TSP: GA + 2-opt/3-opt Local Search

The TSP solver combines global exploration with local refinement:

- **Initialization:** Nearest Neighbor (provides feasible baseline)
- **GA:** Tournament selection, Edge Recombination Crossover, inversion mutation
- **Adaptive mechanisms:** Stagnation detection triggers population replacement and increased mutation
- **Local search:** 2-opt after GA, 3-opt for deeper refinement on hard instances

**Result:** 19/25 exact optimums, 6 near-optimal (avg gap 0.32%).

---

## 4. Cross-Problem Analysis

### 4.1 Success Patterns

```
SAT3:    ████████████████████ 100%   (easy: soft constraints)
Knap:    ████████████████████░░  90%   (hard: strongly correlated)
TSP:     ████████████████░░░░░░  76%   (harder: permutation landscape)
```

The success rates inversely correlate with the effective search space size:
- SAT3 has 2^250 ≈ 10^75 states but many solutions and strong heuristic signals
- Knapsack has 2^10000 states but can be pruned aggressively with bounds
- TSP has 10^1000+ states with fewer structural signals for pruning

### 4.2 Runtime Characteristics

| Metric | SAT3 | Knapsack | TSP |
|---|---|---|---|
| Fastest instance | 0.012s | ~0.1s | 15s |
| Median | 0.73s | ~30s | 945s |
| Slowest | 239s | Not converged | 33618s (9.3h) |

TSP is by far the most computationally expensive due to the need to maintain a population and run local search on each candidate tour.

### 4.3 What Makes an Instance Hard?

**SAT3:** Hardness correlates with proximity to phase transition (m/n ≈ 4.2). Our instances at 4.26 include a long tail of hard cases.

**Knapsack:** Strongly correlated instances (value ≈ weight + constant) are hardest because the fractional bound becomes less informative and less pruning occurs.

**TSP:** Real-world instances with clustered cities (like pcb442, a PCB routing instance) are harder than random Euclidean instances because local search is more likely to get trapped in structural local minima.

---

## 5. Conclusion

This study demonstrates that three NP-complete problems from Karp's original list, despite sharing the same worst-case complexity, respond very differently to hybrid algorithmic approaches:

| Problem | Best approach for us | Key success factor | Failure mode |
|---|---|---|---|
| 3-SAT | Stochastic local search | Strong heuristic signal (make/break) | Long-tailed runtime |
| 0-1 KP | Exact B&B (small) / GA (large) | Tight fractional bound | Weakly informative bounds |
| TSP | GA + local search | Effective 2-opt neighborhoods | Premature convergence |

The 94.2% overall success rate across 155 instances suggests that hybrid approaches combining global exploration with problem-specific heuristics provide a practical path for solving NP-hard problems, even in the absence of polynomial-time algorithms.

---

## 6. Recommended Cross-Problem Improvements

1. **Transfer learning:** Use SAT3's adaptive restart heuristic in TSP's stagnation handling
2. **Hybrid bounds:** Apply knapsack-style fractional bounds to TSP subproblems
3. **Noise strategies:** Borrow WalkSAT's noise parameter for GA mutation control
4. **Unified benchmarking:** Run all solvers on a shared hardware platform for fair comparison

---

## References

- Cook, S. A. (1971). The complexity of theorem-proving procedures. *STOC*, 151–158.
- Karp, R. M. (1972). Reducibility among combinatorial problems. *Complexity of Computer Computations*, 85–103.
- Selman, B., Kautz, H., & Cohen, B. (1994). Noise strategies for improving local search. *AAAI*, 337–343.
- Selman, B., Levesque, H., & Mitchell, D. (1992). A new method for solving hard satisfiability problems. *AAAI*, 440–446.
- Pisinger, D. (2005). Where are the hard knapsack problems? *Computers & Operations Research*, 32(9), 2271–2284.
- Reinelt, G. (1991). TSPLIB—A Traveling Salesman Problem Library. *ORSA Journal on Computing*, 3(4), 376–384.
- Hoos, H. H., & Stützle, T. (2004). *Stochastic Local Search: Foundations & Applications*. Morgan Kaufmann.
