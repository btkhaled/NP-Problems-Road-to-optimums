# Hybrid Genetic and Branch-and-Bound Framework for Solving 0-1 Knapsack Instances: Empirical Evaluation on Pisinger Benchmarks

**Khaled Ben Taïeb**  
bt.khaled@gmail.com

---

# Abstract

The 0-1 Knapsack Problem is one of the most classical NP-hard combinatorial optimization problems. Given a finite set of items, each associated with a value and a weight, the objective is to select a subset of items that maximizes total value while respecting a fixed capacity constraint.

This study presents an empirical notebook-based evaluation of multiple knapsack-solving strategies applied to low-dimensional and large-scale Pisinger benchmark instances. The tested framework combines a genetic algorithm for heuristic exploration on small instances with a turbo-boosted exact Branch-and-Bound solver for systematic optimization.

The Branch-and-Bound solver uses value-to-weight ratio ordering, fractional upper bounds, cumulative weight/value arrays, binary-search bound evaluation, best-first heap exploration, and include/exclude branching to prune the combinatorial search space efficiently.

Experimental evaluation was conducted on **30 benchmark runs** stored in the notebook outputs: **10 low-dimensional Pisinger instances** and **20 large-scale Pisinger instances**. The solver produced complete optimal-output blocks for **27/30 instances**, corresponding to a notebook-level optimization completion rate of **90.00%**.

The saved notebook outputs show that all low-dimensional instances were optimized successfully, while three large-scale `knapPI_3` instances were not finalized in the stored execution output. Two of these cells stop after loading and printing the item table without an optimal-solution block, and one cell terminates with a `KeyboardInterrupt`.

**Keywords:** 0-1 Knapsack, Pisinger Instances, Branch-and-Bound, Genetic Algorithm, Fractional Bound, Best-First Search, Combinatorial Optimization, NP-Hard Problems.

---

# 1. Introduction

The 0-1 Knapsack Problem is a fundamental optimization problem in computer science, operations research, logistics, resource allocation, portfolio selection, scheduling, and computational complexity theory.

A knapsack instance consists of:

- a set of items,
- a value associated with each item,
- a weight associated with each item,
- a maximum capacity constraint.

The objective is to maximize the total value of selected items without exceeding the capacity limit.

This notebook explores practical optimization strategies for solving Pisinger benchmark instances. The study includes both low-dimensional instances, where full validation is straightforward, and large-scale instances with up to 10,000 items, where search-space pruning becomes essential.

The central objective is to determine which instances were fully optimized in the saved notebook outputs and which instances remained unfinished or interrupted.

---

# 2. Methodology

The notebook contains three major computational components:

- A Genetic Algorithm used as a heuristic solver on a small low-dimensional instance.
- A turbo-boosted exact Branch-and-Bound solver used across the benchmark suite.
- Verification scripts used to recompute selected-item value and weight for low-dimensional instances.

---

## 2.1 Genetic Algorithm

The Genetic Algorithm was applied to `f10_l-d_kp_20_879`.

The GA implementation includes:

- Greedy seeding based on a longest-neighbor construction heuristic
- Bitstring representation of candidate solutions
- Tournament selection
- Uniform crossover
- Bit-flip mutation
- Repair and local-improvement mechanisms
- Fitness evaluation based on feasible total value

The GA converged to the same objective value later produced by the exact Branch-and-Bound solver.

| Instance | Algorithm | Best Value | Total Weight | Capacity | Consistency |
|---|---|---|---|---|---|
| f10_l-d_kp_20_879 | Genetic Algorithm | 1025 | 871 | 879 | Matches Branch-and-Bound output |

---

## 2.2 Turbo-Boosted Branch-and-Bound Solver

The primary solver is a best-first Branch-and-Bound algorithm.

The implementation includes:

- Sorting items by decreasing value-to-weight ratio
- Precomputed cumulative weight arrays
- Precomputed cumulative value arrays
- Binary-search fractional-bound computation
- Max-heap style priority queue using negative bounds
- Include/exclude branching
- Bound-based pruning
- Reconstruction of the final binary solution in original item order

