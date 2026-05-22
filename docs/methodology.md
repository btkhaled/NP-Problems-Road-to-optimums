# Methodology — Shared Algorithmic Framework

**Khaled Ben Taïeb** · bt.khaled@gmail.com

This document describes the common algorithmic techniques used across the three NP-hard problems in this repository. Each problem employs a hybrid strategy combining exact methods, metaheuristics, and local search.

---

## 1. Exact Methods

### 1.1 Branch-and-Bound (Knapsack)

Branch-and-Bound is an exact algorithm for discrete optimization that systematically explores the search space by branching on decision variables and pruning branches that cannot contain an optimal solution.

**Implementation details** (see `knapsack/src/solver_bnb.py`):
- **Variable ordering:** Items sorted by value-to-weight ratio (descending)
- **Branching:** Include/exclude decisions on each item
- **Upper bound:** Fractional knapsack bound (Dantzig bound) computed via binary search on cumulative arrays
- **Search strategy:** Best-first (max-heap priority queue using negative bounds)
- **Pruning:** A branch is pruned when its upper bound ≤ incumbent value

Pseudo-code:
```
function branch_and_bound(items, capacity):
    sort items by value/weight ratio
    compute cumulative weight/value arrays
    queue ← [(bound(items[0]), [])]
    best_value ← 0
    while queue not empty:
        (bound, selected) ← pop best from queue
        if bound ≤ best_value: continue
        if all items decided:
            update best_value
        else:
            branch included child
            branch excluded child
    return best_value, solution
```

### 1.2 Best-First Search

The priority-queue-based exploration ensures that the most promising branches (highest upper bound) are explored first, leading to early incumbent discovery and more aggressive pruning.

---

## 2. Metaheuristics

### 2.1 Genetic Algorithm (Knapsack, TSP)

Genetic Algorithms are population-based metaheuristics inspired by natural evolution.

**Knapsack GA** (`knapsack/src/solver_ga.py`):
- Representation: bitstring (1 = selected, 0 = not selected)
- Seeding: greedy longest-neighbor heuristic
- Selection: tournament
- Crossover: uniform
- Mutation: bit-flip (rate 0.05)
- Repair: ensures feasibility after crossover/mutation
- Local improvement: inverted_opt4 (insertion, swap, multi-swap, flip, removal)

**TSP GA** (`tsp/src/solver_ga.py`):
- Representation: permutation of cities
- Seeding: Nearest Neighbor heuristic
- Selection: tournament with elitism
- Crossover: Edge Recombination Crossover (ERX)
- Mutation: inversion (segment reversal)
- Adaptive mutation rate: increases during stagnation
- Stagnation handling: partial population replacement

### 2.2 WalkSAT + GSAT (SAT3)

Stochastic local search for Boolean Satisfiability.

**WalkSAT** (`sat/src/solver_walksat.py`):
- Picks an unsatisfied clause
- With probability p: flip a random variable in that clause (random walk)
- With probability 1-p: flip the variable that minimizes the break count (greedy)

**GSAT integration:**
- Evaluates all candidate flips via make/break scoring
- make = number of newly satisfied clauses
- break = number of newly unsatisfied clauses
- Also selects randomly among equally good flips

**Adaptive restarts:**
- When no improvement for N iterations
- Re-initialize with majority-polarity heuristic
- Targeted perturbation on variables in difficult clauses

### 2.3 Incremental Clause-State Evaluation

Instead of re-evaluating all clauses after each flip, only clauses containing the flipped variable are updated. This reduces per-iteration complexity from O(m) to O(degree of variable).

---

## 3. Local Search

### 3.1 2-opt (TSP)

Removes two edges and reconnects them in the alternative way. If the new tour is shorter, it replaces the current tour. Repeated until no improving 2-opt move exists.

```
Before: ...-A-B-...-C-D-...
After:  ...-A-C-...-B-D-...
```

### 3.2 3-opt (TSP)

Generalizes 2-opt by considering three edge removals. Explores 4 possible reconnection patterns. More computationally expensive but can escape local minima that 2-opt cannot.

### 3.3 inverted_opt4 (Knapsack)

A compound local search combining:
- **Insertion:** move item from selected to not-selected or vice versa
- **Single-swap:** exchange one selected item with one not-selected
- **Multi-swap:** exchange multiple items simultaneously
- **Flip:** toggle a single item
- **Removal:** remove items until feasible
