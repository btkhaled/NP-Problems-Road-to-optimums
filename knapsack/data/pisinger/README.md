# Pisinger 0-1 Knapsack Benchmark Instances

Source: David Pisinger's 0-1 Knapsack benchmark collection.

## Reference

Pisinger, D. (2005). Where are the hard knapsack problems? *Computers & Operations Research*, 32(9), 2271–2284.

## File Format

Each instance file has:
```
Line 1: <n> <capacity>
Lines 2..n+1: <value> <weight>
```

Optimum files contain a single number: the optimal objective value.

## Families

### Low-Dimensional (10 instances)
- Items: 4 to 23
- Capacities: 11 to 10,000
- Mixed value-weight correlations

### Large-Scale (21 instances)

Three families × 7 sizes (100, 200, 500, 1000, 2000, 5000, 10000):

| Family | Correlation | Difficulty |
|---|---|---|
| PI_1 | Uncorrelated | Easiest |
| PI_2 | Weakly correlated | Moderate |
| PI_3 | Strongly correlated (value = weight + constant) | Hardest |

Note: Some large-scale instance files include the optimal binary solution vector as a final line.

## Files

```
low-dimensional/          (10 instances)
low-dimensional-optimum/  (10 optimum values)
large_scale/              (21 instances)
large_scale-optimum/      (21 optimum values)
```
