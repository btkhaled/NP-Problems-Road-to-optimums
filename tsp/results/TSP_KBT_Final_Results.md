# Hybrid Genetic and Local-Search Algorithms for Solving the Traveling Salesman Problem: Empirical Evaluation on TSPLIB Benchmark Instances

**Khaled Ben Taïeb**  
bt.khaled@gmail.com

---

# Abstract

The Traveling Salesman Problem (TSP) is one of the most important NP-hard combinatorial optimization problems, with applications in logistics, transportation, manufacturing, route planning, and network design.

This notebook presents a hybrid heuristic and metaheuristic framework for solving standard TSPLIB benchmark instances. The implemented approach combines Nearest Neighbor initialization, Genetic Algorithms, Edge Recombination Crossover, inversion mutation, adaptive mutation control, stagnation-aware population replacement, and local search procedures based on 2-opt and 3-opt refinements.

A full audit of the notebook code and saved execution outputs was conducted across **25 TSPLIB instances**, ranging from **51** to **575** cities. The algorithm reached the known optimum on **19/25 instances**, corresponding to an exact-optimum success rate of **76.00%**. The remaining **6/25 instances** were near-optimal, with small relative gaps.

The strongest result is that the notebook demonstrates exact optimality on a broad range of classical TSP instances, including `Berlin52`, `KroA100`, `KroB100`, `Pr124`, `Bier127`, `KroA200`, `KroB200`, and `Lin318`.

**Keywords:** Traveling Salesman Problem, TSPLIB, Genetic Algorithm, Nearest Neighbor, Edge Recombination Crossover, 2-opt, 3-opt, Local Search, Metaheuristics, Combinatorial Optimization.

---

# 1. Introduction

The Traveling Salesman Problem asks for the shortest possible closed tour that visits each city exactly once and returns to the starting city.

Despite its simple formulation, TSP is computationally difficult and remains a benchmark problem for evaluating heuristic, metaheuristic, exact, and hybrid optimization algorithms.

This notebook evaluates a custom hybrid framework on standard TSPLIB benchmark instances. The goal is not only to produce good tours, but also to compare the algorithmic tour length against known optimal values and determine which instances were solved exactly.

The notebook contains:

- executable solver cells for each benchmark instance,
- generated route outputs,
- detailed edge-by-edge route distance prints,
- runtime outputs,
- solution verification cells,
- city-count validation cells,
- and an initial manually written performance summary.

This final report is based on a fresh audit of the notebook code and saved outputs.

---

# 2. Methodology

The solver combines constructive heuristics, genetic search, adaptive diversification, and local-search refinement.

---

## 2.1 TSP Loading and Distance Matrix Construction

Each TSPLIB instance is loaded using `tsplib95`.

For every instance, the notebook builds a full distance matrix using the TSPLIB edge-weight function. This allows fast repeated evaluation of candidate tours during genetic search and local optimization.

The core loading and preprocessing pipeline is:

- load the `.tsp` file,
- extract the ordered list of nodes,
- compute pairwise distances,
- build a distance matrix,
- evaluate candidate tours using matrix lookup.

---

## 2.2 Nearest Neighbor Initialization

The solver first constructs an initial route using the Nearest Neighbor heuristic.

This provides a feasible baseline tour and helps seed the Genetic Algorithm with a structured solution rather than relying only on random initialization.

Across the notebook, the final hybrid solver substantially improves the initial Nearest Neighbor tour.

The average improvement from the initial Nearest Neighbor tour to the final route is **19.47%**.

---

## 2.3 Genetic Algorithm

The Genetic Algorithm is the central exploration mechanism of the notebook.

The implemented GA includes:

- population-based search,
- tournament selection,
- elitism,
- Edge Recombination Crossover,
- inversion mutation,
- adaptive mutation-rate control,
- stagnation detection,
- population replacement during stagnation,
- and repeated local-search refinement.

The Genetic Algorithm searches globally across the tour space while local search improves candidate tours at the neighborhood level.

---

## 2.4 Edge Recombination Crossover

The notebook uses Edge Recombination Crossover as the main crossover operator.

This operator is particularly suitable for TSP because it attempts to preserve adjacency information from parent tours. Since high-quality TSP solutions depend strongly on good local edge structures, preserving useful edges can improve convergence toward short tours.

---

## 2.5 Inversion Mutation

The mutation operator is based on inversion.

Instead of randomly swapping individual cities, inversion mutation reverses a segment of the route. This is a natural mutation operator for TSP because it can meaningfully reorganize route geometry while preserving a valid permutation.

---