This design allows the solver to explore promising branches first while pruning branches whose upper bound cannot exceed the current best value.

---

## 2.3 Verification Procedure

For low-dimensional instances, separate verification cells recompute:

- total value of selected items,
- total weight of selected items,
- capacity feasibility,
- consistency between reported value and recomputed value.

All low-dimensional verification cells returned `valid: True`.

| Instance | Items | Capacity | Reported Value | Computed Weight | Valid? |
|---|---|---|---|---|---|
| f10_l-d_kp_20_879 | 20 | 879 | 1025 | 871 | Yes |
| f1_l-d_kp_10_269 | 10 | 269 | 295 | 269 | Yes |
| f2_l-d_kp_20_878 | 20 | 878 | 1024 | 871 | Yes |
| f3_l-d_kp_4_20 | 4 | 20 | 35 | 18 | Yes |
| f4_l-d_kp_4_11 | 4 | 11 | 23 | 11 | Yes |
| f5_l-d_kp_15_375 | 15 | 375 | 481.069368 | 354.960784 | Yes |
| f6_l-d_kp_10_60 | 10 | 60 | 52 | 57 | Yes |
| f7_l-d_kp_7_50 | 7 | 50 | 107 | 50 | Yes |
| f8_l-d_kp_23_10000 | 23 | 10000 | 9767 | 9768 | Yes |
| f9_l-d_kp_5_80 | 5 | 80 | 130 | 60 | Yes |

---

# 3. Experimental Setup

The notebook evaluates instances from the Pisinger 0-1 Knapsack benchmark collection.

The evaluated subset contains:

- **10 low-dimensional instances**
- **20 large-scale instances**
- **30 Branch-and-Bound benchmark runs in total**
- Item counts ranging from **4** to **10,000**
- Capacities ranging from **11** to **49,877**

The evaluation criterion used in this report is strict:

- **Optimized** means the saved notebook output contains a completed `Optimal Solution` / `FINAL OPTIMAL SOLUTION SUMMARY` block with a solver value and feasible total weight.
- **Not finalized** means the cell loaded the instance but did not print a final optimal-solution block.
- **Interrupted** means the notebook output explicitly contains a `KeyboardInterrupt`.

---

# 4. Results

The notebook outputs show that **27 out of 30 Branch-and-Bound runs were finalized successfully**.

---

## 4.1 Statistical Performance Summary

| Metric | Value |
|---|---:|
| Total Branch-and-Bound Runs in Notebook | 30 |
| Optimized / Finalized Runs | 27 |
| Not Optimized / Unfinished Runs | 3 |
| Optimization Completion Rate | 90.00% |
| Low-Dimensional Instances Optimized | 10/10 |
| Large-Scale Instances Optimized | 17/20 |
| Explicit KeyboardInterrupt Runs | 1 |
| Not-Finalized Output Runs | 2 |
| Largest Optimized Instance Size | 10,000 items |
| Highest Optimized Objective Value | 563647 on `knapPI_1_10000_1000_1` |
| Lowest Optimized Objective Value | 23 on `f4_l-d_kp_4_11` |
| Average Selected Items in Optimized Runs | 107.52 |

---

## 4.2 Optimization Status Summary

| Category | Optimized | Not Optimized | Total | Completion Rate |
|---|---:|---:|---:|---:|
| Low-dimensional Pisinger | 10 | 0 | 10 | 100.00% |
| Large-scale Pisinger | 17 | 3 | 20 | 85.00% |
| Overall | 27 | 3 | 30 | 90.00% |

---

## 4.3 Low-Dimensional Benchmark Results

All low-dimensional benchmark instances were optimized successfully.

