# Benchmark Catalog — Complete Instance Inventory

**Khaled Ben Taïeb** · bt.khaled@gmail.com

This document catalogs all 242 benchmark instances across the three NP-hard problems.

---

## 1. 0-1 Knapsack — Pisinger Instances (31 files)

Source: David Pisinger's 0-1 Knapsack benchmark collection.

### 1.1 Low-Dimensional (10 instances)

| File | Items | Capacity | Optimal Value | Notes |
|---|---|---|---|---|
| f1_l-d_kp_10_269 | 10 | 269 | 295 | |
| f2_l-d_kp_20_878 | 20 | 878 | 1024 | |
| f3_l-d_kp_4_20 | 4 | 20 | 35 | |
| f4_l-d_kp_4_11 | 4 | 11 | 23 | |
| f5_l-d_kp_15_375 | 15 | 375 | 481.069368 | Real-valued weights |
| f6_l-d_kp_10_60 | 10 | 60 | 52 | |
| f7_l-d_kp_7_50 | 7 | 50 | 107 | |
| f8_l-d_kp_23_10000 | 23 | 10000 | 9767 | |
| f9_l-d_kp_5_80 | 5 | 80 | 130 | |
| f10_l-d_kp_20_879 | 20 | 879 | 1025 | |

All solved (100%).

### 1.2 Large-Scale (21 instances, 20 tested)

Three families × 7 sizes. The `knapPI_1_200_1000_1` instance exists in data but was not tested.

**Family PI_1** (uncorrelated):
- 100 items: capacity 995, optimum 9147
- [200 items] ← not tested
- 500 items: capacity 2543, optimum 28857
- 1000 items: capacity 5002, optimum 54503
- 2000 items: capacity 10011, optimum 110625
- 5000 items: capacity 25016, optimum 276457
- 10000 items: capacity 49877, optimum 563647

**Family PI_2** (weakly correlated):
- 100 items: capacity 995, optimum 1514
- 200 items: capacity 1008, optimum 1634
- 500 items: capacity 2543, optimum 4566
- 1000 items: capacity 5002, optimum 9052
- 2000 items: capacity 10011, optimum 18051
- 5000 items: capacity 25016, optimum 44356
- 10000 items: capacity 49877, optimum 90204

**Family PI_3** (strongly correlated — hardest):
- 100 items: capacity 997, optimum 2397
- 200 items: capacity 997, optimum 2697
- 500 items: capacity 2517, optimum 7117
- 1000 items: capacity 4990, optimum 14390
- 2000 items: capacity 9819, optimum ⟵ **unsolved**
- 5000 items: capacity 24805, optimum ⟵ **interrupted**
- 10000 items: capacity 49519, optimum ⟵ **unsolved**

---

## 2. 3-SAT — SATLIB UF250 (100 instances)

Source: SATLIB (Uniform Random-3-SAT benchmark suite).

### Parameters
- **Variables:** 250 per instance
- **Clauses:** 1065 per instance (ratio 4.26)
- **Literals per clause:** 3
- **Type:** All satisfiable

### File naming
`uf250-{01..100}.cnf` (DIMACS CNF format)

### Runtime distribution (all 100 solved)

| Range | Count |
|---|---|
| < 1 second | 61 |
| 1 – 10 seconds | 26 |
| 10 – 30 seconds | 9 |
| 30 – 100 seconds | 3 |
| > 100 seconds | 1 |

**Fastest:** uf250-061 — 0.012 sec
**Slowest:** uf250-054 — 239.931 sec
**Median:** 0.731 sec
**Average:** 5.341 sec

---

## 3. TSP — TSPLIB Symmetric Instances (111 files)

Source: TSPLIB95 (Reinelt, 1991).

### Instance size distribution

| Category | Size Range | Count | Examples |
|---|---|---|---|
| Tiny | < 50 cities | 11 | burma14, ulysses16, gr17, gr21, gr24, fri26, bayg29, bays29, att48, gr48, hk48 |
| Small | 50 – 100 | 12 | eil51, berlin52, brazil58, st70, eil76, pr76, rat99, rd100, kroA/B/C/D/E100 |
| Medium | 100 – 300 | 27 | eil101, lin105, pr107, bier127, ch130, pr136, gr137, kroA/B150, u159, si175, brg180, rat195, d198, kroA/B200, tsp225, ts225, gil262 |
| Large | 300 – 1000 | 16 | a280, pr299, lin318, linhp318, fl417, pr439, pcb442, d493, att532, si535, pa561, rat575, d657, gr666, u724, rat783 |
| Very Large | 1000 – 5000 | 22 | dsj1000, pr1002, si1032, u1060, vm1084, d1291, rl1304, rl1323, nrw1379, fl1400, u1432, fl1577, d1655, vm1748, u1817, rl1889, d2103, u2152, u2319, pr2392, pcb3038 |
| Massive | 5000 – 90000 | 11 | fl3795, fnl4461, rl5915, rl5934, pla7397, rl11849, usa13509, d15112, d18512, pla33810, pla85900 |

**Total: 111 instances** (25 tested in this repository).

### Edge-weight types

| Type | Description | Count |
|---|---|---|
| EUC_2D | 2D Euclidean coordinates | Majority |
| GEO | Geographic (latitude/longitude) | Several |
| ATT | Pseudo-Euclidean (ATT distance) | att48, att532 |
| CEIL_2D | 2D Euclidean, rounded up | dsj1000 |
| MATRIX | Explicit distance matrix | A few |

### Tested instances (25)

| Status | Count | Instances |
|---|---|---|
| **Exact optimum** | 19 | eil51, berlin52, st70, eil76, pr76, rat99, kroA100, kroB100, eil101, lin105, pr107, pr124, bier127, kroA150, kroB150, u159, kroA200, kroB200, lin318 |
| **Near-optimal** | 6 | ch130 (gap 0.29%), rat195 (0.52%), d198 (0.006%), fl417 (0.067%), pcb442 (0.43%), rat575 (0.62%) |

---

## Summary Statistics

| Metric | Knapsack | SAT3 | TSP | Total |
|---|---|---|---|---|
| Data files | 62 | 100 | 113 | **275** |
| Instances tested | 30 | 100 | 25 | **155** |
| Solved / satisfied | 27 | 100 | 19 exact | **146** |
| Success rate | 90.00% | 100.00% | 76.00% | **94.19%** |
