# 0-1 Knapsack Problem — Empirical Evaluation

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

## Problem Definition

Given a set of n items, each with value v_i and weight w_i, and a capacity C, select a subset S ⊆ {1, ..., n} that maximizes ∑_{i∈S} v_i subject to ∑_{i∈S} w_i ≤ C.

**Complexity:** NP-hard (Karp's #8). Pseudo-polynomial via dynamic programming O(nC).

---

## Solver

### Genetic Algorithm (`src/solver_ga.py`)
- Bitstring representation, greedy seeding
- Tournament selection, uniform crossover, bit-flip mutation
- inverted_opt4 local improvement
- 50 population, 100 generations

### Branch-and-Bound (`src/solver_bnb.py`)
- Best-first search with max-heap priority queue
- Items sorted by value-to-weight ratio
- Fractional Dantzig bound via binary search
- Include/exclude branching with bound pruning

---

## Benchmarks: Pisinger Instances

| Family | Type | Count | Tested | Solved |
|---|---|---|---|---|
| Low-dimensional | Mixed | 10 | 10 | 10 (100%) |
| PI_1 | Uncorrelated | 7 | 6 | 6 (100%) |
| PI_2 | Weakly correlated | 7 | 7 | 7 (100%) |
| PI_3 | Strongly correlated | 7 | 7 | 4 (57%) |
| **Total** | | **31** | **30** | **27 (90%)** |

The 3 unsolved instances are all in the strongly-correlated PI_3 family:
- knapPI_3_2000_1000_1 (2000 items)
- knapPI_3_5000_1000_1 (interrupted)
- knapPI_3_10000_1000_1 (10000 items)

---

## Structure

```
knapsack/
├── README.md
├── data/pisinger/
│   ├── README.md              # Benchmark documentation
│   ├── low-dimensional/       # 10 instances
│   ├── low-dimensional-optimum/  # 10 optimum values
│   ├── large_scale/           # 21 instances
│   └── large_scale-optimum/   # 21 optimum values
├── notebooks/
│   └── KNAPSACK_KBT.ipynb     # Original notebook
├── results/
│   ├── Knapsack_KBT_Final_Results.md  # Full paper
│   └── results-summary.md            # Condensed results
└── src/
    ├── solver_bnb.py          # Branch-and-Bound solver
    ├── solver_ga.py           # Genetic Algorithm
    └── utils.py               # Instance loading, verification
```

---

## Results

| Best result | Worst result |
|---|---|
| knapPI_1_10000: 563,647 (10K items) | knapPI_3_10000: not converged |
| f10_l-d_kp_20_879: 1025, verified | avg runtime on PI_3: very high |
| All low-dimensional: 100% | |

---

## References

- Pisinger, D. (2005). Where are the hard knapsack problems? *Computers & Operations Research*, 32(9), 2271–2284.
- Martello, S., & Toth, P. (1990). *Knapsack Problems: Algorithms and Computer Implementations*. Wiley.
- Kellerer, H., Pferschy, U., & Pisinger, D. (2004). *Knapsack Problems*. Springer.