| Instance | Items | Capacity | Optimal Value | Total Weight | Selected Items | Optimized? |
|---|---|---|---|---|---|---|
| f10_l-d_kp_20_879 | 20 | 879 | 1025 | 871 | 17 | Yes |
| f1_l-d_kp_10_269 | 10 | 269 | 295 | 269 | 6 | Yes |
| f2_l-d_kp_20_878 | 20 | 878 | 1024 | 871 | 17 | Yes |
| f3_l-d_kp_4_20 | 4 | 20 | 35 | 18 | 3 | Yes |
| f4_l-d_kp_4_11 | 4 | 11 | 23 | 11 | 2 | Yes |
| f5_l-d_kp_15_375 | 15 | 375 | 481.069368 | 354.960784 | 9 | Yes |
| f6_l-d_kp_10_60 | 10 | 60 | 52 | 57 | 7 | Yes |
| f7_l-d_kp_7_50 | 7 | 50 | 107 | 50 | 2 | Yes |
| f8_l-d_kp_23_10000 | 23 | 10000 | 9767 | 9768 | 11 | Yes |
| f9_l-d_kp_5_80 | 5 | 80 | 130 | 60 | 4 | Yes |

---

## 4.4 Large-Scale Benchmark Results

The solver finalized **17/20** large-scale runs. The unresolved cases are concentrated in the `knapPI_3` family.

| Instance | Items | Capacity | Solver Value | Total Weight | Selected Items | Optimized? | Notebook Evidence |
|---|---|---|---|---|---|---|---|
| knapPI_1_10000_1000_1 | 10000 | 49877 | 563647 | 49877 | 840 | Yes | Final optimal solution printed |
| knapPI_1_1000_1000_1 | 1000 | 5002 | 54503 | 5002 | 83 | Yes | Final optimal solution printed |
| knapPI_1_100_1000_1 | 100 | 995 | 9147 | 985 | 12 | Yes | Final optimal solution printed |
| knapPI_1_2000_1000_1 | 2000 | 10011 | 110625 | 10011 | 160 | Yes | Final optimal solution printed |
| knapPI_1_5000_1000_1 | 5000 | 25016 | 276457 | 25016 | 410 | Yes | Final optimal solution printed |
| knapPI_1_500_1000_1 | 500 | 2543 | 28857 | 2543 | 42 | Yes | Final optimal solution printed |
| knapPI_2_10000_1000_1 | 10000 | 49877 | 90204 | 49877 | 603 | Yes | Final optimal solution printed |
| knapPI_2_1000_1000_1 | 1000 | 5002 | 9052 | 5002 | 59 | Yes | Final optimal solution printed |
| knapPI_2_100_1000_1 | 100 | 995 | 1514 | 991 | 9 | Yes | Final optimal solution printed |
| knapPI_2_2000_1000_1 | 2000 | 10011 | 18051 | 10010 | 115 | Yes | Final optimal solution printed |
| knapPI_2_200_1000_1 | 200 | 1008 | 1634 | 1006 | 9 | Yes | Final optimal solution printed |
| knapPI_2_5000_1000_1 | 5000 | 25016 | 44356 | 25016 | 284 | Yes | Final optimal solution printed |
| knapPI_2_500_1000_1 | 500 | 2543 | 4566 | 2543 | 28 | Yes | Final optimal solution printed |
| knapPI_3_10000_1000_1 | 10000 | 49519 | — | — | — | No | Output stops before optimal solution block |
| knapPI_3_1000_1000_1 | 1000 | 4990 | 14390 | 4990 | 94 | Yes | Final optimal solution printed |
| knapPI_3_100_1000_1 | 100 | 997 | 2397 | 997 | 14 | Yes | Final optimal solution printed |
| knapPI_3_2000_1000_1 | 2000 | 9819 | — | — | — | No | Output stops before optimal solution block |
| knapPI_3_200_1000_1 | 200 | 997 | 2697 | 997 | 17 | Yes | Final optimal solution printed |
| knapPI_3_5000_1000_1 | 5000 | 24805 | — | — | — | No | Execution interrupted by KeyboardInterrupt |
| knapPI_3_500_1000_1 | 500 | 2517 | 7117 | 2517 | 46 | Yes | Final optimal solution printed |

