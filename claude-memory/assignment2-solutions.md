---
name: assignment2-solutions
description: Answers and conventions used for COMP9312 26T2 Assignment 2 (patterns, WL kernel, reachability indexing, GNN)
metadata:
  node_type: memory
  type: project
---

`assignment2/` holds COMP9312 26T2 Assignment 2 (20 marks, 10%, due 21:00 Fri
7 Aug 2026). Deliverable: one `ass2_z5565446.pdf` on Moodle, built from
`ass2_z5565446.tex`.

**The question page is entirely inline SVG** — fetching it as text loses every graph.
The seven figures were decoded from the SVG coordinates into `assignment2/assets/*.svg`
plus YAML edge lists in `ass2.md`; start from those, never from a text scrape.
`ass2.md` is a **verbatim** transcription of the page (the user hand-checks it against the
website), with every non-page addition flagged inline by a `🔧 [非原文]` marker — keep it
that way.

Answers (all verified by an independent Python script — brute-force isomorphism,
all-pairs reachability cross-checks, exact rational arithmetic):

- **Q1.** `G` = `A-B,A-C,B-C,B-D,C-D,D-E,D-F,E-F,E-G`; `P` = triangle `pqr` + pendant `s`.
  Homomorphism `p→B,q→C,r→D,s→E`. Exactly 5 induced copies: `{B,C,D,E}`, `{B,C,D,F}`,
  `{B,D,E,F}`, `{C,D,E,F}`, `{D,E,F,G}` (= a triangle plus a 4th vertex with *exactly one*
  edge into it).
- **Q2.** Colour IDs restart at 0 each round (lexicographic over the joint tuple set);
  continuing them across rounds changes neither `φ` nor the kernel. `K_WL = 36+14+4 = 54`;
  round 2 is the first round that separates the two trees.
- **Q3(a).** Total-degree hub order `5,6,2,4,10,1,3,7,8,9,11`, pruned forward+backward BFS,
  self-labels included. 44 entries (23 `L_in` + 21 `L_out`). **Trap:** it is easy to run only
  the backward BFS for a hub — missing hub 4's *forward* pass wrongly drops `4` from
  `L_in(4)/L_in(8)/L_in(9)` and wrongly leaves `8` in `L_out(4)`. The result is stable under
  the forward-first and rank-restricted variants of the algorithm.
- **Q3(b).** Left-to-right post-order intervals + reverse-topological propagation, removing
  only *contained* intervals (never merging adjacent/overlapping ones). Tree 1 = **26**
  intervals, Tree 2 = **31** → Tree 1 wins because it keeps the DAG's converging tail
  `{4,8,10,11,12,13,14}` in one subtree, while Tree 2 scatters it over three sibling
  branches and leaves vertex 4 a leaf. Note both trees capture the same 33/61 reachable
  pairs by tree edges alone, so *that* statistic does not discriminate — use the interval
  totals.
- **Q4.** `h_B⁽¹⁾=(0.00,0.50)ᵀ`, `h_C⁽¹⁾=(1.00,0.33)ᵀ`; adding `B–D` affects exactly
  `{B, D}` (check all four vertices explicitly — ReLU can mask a change).

**Two plausible-sounding justifications that are FALSE** — both were written into the
solution and only caught on a later review pass, because scripts that check numbers cannot
check reasoning:

1. *"A non-tree edge `u→w` is free because every interval of `L(w)` is contained in
   `I(u)`."* False — Tree 1's edge `2→9` is free although `L(9)={[1,1],[7,8]}` is not inside
   `I(2)=[1,4]`. The real reason is that `w` is a tree **descendant** of `u`, so the tree
   path already carries `L(w)` upward. Containment is a *separate* effect: it is what
   collapses `L(u)` to `{I(u)}`, which happens exactly when everything `u` reaches lies in
   `u`'s subtree.
2. *"The only hub candidates for a direct edge `u→v` are `u` and `v`."* False — any vertex
   on another `u`–`v` path qualifies (Figure 4: edge `2→5` also has the path `2→1→5`).
   Justify self-labels from the prescribed algorithm instead: each hub's search starts at
   the hub, and `L_out(k) ∩ L_in(k)` must be empty beforehand in a DAG (a common hub would
   imply a cycle), so `k` is never pruned at itself.

**How to apply:** verifying computed values is not the same as verifying the argument
offered for them. Re-read every "because …" clause on its own and try to break it.

See [[comp9312-repo]], [[solution-file-convention]], [[readability-first]].