## 2.6 Local Search: 2-opt and 3-opt

The notebook implements local-search refinements using 2-opt and 3-opt logic.

The 2-opt step removes two edges and reconnects the tour in a way that can reduce total distance.

The 3-opt logic explores stronger neighborhood moves by considering three breakpoints and segment reversals.

After the Genetic Algorithm, the best tour is refined again using 2-opt before final reporting.

---

## 2.7 Stagnation Handling

The algorithm tracks stagnation across generations.

When the population stops improving, part of the population is replaced. This helps the search escape local minima and improves diversity.

This mechanism is visible in the outputs through repeated messages such as:

```text
Stagnation detected. Replacing part of the population.
```

---

# 3. Experimental Setup

The notebook evaluates **25 TSPLIB benchmark instances**.

The benchmark suite includes small, medium, and larger instances:

- `Eil51`
- `Berlin52`
- `St70`
- `Eil76`
- `Pr76`
- `Rat99`
- `KroA100`
- `KroB100`
- `Eil101`
- `Lin105`
- `Pr107`
- `Pr124`
- `Bier127`
- `Ch130`
- `KroA150`
- `KroB150`
- `U159`
- `Rat195`
- `D198`
- `KroA200`
- `KroB200`
- `Lin318`
- `Fl417`
- `Pcb442`
- `Rat575`

The evaluation criteria are:

- known TSPLIB optimum,
- algorithm-generated tour length,
- absolute optimality gap,
- relative optimality gap,
- runtime,
- route validity,
- number of unique cities visited,
- and whether the known optimum was achieved.

---

# 4. Code-Level Configuration Audit

The notebook does not use one single static configuration for all instances. Several parameter configurations appear across the solver cells.

The main configurable elements are:

- population size,
- number of generations,
- stagnation threshold,
- tournament size,
- local-search refinement intensity.

| Parameter | Role |
|---|---|
| Population size | Controls search diversity and computational cost |
| Generations | Controls the number of evolutionary search iterations |
| Max stagnation | Controls when diversification is triggered |
| Tournament size | Controls selection pressure |
| 2-opt / 3-opt | Improves local tour quality |
| Adaptive mutation | Increases exploration during stagnation |

---

## 4.1 Per-Instance Configuration Summary

| Instance   |   Population |   Configured Generations |   Observed Max Generation |   Max Stagnation | Tournament Size   |
|:-----------|-------------:|-------------------------:|--------------------------:|-----------------:|:------------------|
| Eil51      |          300 |                      100 |                       100 |               15 | —                 |
| Berlin52   |          300 |                      100 |                       100 |               15 | —                 |
| St70       |          300 |                      100 |                       100 |               15 | —                 |
| Eil76      |          300 |                      100 |                       100 |               15 | —                 |
| Pr76       |          300 |                      100 |                       100 |               15 | —                 |
| Rat99      |          300 |                      100 |                       100 |               15 | —                 |
| KroA100    |          300 |                      100 |                       100 |               15 | —                 |
| KroB100    |          300 |                      100 |                       100 |               15 | —                 |
| Eil101     |          400 |                      100 |                       100 |               15 | —                 |
| Lin105     |          400 |                      100 |                       100 |               15 | —                 |
| Pr107      |          400 |                      100 |                       100 |               15 | —                 |
| Pr124      |          400 |                      100 |                       100 |               15 | —                 |
| Bier127    |          400 |                      100 |                       100 |               15 | —                 |
| Ch130      |          500 |                      500 |                       500 |               10 | 50                |
| KroA150    |         1000 |                      200 |                       200 |               20 | 12                |
| KroB150    |        10000 |                       10 |                        10 |               10 | 50                |
| U159       |         1000 |                      200 |                        10 |                5 | 12                |
| Rat195     |          800 |                      200 |                       200 |               10 | 8                 |
| D198       |          800 |                      200 |                       200 |               10 | 16                |
| KroA200    |          800 |                      200 |                       200 |               10 | 16                |
| KroB200    |          100 |                      200 |                       200 |               10 | 16                |
| Lin318     |         1000 |                      200 |                       200 |               10 | 16                |
| Fl417      |          800 |                      200 |                       200 |               10 | 22                |
| Pcb442     |          800 |                      200 |                       200 |               10 | 22                |
| Rat575     |          900 |                      100 |                       100 |               10 | 30                |

---

# 5. Results

The notebook achieved exact optimality on **19/25** TSPLIB instances.

---

## 5.1 Statistical Performance Summary

