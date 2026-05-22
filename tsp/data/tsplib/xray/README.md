# X-Ray Crystallography TSP Data

This directory contains data and FORTRAN source code for generating Traveling Salesman Problem instances from x-ray crystallography experiments.

## Reference

This data accompanies the TSPLIB distribution. The FORTRAN code computes detector positioning angles (phi, chi, two-theta) for collecting diffraction data at different Miller indices (h, k, l).

## Source Files

| File | Description |
|---|---|
| `DAUX.FOR` | Subroutines: SETPTS (compute coordinates), TCOST (tour cost), ANGLES (detector angles) |
| `DEQ.FOR` | Cost function for equal motor speeds |
| `DUNEQ.FOR` | Cost function for different motor speeds |
| `GENTSP.FOR` | Main program: reads crystal parameters, generates TSP points |

## Crystal Structures

| ID | Crystal | Nodes | Wavelength |
|---|---|---|---|
| a | Ammonium tartrate | 9,070 | 1.35 Å |
| b | Biphenyl | 14,464 | 1.00 Å |
| d | Dinitrodiphenyltetrathiofulvalene | 14,012 | 1.35 Å |
| e | Bis-2-imidazole iron octaethylporphyrin | 13,590 | 1.00 Å |
| f | Iron dipyridyltetraphenylporphyrin | 13,804 | 1.35 Å |

## Data Files

The `.data` files contain crystal orientation matrices and Miller index ranges for generating TSP instances. Each file contains:
1. 3×3 orientation matrix (defining reciprocal lattice vectors)
2. Wavelength (in Ångströms)
3. Miller index ranges (hlo hhi klo khi llo lhi)

## Problem

Given the crystal structure and a set of observable diffraction spots (Miller indices), the detector must move between spots. The cost of moving from one spot to another depends on the motor speeds for the phi, chi, and two-theta axes. The goal is to find the shortest tour visiting all observable spots — a Traveling Salesman Problem.

## Source

From TSPLIB (Reinelt, 1991), originally contributed by David Shallcross and Matt Small.