---

## 4.5 Instances Not Optimized in the Saved Notebook Output

The saved notebook output contains **three** unresolved large-scale cases.

| Instance | Items | Capacity | Status | Reason |
|---|---|---|---|---|
| knapPI_3_10000_1000_1 | 10000 | 49519 | Not finalized | Output stops before optimal solution block |
| knapPI_3_2000_1000_1 | 2000 | 9819 | Not finalized | Output stops before optimal solution block |
| knapPI_3_5000_1000_1 | 5000 | 24805 | Interrupted | Execution interrupted by KeyboardInterrupt |

Important interpretation:

- `knapPI_3_10000_1000_1` is not counted as optimized because the output stops after loading the instance and printing the item preview.
- `knapPI_3_2000_1000_1` is not counted as optimized for the same reason.
- `knapPI_3_5000_1000_1` is not counted as optimized because execution was explicitly interrupted.

Therefore, based strictly on the uploaded notebook outputs, the number of non-optimized instances is **3**, not 2.

---

# 5. Discussion

The results show strong empirical performance across both low-dimensional and large-scale Pisinger benchmark instances.

The low-dimensional benchmark group was completely solved. In addition, every low-dimensional verification cell recomputed the reported solution value and weight successfully, confirming internal consistency of the selected binary vectors.

The large-scale benchmark group was also mostly solved, with successful optimization on all stored `knapPI_1` and `knapPI_2` runs and on several `knapPI_3` runs.

The unresolved cases are concentrated in the `knapPI_3` family, which appears to be the hardest family for the implemented Branch-and-Bound configuration in this notebook. This is consistent with the nature of strongly correlated knapsack instances, where value-weight structure can make pruning less aggressive and increase the number of branches that must be explored.

The Branch-and-Bound implementation is exact when it terminates, because it explores include/exclude decisions and prunes only branches whose fractional upper bound cannot beat the incumbent solution. However, the notebook does not provide wall-clock runtimes or node-exploration statistics, which limits runtime analysis.

The three unresolved cases should therefore be interpreted as **not finalized in the saved notebook execution**, rather than as mathematical failures of the algorithm.

---

# 6. Conclusion

This notebook presents a hybrid empirical framework for solving 0-1 Knapsack benchmark instances using genetic search, exact Branch-and-Bound optimization, and validation scripts.

The Genetic Algorithm successfully solved the small `f10_l-d_kp_20_879` instance and matched the Branch-and-Bound result.

The turbo-boosted Branch-and-Bound solver finalized **27/30** benchmark runs, including all low-dimensional instances and most large-scale instances.

The final notebook-level result is:

- **27 optimized instances**
- **3 non-optimized / unfinished instances**
- **90.00% optimization completion rate**

The three non-optimized instances are:

1. `knapPI_3_10000_1000_1`
2. `knapPI_3_2000_1000_1`
3. `knapPI_3_5000_1000_1`

These instances should be prioritized for the next optimization pass, ideally using improved pruning, incumbent seeding, runtime logging, and checkpointing to avoid losing progress on long-running Branch-and-Bound executions.

---

# 7. Recommended Next Steps

The next version of the solver should add:

- Wall-clock runtime logging
- Node expansion counts
- Priority-queue size tracking
- Incumbent update logs
- Checkpoint saving and resume support
- Better initial incumbent generation using greedy + local search
- Dominance preprocessing
- Surrogate relaxation or core-problem reduction
- Memory-safe bounded priority queues for very large instances

These additions would make the final report stronger and would allow precise runtime and scalability analysis.

---

# 8. Final Optimization Status

| Final Status | Count |
|---|---:|
| Optimized | 27 |
| Not Optimized | 3 |
| Total | 30 |

**Final conclusion:** the uploaded notebook demonstrates successful optimization of **27/30** benchmark instances, with **3 unresolved `knapPI_3` large-scale cases** remaining.
