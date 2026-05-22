# SATLIB UF250 Benchmark Suite

Source: SATLIB — Uniform Random-3-SAT benchmark suite.

## Reference

Hoos, H. H., & Stützle, T. (2000). SATLIB: An online resource for research on SAT. In *SAT 2000*, 283–292.

## Instance Parameters

| Parameter | Value |
|---|---|
| Variables | 250 |
| Clauses | 1065 |
| Literals per clause | 3 |
| Clause/Variable ratio | 4.26 |
| Number of instances | 100 |
| All satisfiable | Yes |

## DIMACS CNF Format

```
c comment line
p cnf <variables> <clauses>
<lit1> <lit2> <lit3> 0
...
```

Each line is a clause terminated by 0. Positive integers = positive literals, negative integers = negated literals.

## Files

```
uf250-01.cnf  to  uf250-100.cnf
```

## Source

https://www.cs.ubc.ca/~hoos/SATLIB/benchm.html
