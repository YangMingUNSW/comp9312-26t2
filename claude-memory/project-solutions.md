---
name: project-solutions
description: Approaches used for the COMP9312 26T2 project (Q1 first cycle edge, Q2 k-triangle core)
metadata:
  node_type: memory
  type: project
---

The `project/` folder holds the COMP9312 26T2 project (25 marks, due 9pm Fri 24
July 2026). Deliverables: `Q1.ipynb`/`Q1.pdf`, `Q2.ipynb`/`Q2.pdf` on Moodle;
PDFs are built in Overleaf from `Q1.tex`/`Q2.tex` (readable mirrors in
`Q*_solution.md`). Only Python Standard Library is allowed.

- **Q1 — first cycle-causing edge (10 marks).** Union-find over the insertion
  stream: the first edge whose endpoints already share a component is the answer;
  recover the cycle by BFS over the spanning forest and close it. `query(n, L)`
  in `FirstCycleEdgeQuery`. `O(m*alpha(n)+n)` time, `O(n)` space. Verified against
  all 3 official UNSW server tests (`q1_test_*` incl. a large n=10680 graph).

- **Q2 — index-based k-triangle core search (15 marks).** Non-obvious subtlety:
  a k-triangle core uses **triangle connectivity**, not plain graph connectivity
  — the bridge `(5,6)` in the spec figure is in no triangle so it must NOT join
  the two sides. Solution: triangle-core peeling (Batagelj–Zaversnik on triangle
  counts) gives `tc(v)`; per-edge `level(u,w)=max_tri min(tc)` lets `query(k,v)`
  be a threshold DFS. `build` `O(m^1.5+n)`, query output-sensitive
  `O(sum_{x in C} d(x))`, space `O(n+m)`. Passes all local tests incl. the
  10k/80k benchmark (build ~0.03s) and 500+ brute-force cross-checks.

See [[comp9312-repo]], [[solution-file-convention]], [[readability-first]].
