# 3-SAT Problem — Empirical Evaluation

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

## Problem Definition

Given a Boolean formula in Conjunctive Normal Form (CNF) where each clause contains exactly three literals, determine whether there exists a truth assignment to the variables that satisfies all clauses.

**Complexity:** NP-complete (Cook-Levin 1971; Karp's #2). The first problem proven NP-complete.

---

## Solver: Hybrid WalkSAT + GSAT

The solver combines WalkSAT's stochastic exploration with GSAT's greedy scoring:

- **Initialization:** Majority-polarity heuristic
- **Selection:** Random unsatisfied clause → evaluate candidate flips
- **Greedy step:** Flip variable with break = 0 (if exists) or lowest break
- **Random walk:** With noise probability p, flip random variable in clause
- **Incremental evaluation:** Only clauses containing the flipped variable are updated
- **Restart:** Adaptive restart with targeted perturbation when stalled

### Key Innovation

Incremental clause-state tracking reduces per-flip complexity from O(m) to O(degree(x)), enabling millions of flips per second.

---

## Benchmarks: SATLIB UF250

| Parameter | Value |
|---|---|
| Instances | 100 (uf250-01.cnf to uf250-100.cnf) |
| Variables | 250 per instance |
| Clauses | 1065 per instance |
| Ratio | 4.26 (near phase transition ~4.2) |
| All satisfiable | Yes (SATLIB guarantees) |

### Runtime Distribution

| Range | Count |
|---|---|
| < 1 sec | 61 |
| 1–10 sec | 26 |
| 10–30 sec | 9 |
| 30–100 sec | 3 |
| > 100 sec | 1 |

**Fastest:** uf250-061 (0.012s) | **Slowest:** uf250-054 (239.931s) | **Median:** 0.731s

---

## Structure

```
sat/
├── README.md
├── data/UF250.1065.100/
│   ├── README.md            # SATLIB UF250 documentation
│   └── uf250-*.cnf         # 100 DIMACS CNF files
├── notebooks/
│   └── SAT3_KBT.ipynb      # Original notebook with solver + results
├── results/
│   ├── SAT3_Final_Results.md  # Full paper (renamed from Results.md)
│   └── results-summary.md     # Condensed results
└── src/
    ├── solver_walksat.py    # WalkSAT + GSAT solver
    ├── clause_state.py      # Incremental clause tracking
    └── utils.py             # CNF parsing, verification
```

---

## References

- Cook, S. A. (1971). The complexity of theorem-proving procedures. *STOC*, 151–158.
- Selman, B., Kautz, H., & Cohen, B. (1994). Noise strategies for improving local search. *AAAI*, 337–343.
- Selman, B., Levesque, H., & Mitchell, D. (1992). A new method for solving hard satisfiability problems. *AAAI*, 440–446.
- Mitchell, D., Selman, B., & Levesque, H. (1992). Hard and easy distributions of SAT problems. *AAAI*, 459–465.
- Hoos, H. H., & Stützle, T. (2004). *Stochastic Local Search: Foundations & Applications*. Morgan Kaufmann.
