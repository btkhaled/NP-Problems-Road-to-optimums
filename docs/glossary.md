# Glossary

**Khaled Ben Taïeb** · bt.khaled@gmail.com

---

### 2-opt
A local search operator for TSP that removes two edges from a tour and reconnects the two resulting paths in a different way, potentially reducing total tour length.

### 3-opt
An extension of 2-opt that considers three edge removals and four possible reconnection patterns. More powerful but computationally more expensive.

### 3-SAT / SAT3
The Boolean satisfiability problem restricted to clauses containing exactly three literals. This is the classical NP-complete version of SAT (Karp's #2).

### B&B (Branch-and-Bound)
An exact algorithm for discrete optimization that systematically explores decision branches while pruning subtrees that cannot contain an optimal solution, using upper/lower bounds.

### Best-First Search
A search strategy that always expands the most promising node first (highest upper bound), commonly implemented via a priority queue.

### CDCL (Conflict-Driven Clause Learning)
A complete SAT solving paradigm combining backtracking with clause learning, unit propagation, and restart mechanisms. The dominant approach in modern SAT solvers.

### Dantzig Bound
The fractional knapsack upper bound obtained by allowing fractional items. Computed by filling the knapsack greedily by value-to-weight ratio, taking the last item fractionally.

### DIMACS CNF
Standard file format for SAT benchmarks. Header: `p cnf <variables> <clauses>`. Each clause is a space-separated list of literals (positive or negative integers), terminated by 0.

### ERX (Edge Recombination Crossover)
A crossover operator for permutation-based GAs used in TSP that preserves edges common to both parents, improving solution quality by maintaining adjacency information.

### EUC_2D
A TSPLIB edge-weight type where distances are computed as Euclidean distances in a 2D plane, rounded to the nearest integer.

### FPTAS (Fully Polynomial-Time Approximation Scheme)
An algorithm that finds a (1-ε)-approximation in time polynomial in both n and 1/ε. The 0-1 Knapsack problem admits an FPTAS.

### GA (Genetic Algorithm)
A population-based metaheuristic inspired by natural selection, using operators such as selection, crossover (recombination), and mutation.

### GSAT
A greedy local search algorithm for SAT that flips the variable with the largest net gain in satisfied clauses. Often combined with random restarts and noise strategies.

### Incumbent
The best feasible solution found so far during an optimization process. Used for pruning in Branch-and-Bound.

### Inversion Mutation
A TSP mutation operator that reverses a random contiguous segment of a tour. Preserves permutation validity.

### Karp's 21
The 21 NP-complete problems identified by Richard Karp in 1972, including SAT, 3-SAT, TSP, and Knapsack.

### KP (Knapsack Problem)
Given n items with values v_i and weights w_i, and a capacity C, select a subset maximizing total value while not exceeding capacity.

### Lin-Kernighan
A powerful variable-depth local search heuristic for TSP. Generalizes k-opt by dynamically varying the number of edges to replace.

### Make/Break Scores
SAT local search heuristics evaluating variable flips. "Make" = number of newly satisfied clauses. "Break" = number of newly unsatisfied clauses.

### Nearest Neighbor (NN)
A constructive TSP heuristic: starting from a random city, repeatedly visit the nearest unvisited city. Fast but typically 15–20% above optimum.

### NP (Nondeterministic Polynomial Time)
The complexity class of decision problems whose solutions can be verified in polynomial time.

### NP-complete
The intersection of NP and NP-hard. The hardest problems in NP; a polynomial-time algorithm for any NP-complete problem would imply P = NP.

### NP-hard
Problems at least as hard as the hardest NP problems. All problems in NP reduce to any NP-hard problem in polynomial time.

### P (Polynomial Time)
The complexity class of problems solvable in deterministic polynomial time.

### Pisinger Instances
A widely-used benchmark collection for the 0-1 Knapsack problem, created by David Pisinger. Includes uncorrelated, weakly correlated, and strongly correlated families.

### SAT (Boolean Satisfiability)
The problem of determining whether there exists a truth assignment satisfying a given Boolean formula. The first problem proven NP-complete (Cook-Levin).

### SATLIB
A comprehensive online library of SAT benchmark instances, maintained by Holger Hoos and Thomas Stützle.

### Strongly Correlated Instances
Knapsack instances where value = weight + constant. These are among the hardest KP instances for Branch-and-Bound solvers.

### TSP (Traveling Salesman Problem)
The problem of finding the shortest Hamiltonian cycle in a complete graph. Given n cities and pairwise distances, find a tour visiting each city exactly once and returning to the start.

### TSPLIB
A library of TSP benchmark instances maintained by Gerhard Reinelt, ranging from 14 to 85,900 cities.

### UF250
A SATLIB benchmark suite of 100 satisfiable uniform random 3-SAT instances with 250 variables and 1065 clauses each.

### WalkSAT
A stochastic local search algorithm for SAT that combines random walks with greedy moves. Developed by Selman, Kautz, and Cohen (1994). One of the most effective stochastic SAT solvers.