| Metric | Value |
|---|---:|
| Total TSPLIB Instances | 25 |
| Exact Optimum Achieved | 19 |
| Non-Optimal / Near-Optimal | 6 |
| Exact Success Rate | 76.00% |
| Average Runtime | 2508.76 sec |
| Median Runtime | 944.81 sec |
| Total Runtime | 62719.08 sec |
| Fastest Instance | U159 (15.17 sec) |
| Slowest Instance | Ch130 (33617.67 sec) |
| Average Relative Gap Across All Instances | 0.0775% |
| Average Relative Gap Among Non-Optimal Instances | 0.3227% |
| Largest Absolute Gap | Pcb442 (+219) |
| Average Improvement vs Nearest Neighbor | 19.47% |
| Closed Routes in Main Outputs | 25/25 |
| Verification Cells Executed | 24/25 |
| Verification Cells Valid | 24/24 |

---

## 5.2 Runtime Distribution

| Runtime Category   |   Instances |
|:-------------------|------------:|
| Under 1 minute     |           2 |
| Under 5 minutes    |           9 |
| Under 30 minutes   |          19 |
| Over 1 hour        |           3 |

**Important audit note:** the initial manually written summary inside the notebook reports `Ch130` with a runtime of `5077.46` seconds, but the saved execution output of the `Ch130` code cell reports `33617.67` seconds. This final report uses the saved execution output because it is the direct result printed by the executed code cell.

---

## 5.3 Complete Benchmark Results

|   # | Instance   |   Cities |   Known Optimum |   Initial NN |   Algorithm Value |   Gap | Gap (%)   | NN Improvement   |   Runtime (sec) | Exact Optimum?   |
|----:|:-----------|---------:|----------------:|-------------:|------------------:|------:|:----------|:-----------------|----------------:|:-----------------|
|   1 | Eil51      |       51 |             426 |          511 |               426 |     0 | 0.0000%   | 16.63%           |          120.16 | Yes              |
|   2 | Berlin52   |       52 |            7542 |         8980 |              7542 |     0 | 0.0000%   | 16.01%           |          130.24 | Yes              |
|   3 | St70       |       70 |             675 |          830 |               675 |     0 | 0.0000%   | 18.67%           |          357.33 | Yes              |
|   4 | Eil76      |       76 |             538 |          642 |               538 |     0 | 0.0000%   | 16.20%           |          487.37 | Yes              |
|   5 | Pr76       |       76 |          108159 |       153462 |            108159 |     0 | 0.0000%   | 29.52%           |          489.89 | Yes              |
|   6 | Rat99      |       99 |            1211 |         1554 |              1211 |     0 | 0.0000%   | 22.07%           |         1291.74 | Yes              |
|   7 | KroA100    |      100 |           21282 |        27807 |             21282 |     0 | 0.0000%   | 23.47%           |         1416.9  | Yes              |
|   8 | KroB100    |      100 |           22141 |        29158 |             22141 |     0 | 0.0000%   | 24.07%           |         1273.39 | Yes              |
|   9 | Eil101     |      101 |             629 |          803 |               629 |     0 | 0.0000%   | 21.67%           |         1755.4  | Yes              |
|  10 | Lin105     |      105 |           14379 |        20356 |             14379 |     0 | 0.0000%   | 29.36%           |         2309.69 | Yes              |
|  11 | Pr107      |      107 |           44303 |        46680 |             44303 |     0 | 0.0000%   | 5.09%            |         2694.75 | Yes              |
|  12 | Pr124      |      124 |           59030 |        69297 |             59030 |     0 | 0.0000%   | 14.82%           |         4543.3  | Yes              |
|  13 | Bier127    |      127 |          118282 |       135737 |            118282 |     0 | 0.0000%   | 12.86%           |         5102.24 | Yes              |
|  14 | Ch130      |      130 |            6110 |         7579 |              6128 |    18 | 0.2946%   | 19.15%           |        33617.7  | No               |
|  15 | KroA150    |      150 |           26524 |        33633 |             26524 |     0 | 0.0000%   | 21.14%           |          213.54 | Yes              |
|  16 | KroB150    |      150 |           26130 |        34499 |             26130 |     0 | 0.0000%   | 24.26%           |          265.54 | Yes              |
|  17 | U159       |      159 |           42080 |        54675 |             42080 |     0 | 0.0000%   | 23.04%           |           15.17 | Yes              |
|  18 | Rat195     |      195 |            2323 |         2752 |              2335 |    12 | 0.5166%   | 15.15%           |          273.39 | No               |
|  19 | D198       |      198 |           15780 |        18240 |             15781 |     1 | 0.0063%   | 13.48%           |          274.92 | No               |
|  20 | KroA200    |      200 |           29368 |        35859 |             29368 |     0 | 0.0000%   | 18.10%           |          279.69 | Yes              |
|  21 | KroB200    |      200 |           29437 |        36980 |             29437 |     0 | 0.0000%   | 20.40%           |           35.67 | Yes              |
|  22 | Lin318     |      318 |           42029 |        54019 |             42029 |     0 | 0.0000%   | 22.20%           |          944.81 | Yes              |
|  23 | Fl417      |      417 |           11861 |        15013 |             11869 |     8 | 0.0674%   | 20.94%           |         1308.84 | No               |
|  24 | Pcb442     |      442 |           50778 |        61979 |             50997 |   219 | 0.4313%   | 17.72%           |         1478.06 | No               |
|  25 | Rat575     |      575 |            6773 |         8605 |              6815 |    42 | 0.6201%   | 20.80%           |         2039.38 | No               |

