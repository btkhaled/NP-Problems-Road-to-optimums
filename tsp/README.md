# Traveling Salesman Problem — Empirical Evaluation

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

## Problem Definition

Given n cities and pairwise distances d(i, j), find the shortest Hamiltonian cycle visiting each city exactly once and returning to the origin.

**Complexity:** NP-hard (Karp's #6). The decision version (does a tour of length ≤ L exist?) is NP-complete.

---

## Solver: Hybrid GA + Local Search

### Genetic Algorithm (`src/solver_ga.py`)
- **Initialization:** Nearest Neighbor heuristic (provides 15–25% improvement baseline)
- **Selection:** Tournament with elitism
- **Crossover:** Edge Recombination Crossover (ERX) — preserves adjacency information
- **Mutation:** Inversion (segment reversal) with adaptive rate
- **Stagnation handling:** Population replacement when no improvement

### Local Search (`src/local_search.py`)
- **2-opt:** Removes 2 edges, reconnects in alternative configuration
- **3-opt:** Removes 3 edges, explores 4 reconnection patterns

---

## Benchmarks: TSPLIB

| Category | Instances | Tested | Exact Optimum | Near-optimal |
|---|---|---|---|---|
| Small (51–100 cities) | ~23 | 8 | 8 (100%) | 0 |
| Medium (100–200) | ~27 | 12 | 9 (75%) | 3 |
| Large (200–575) | ~16 | 5 | 2 (40%) | 3 |
| **Total** | **111** | **25** | **19 (76%)** | **6** |

### Results Detail

| Instance | Cities | Algorithm | Known Opt | Gap |
|---|---|---|---|---|
| eil51 | 51 | 426 | 426 | 0% ✓ |
| berlin52 | 52 | 7542 | 7542 | 0% ✓ |
| st70 | 70 | 675 | 675 | 0% ✓ |
| ... | ... | ... | ... | ... |
| d198 | 198 | 15781 | 15780 | 0.006% |
| pcb442 | 442 | 50997 | 50778 | 0.43% |
| rat575 | 575 | 6815 | 6773 | 0.62% |

Average improvement vs Nearest Neighbor: **19.47%**

---

## Structure

```
tsp/
├── README.md
├── data/tsplib/
│   ├── README.md           # TSPLIB documentation
│   ├── solutions           # Known optimal tour lengths (111 entries)
│   ├── *.tsp               # 111 TSPLIB instances
│   └── xray/               # X-ray crystallography TSP data
│       ├── README.md
│       ├── crystal-data.md
│       ├── DAUX.FOR
│       ├── DEQ.FOR
│       ├── DUNEQ.FOR
│       └── GENTSP.FOR
├── notebooks/
│   └── TSP_KBT.ipynb      # Original notebook
├── results/
│   ├── TSP_KBT_Final_Results.md  # Full paper
│   └── results-summary.md        # Condensed results
└── src/
    ├── solver_ga.py        # GA with ERX
    ├── solver_nn.py        # Nearest Neighbor
    ├── local_search.py     # 2-opt, 3-opt
    └── utils.py            # TSPLIB loading, distance
```

---

## References

- Reinelt, G. (1991). TSPLIB—A Traveling Salesman Problem Library. *ORSA Journal on Computing*, 3(4), 376–384.
- Lin, S., & Kernighan, B. W. (1973). An effective heuristic algorithm for the traveling-salesman problem. *Operations Research*, 21(2), 498–516.
- Johnson, D. S., & McGeoch, L. A. (1997). The traveling salesman problem: A case study in local optimization. In *Local Search in Combinatorial Optimization*.
