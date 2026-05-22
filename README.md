<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=0,2,4,6,8&height=180&section=header&text=NP%20Problems%20—%20Road%20to%20Optimums&fontSize=38&fontAlignY=30&desc=146/155%20NP-hard%20instances%20solved%20(94.2%25)&descAlignY=50&animation=fadeIn" width="100%"/>
</p>

<div align="center">

**Khaled Ben Taïeb** · bt.khaled@gmail.com

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)]()
[![Knapsack 90%](https://img.shields.io/badge/Knapsack-90%25-00C853)]()
[![SAT3 100%](https://img.shields.io/badge/3--SAT-100%25-00E676)]()
[![TSP 76%](https://img.shields.io/badge/TSP-76%25-FF9100)]()
[![DOI](https://img.shields.io/badge/DOI-pending-lightgrey)]()
![Visitors](https://api.visitorbadge.io/api/visitors?path=https%3A%2F%2Fgithub.com%2Fbtkhaled%2FNP-Problems%2DRoad-to-optimums&label=Visitors&countColor=%23555555)

</div>

---

## 🧭 Navigation

[🏆 Podium](#-the-podium) · [📊 Benchmarks](#-benchmark-arsenal) · [🔬 Highlights](#-key-highlights) ·
[📂 Structure](#-repository-map) · [🚀 Quick Start](#-quick-start) ·
[🧪 Colab](#-run-in-colab) · [📄 Papers](#-scientific-papers) · [📖 Citation](#-citation)

---

## 🏆 The Podium

| 🥇 **3‑SAT** | 🥈 **0‑1 Knapsack** | 🥉 **TSP** |
|---|---|---|
| 🟢 100% (100/100) | 🟡 90% (27/30) | 🟠 76% (19/25 exact) |
| WalkSAT + GSAT | GA + Branch‑&‑Bound | GA + 2‑opt/3‑opt |
| [📄 Paper](docs/papers/paper-sat3-kbt.md) · [📓 NB](sat/notebooks/SAT3_KBT.ipynb) | [📄 Paper](docs/papers/paper-knapsack-kbt.md) · [📓 NB](knapsack/notebooks/KNAPSACK_KBT.ipynb) | [📄 Paper](docs/papers/paper-tsp-kbt.md) · [📓 NB](tsp/notebooks/TSP_KBT.ipynb) |

---

## 📊 Benchmark Arsenal

| Problem | Source | Instances | Tested | Solved | Rate | Algorithm |
|---|---|---|---|---|---|---|
| 🎒 **0-1 Knapsack** | Pisinger (2005) | 31 | 30 | **27** | **90.0%** | GA + Best‑First B&B |
| 🧩 **3-SAT** | SATLIB UF250 | 100 | 100 | **100** | **100.0%** | WalkSAT + GSAT |
| ✈️ **TSP** | TSPLIB (Reinelt, 1991) | 111 | 25 | **19** | **76.0%** | GA + ERX + 2‑opt/3‑opt |
| **🧬 TOTAL** | **3 benchmarks** | **242** | **155** | **146** | **94.2%** | **Hybrid framework** |

<pre>
<b>Performance Spectrum</b>

┌────────────────────────────────────────────────────────────────────────────┐
│                         <b>📊  PERFORMANCE SPECTRUM</b>                     │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                            │
│   🎒  0‑1 KNAPSACK                                                         │
│       ██████████████████████████████████████████████░░░░░  90% (27/30)     │
│                                                                            │
│   🧩  3‑SAT                                                                │
│       ██████████████████████████████████████████████████████████  100% (100)│
│                                                                            │
│   ✈️  TRAVELING SALESMAN                                                   │
│       ██████████████████████████████████████░░░░░░░░░░░░░░  76% (19/25)    │
│                                                                            │
├────────────────────────────────────────────────────────────────────────────┤
│   🏆  TOTAL (155 instances)                                                │
│       ████████████████████████████████████████████████░░░░░░  94% (146)    │
│                                                                            │
└────────────────────────────────────────────────────────────────────────────┘
</pre>

---

## 🔬 Key Highlights

| 🏅 | Achievement | Detail |
|---|---|---|
| 🥇 | **Perfect score** | SAT3: 100/100 instances solved, 61% under 1 second |
| 🥇 | **Exact optimality** | 19 TSPLIB instances solved to known optimum (up to 318 cities) |
| 🥇 | **Industrial scale** | Knapsack: 10,000-item instance solved optimally (value 563,647) |
| 🥈 | **Near‑optimal** | TSP: 6 instances within 0.32% avg gap of known optima |
| 🥉 | **Research‑ready** | 4 scientific papers, 242 cataloged instances, full reproducibility |

---

## 📂 Repository Map

```
NP‑Problems‑Road‑to‑optimums/
│
├── 📖 README.md                # You are here
├── 📜 LICENSE                  # MIT
├── 🆔 CITATION.cff             # Citation metadata
│
├── 📚 docs/                    # Scientific Documentation
│   ├── 📑 papers/              # 4 full research papers
│   │   ├── paper‑knapsack‑kbt.md
│   │   ├── paper‑sat3‑kbt.md
│   │   ├── paper‑tsp‑kbt.md
│   │   └── paper‑complexity‑landscape.md
│   ├── methodology.md
│   ├── theoretical‑background.md
│   ├── benchmark‑catalog.md
│   └── glossary.md
│
├── 🎒 knapsack/                # 0‑1 Knapsack Problem
│   ├── data/pisinger/          # 62 Pisinger benchmark files
│   ├── notebooks/KNAPSACK_KBT.ipynb
│   └── results/                # Paper + summary
│
├── 🧩 sat/                     # 3‑SAT Problem
│   ├── data/UF250.1065.100/    # 100 DIMACS CNF instances
│   ├── notebooks/SAT3_KBT.ipynb
│   └── results/
│
├── ✈️ tsp/                      # Traveling Salesman Problem
│   ├── data/tsplib/            # 111 TSPLIB instances + solutions
│   │   └── xray/               # X‑ray crystallography (FORTRAN)
│   ├── notebooks/TSP_KBT.ipynb
│   └── results/
```

---

## 🚀 Quick Start

```bash
git clone https://github.com/btkhaled/NP-Problems-Road-to-optimums.git
cd NP-Problems-Road-to-optimums
```

Each notebook is fully self‑contained — no dependencies to install. Open in Colab or locally with Jupyter.

---

## 🧪 Run in Colab

Zero‑install execution. Click and run.

[![Open In Colab - Knapsack](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/btkhaled/NP-Problems-Road-to-optimums/blob/main/knapsack/notebooks/KNAPSACK_KBT.ipynb)
[![Open In Colab - SAT3](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/btkhaled/NP-Problems-Road-to-optimums/blob/main/sat/notebooks/SAT3_KBT.ipynb)
[![Open In Colab - TSP](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/btkhaled/NP-Problems-Road-to-optimums/blob/main/tsp/notebooks/TSP_KBT.ipynb)

---

## 📄 Scientific Papers

| Paper | Focus | Pages | Key Result |
|---|---|---|---|
| [🎒 Knapsack](docs/papers/paper-knapsack-kbt.md) | GA + B&B on Pisinger KP | ~15 | 27/30 solved (90%), 3 PI_3 unsolved |
| [🧩 SAT3](docs/papers/paper-sat3-kbt.md) | WalkSAT + GSAT on UF250 | ~15 | 100/100 solved (100%), avg 5.34s |
| [✈️ TSP](docs/papers/paper-tsp-kbt.md) | GA + 2-opt/3-opt on TSPLIB | ~20 | 19/25 exact (76%), avg gap 0.32% |
| [🧬 Complexity Landscape](docs/papers/paper-complexity-landscape.md) | Cross‑problem NP survey | ~10 | Comparative analysis of Karp's 21 |

---

## 📖 Citation

```bibtex
@misc{bentaieb2025npproblems,
  author = {Khaled Ben Taïeb},
  title  = {{NP Problems — Road to Optimums: Hybrid Solvers for 0-1 Knapsack,
            3-SAT, and the Traveling Salesman Problem}},
  year   = {2025},
  howpublished = {\url{https://github.com/btkhaled/NP-Problems-Road-to-optimums}}
}
```

---

## 📚 References

1. **Cook, S. A.** (1971). The complexity of theorem-proving procedures. *STOC*, 151–158.
2. **Karp, R. M.** (1972). Reducibility among combinatorial problems. *Complexity of Computer Computations*, 85–103.
3. **Pisinger, D.** (2005). Where are the hard knapsack problems? *Computers & Operations Research*, 32(9), 2271–2284.
4. **Reinelt, G.** (1991). TSPLIB—A Traveling Salesman Problem Library. *ORSA Journal on Computing*, 3(4), 376–384.
5. **Selman, B., Kautz, H., & Cohen, B.** (1994). Noise strategies for improving local search. *AAAI*, 337–343.
6. **Selman, B., Levesque, H., & Mitchell, D.** (1992). A new method for solving hard satisfiability problems. *AAAI*, 440–446.
7. **Hoos, H. H., & Stützle, T.** (2004). *Stochastic Local Search: Foundations & Applications*. Morgan Kaufmann.
8. **Lin, S., & Kernighan, B. W.** (1973). An effective heuristic algorithm for the traveling-salesman problem. *Operations Research*, 21(2), 498–516.
9. **Garey, M. R., & Johnson, D. S.** (1979). *Computers and Intractability*. W. H. Freeman.
10. **Mitchell, D., Selman, B., & Levesque, H.** (1992). Hard and easy distributions of SAT problems. *AAAI*, 459–465.

---

<p align="center">
  <sub>Built with 🧠, ☕, and an unreasonable obsession with optimality.</sub>
  <br>
  <sub>Khaled Ben Taïeb · bt.khaled@gmail.com · 2026</sub>
  <br>
  <br>
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=0,2,4,6,8&height=120&section=footer" width="100%"/>
</p>