---

# 6. Optimized Instances

The following **19 instances** reached the known TSPLIB optimum exactly:

- `Eil51`
- `Berlin52`
- `St70`
- `Eil76`
- `Pr76`
- `Rat99`
- `KroA100`
- `KroB100`
- `Eil101`
- `Lin105`
- `Pr107`
- `Pr124`
- `Bier127`
- `KroA150`
- `KroB150`
- `U159`
- `KroA200`
- `KroB200`
- `Lin318`

---

# 7. Non-Optimized / Near-Optimal Instances

The following **6 instances** did not reach the known optimum in the saved notebook outputs:

| Instance   |   Known Optimum |   Algorithm Value |   Absolute Gap | Relative Gap   |   Runtime (sec) | Status                  |
|:-----------|----------------:|------------------:|---------------:|:---------------|----------------:|:------------------------|
| Ch130      |            6110 |              6128 |             18 | 0.2946%        |        33617.7  | Near-optimal, not exact |
| Rat195     |            2323 |              2335 |             12 | 0.5166%        |          273.39 | Near-optimal, not exact |
| D198       |           15780 |             15781 |              1 | 0.0063%        |          274.92 | Near-optimal, not exact |
| Fl417      |           11861 |             11869 |              8 | 0.0674%        |         1308.84 | Near-optimal, not exact |
| Pcb442     |           50778 |             50997 |            219 | 0.4313%        |         1478.06 | Near-optimal, not exact |
| Rat575     |            6773 |              6815 |             42 | 0.6201%        |         2039.38 | Near-optimal, not exact |

---

## 7.1 Interpretation of Non-Optimal Cases

The non-optimal results are still close to the known optima.

The hardest gap in absolute distance is `Pcb442`, with a gap of **219 units**. However, in relative terms, the gap remains small.

The largest relative gap is `Rat575`, with a gap of approximately **0.6201%**.

This indicates that the algorithm is not failing catastrophically on the unsolved instances. Instead, it is producing strong near-optimal tours that are very close to benchmark optima.

---

# 8. Verification Audit

The notebook includes verification cells for most instances.

These verification cells recompute the tour length independently and confirm route validity. In addition, route city-count checks confirm that each route visits the expected number of cities.

`Rat575` has a valid closed route in the main output, but its separate verification cells were not executed in the saved notebook state.

