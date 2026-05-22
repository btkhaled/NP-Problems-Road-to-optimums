# TSPLIB — Symmetric TSP Benchmark Library

Source: http://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/

## Reference

Reinelt, G. (1991). TSPLIB—A Traveling Salesman Problem Library. *ORSA Journal on Computing*, 3(4), 376–384.

## Contents

- **111 TSP instances** (.tsp files) ranging from 14 to 85,900 cities
- **solutions:** Known optimal/best tour lengths for all instances
- **xray/:** X-ray crystallography TSP problem data (FORTRAN source + crystal parameters)

### Edge-Weight Types

| Type | Description |
|---|---|
| EUC_2D | Euclidean 2D (rounded to nearest integer) |
| GEO | Geographic (latitude/longitude) |
| ATT | Pseudo-Euclidean |
| CEIL_2D | Euclidean 2D (rounded up) |
| MATRIX | Explicit matrix |

## Files

```
*.tsp           111 instance files
solutions       Known optimal tour lengths (111 entries)
xray/           X-ray crystallography data
```