| Instance   |   Final Distance | Verification Distance   | Verification Cell   | Unique Cities from Verification Cell   | Route Closed from Main Output   |   Unique Cities from Route |
|:-----------|-----------------:|:------------------------|:--------------------|:---------------------------------------|:--------------------------------|---------------------------:|
| Eil51      |              426 | 426                     | Yes                 | 51                                     | Yes                             |                         51 |
| Berlin52   |             7542 | 7542                    | Yes                 | 52                                     | Yes                             |                         52 |
| St70       |              675 | 675                     | Yes                 | 70                                     | Yes                             |                         70 |
| Eil76      |              538 | 538                     | Yes                 | 76                                     | Yes                             |                         76 |
| Pr76       |           108159 | 108159                  | Yes                 | 76                                     | Yes                             |                         76 |
| Rat99      |             1211 | 1211                    | Yes                 | 99                                     | Yes                             |                         99 |
| KroA100    |            21282 | 21282                   | Yes                 | 100                                    | Yes                             |                        100 |
| KroB100    |            22141 | 22141                   | Yes                 | 100                                    | Yes                             |                        100 |
| Eil101     |              629 | 629                     | Yes                 | 101                                    | Yes                             |                        101 |
| Lin105     |            14379 | 14379                   | Yes                 | 105                                    | Yes                             |                        105 |
| Pr107      |            44303 | 44303                   | Yes                 | 107                                    | Yes                             |                        107 |
| Pr124      |            59030 | 59030                   | Yes                 | 124                                    | Yes                             |                        124 |
| Bier127    |           118282 | 118282                  | Yes                 | 127                                    | Yes                             |                        127 |
| Ch130      |             6128 | 6128                    | Yes                 | 130                                    | Yes                             |                        130 |
| KroA150    |            26524 | 26524                   | Yes                 | 150                                    | Yes                             |                        150 |
| KroB150    |            26130 | 26130                   | Yes                 | 150                                    | Yes                             |                        150 |
| U159       |            42080 | 42080                   | Yes                 | 159                                    | Yes                             |                        159 |
| Rat195     |             2335 | 2335                    | Yes                 | 195                                    | Yes                             |                        195 |
| D198       |            15781 | 15781                   | Yes                 | 198                                    | Yes                             |                        198 |
| KroA200    |            29368 | 29368                   | Yes                 | 200                                    | Yes                             |                        200 |
| KroB200    |            29437 | 29437                   | Yes                 | 200                                    | Yes                             |                        200 |
| Lin318     |            42029 | 42029                   | Yes                 | 318                                    | Yes                             |                        318 |
| Fl417      |            11869 | 11869                   | Yes                 | 417                                    | Yes                             |                        417 |
| Pcb442     |            50997 | 50997                   | Yes                 | 442                                    | Yes                             |                        442 |
| Rat575     |             6815 | —                       | Not executed        | —                                      | Yes                             |                        575 |

---

# 9. Discussion

The notebook demonstrates strong empirical performance across classical TSPLIB benchmarks.

The most important result is that the hybrid solver reaches the known optimum on **19 out of 25** benchmark instances.

This includes several non-trivial benchmark cases such as:

- `Pr124`
- `Bier127`
- `KroA150`
- `KroB150`
- `KroA200`
- `KroB200`
- `Lin318`

The algorithm performs especially well when its evolutionary search rapidly discovers high-quality edge structures and local search can refine the route to the benchmark optimum.

The remaining unsolved instances are concentrated among more difficult medium-to-large cases:

- `Ch130`
- `Rat195`
- `D198`
- `Fl417`
- `Pcb442`
- `Rat575`

These cases likely require stronger intensification, additional restarts, larger elite diversity, or more aggressive 3-opt / Lin-Kernighan-style refinement.

A key strength of the notebook is that the route outputs are not only reported as scalar scores. The notebook also prints detailed route sequences, edge distances, route plots, and validation checks.

---

# 10. Conclusion

This notebook presents a strong hybrid Genetic Algorithm and local-search framework for solving the Traveling Salesman Problem on standard TSPLIB instances.

The final audited result is:

- **25 total TSPLIB instances**
- **19 exact optimums achieved**
- **6 near-optimal non-exact solutions**
- **76.00% exact success rate**
- **25/25 closed routes in main outputs**
- **24/25 explicit verification cells executed**
- **24/24 executed verification cells valid**

The results demonstrate that a carefully engineered hybrid of Nearest Neighbor initialization, Genetic Algorithms, Edge Recombination Crossover, inversion mutation, adaptive diversification, and local search can solve many classical TSP benchmarks exactly and produce very strong near-optimal solutions on harder instances.

---

# 11. Recommended Next Improvements

To push the remaining 6 instances to optimum, the next solver version should prioritize:

- multi-start restart loops,
- stronger 3-opt scheduling,
- Lin-Kernighan-style local search,
- edge-frequency memory,
- candidate-neighbor lists,
- population checkpointing,
- elite archive diversity control,
- runtime-normalized comparisons,
- and automatic reruns on only the unsolved instances.

Priority order for improvement should be:

1. `D198` — only 1 unit above optimum.
2. `Fl417` — only 8 units above optimum.
3. `Rat195` — 12 units above optimum.
4. `Ch130` — 18 units above optimum.
5. `Rat575` — 42 units above optimum.
6. `Pcb442` — 219 units above optimum.

The notebook is already strong. The next version should focus on converting these near-optimal results into exact optima.

---

# 12. Final Status

| Final Status | Count |
|---|---:|
| Exact optimum achieved | 19 |
| Near-optimal but not exact | 6 |
| Total instances | 25 |

**Final conclusion:** the uploaded TSP notebook demonstrates exact optimization of **19/25 TSPLIB benchmark instances**, with **6 near-optimal instances remaining**.
