# COMP9312 — Assignment 2 Solutions

**YUHUI LUO (z5565446)**

Question text and figure data: [`ass2.md`](ass2.md). This is the readable mirror of
[`ass2_z5565446.tex`](ass2_z5565446.tex), which is the source for the submitted PDF.

---

## Q1. Graph homomorphism and induced subgraph matching

**Data graph `G`:** `A-B, A-C, B-C, B-D, C-D, D-E, D-F, E-F, E-G`
**Query graph `P`:** triangle `{p,q,r}` plus a pendant `s` attached to `r`.

### (a) A homomorphism from `P` to `G`

A homomorphism only has to map every **edge** of `P` to an edge of `G`; distinct vertices
of `P` may collapse onto the same vertex of `G`. One exists, and here we do not even need
to collapse anything:

| `P` | `p` | `q` | `r` | `s` |
| --- | --- | --- | --- | --- |
| ↦ `G` | `B` | `C` | `D` | `E` |

Checking all four edges of `P`:

| edge of `P` | image | in `G`? |
| --- | --- | --- |
| `p-q` | `B-C` | yes |
| `p-r` | `B-D` | yes |
| `q-r` | `C-D` | yes |
| `r-s` | `D-E` | yes |

> **Answer (a).** A homomorphism exists: `p ↦ B, q ↦ C, r ↦ D, s ↦ E`.

### (b) All induced subgraphs of `G` isomorphic to `P`

`P` has 4 vertices and 4 edges: a triangle plus one pendant vertex. So a vertex set
`S ⊆ V(G)` with `|S| = 4` works **iff** three of its vertices form a triangle and the
fourth has **exactly one** edge into that triangle (exactly one, because an induced
subgraph keeps every edge of `G` inside `S` — a second edge would give 5 edges, and no
edge would leave the set disconnected).

`G` has exactly three triangles: `{A,B,C}`, `{B,C,D}`, `{D,E,F}`. Extend each by one
vertex and count the edges it sends into the triangle:

| triangle | 4th vertex | edges into the triangle | induced copy of `P`? |
| --- | --- | --- | --- |
| `{A,B,C}` | `D` | `D-B`, `D-C` (2) | no |
| `{A,B,C}` | `E` | — (0) | no |
| `{A,B,C}` | `F` | — (0) | no |
| `{A,B,C}` | `G` | — (0) | no |
| `{B,C,D}` | `A` | `A-B`, `A-C` (2) | no |
| `{B,C,D}` | `E` | `E-D` (1) | **yes** |
| `{B,C,D}` | `F` | `F-D` (1) | **yes** |
| `{B,C,D}` | `G` | — (0) | no |
| `{D,E,F}` | `A` | — (0) | no |
| `{D,E,F}` | `B` | `B-D` (1) | **yes** |
| `{D,E,F}` | `C` | `C-D` (1) | **yes** |
| `{D,E,F}` | `G` | `G-E` (1) | **yes** |

> **Answer (b).** Exactly 5 vertex sets:
> `{B,C,D,E}`, `{B,C,D,F}`, `{B,D,E,F}`, `{C,D,E,F}`, `{D,E,F,G}`.

---

## Q2. Weisfeiler-Lehman color refinement

**`G`:** `2-3, 2-4, 1-2, 1-5, 5-6` **`G'`:** `a-d, a-b, a-c, b-e, c-f`

Both are trees on 6 vertices with degree sequence `[3,2,2,1,1,1]`, so they cannot be told
apart by degrees alone.

Convention used, exactly as stated in the question: a vertex's tuple is
`(own colour, sorted multiset of neighbour colours)`; each round the distinct tuples
occurring in **either** graph are sorted lexicographically and given IDs `0, 1, 2, …`.
*(Restarting IDs at 0 each round is what "starting from 0" prescribes; continuing the
numbering across rounds would shift every ID by a constant and change neither the entries
of any `φ_t` nor the kernel, since `φ_t(G)` and `φ_t(G')` are always indexed the same way.)*

### Round 0

Every vertex has colour `0`, so there is a single colour and

`φ₀(G) = [6]`,  `φ₀(G') = [6]`.

### Round 1

| ID | tuple | which vertices |
| --- | --- | --- |
| 0 | `(0, [0])` | degree-1 vertices |
| 1 | `(0, [0,0])` | degree-2 vertices |
| 2 | `(0, [0,0,0])` | degree-3 vertices |

(Lexicographic order on the flattened tuples: `(0,0) < (0,0,0) < (0,0,0,0)`.)

| `G` vertex | tuple | new colour | | `G'` vertex | tuple | new colour |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | `(0, [0,0])` | 1 | | a | `(0, [0,0,0])` | 2 |
| 2 | `(0, [0,0,0])` | 2 | | b | `(0, [0,0])` | 1 |
| 3 | `(0, [0])` | 0 | | c | `(0, [0,0])` | 1 |
| 4 | `(0, [0])` | 0 | | d | `(0, [0])` | 0 |
| 5 | `(0, [0,0])` | 1 | | e | `(0, [0])` | 0 |
| 6 | `(0, [0])` | 0 | | f | `(0, [0])` | 0 |

`φ₁(G) = [3, 2, 1]`,  `φ₁(G') = [3, 2, 1]` — still identical (round 1 just counts degrees).

### Round 2

Tuples are built from the round-1 colours above.

| `G` vertex | round-1 colour | neighbours' colours | tuple | new colour |
| --- | --- | --- | --- | --- |
| 1 | 1 | 2 (c=2), 5 (c=1) | `(1, [1,2])` | 4 |
| 2 | 2 | 1 (c=1), 3 (c=0), 4 (c=0) | `(2, [0,0,1])` | 5 |
| 3 | 0 | 2 (c=2) | `(0, [2])` | 1 |
| 4 | 0 | 2 (c=2) | `(0, [2])` | 1 |
| 5 | 1 | 1 (c=1), 6 (c=0) | `(1, [0,1])` | 2 |
| 6 | 0 | 5 (c=1) | `(0, [1])` | 0 |

| `G'` vertex | round-1 colour | neighbours' colours | tuple | new colour |
| --- | --- | --- | --- | --- |
| a | 2 | b (c=1), c (c=1), d (c=0) | `(2, [0,1,1])` | 6 |
| b | 1 | a (c=2), e (c=0) | `(1, [0,2])` | 3 |
| c | 1 | a (c=2), f (c=0) | `(1, [0,2])` | 3 |
| d | 0 | a (c=2) | `(0, [2])` | 1 |
| e | 0 | b (c=1) | `(0, [1])` | 0 |
| f | 0 | c (c=1) | `(0, [1])` | 0 |

Seven distinct tuples occur across the two graphs; sorted lexicographically they receive
IDs `0`–`6`:

| ID | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| tuple | `(0,[1])` | `(0,[2])` | `(1,[0,1])` | `(1,[0,2])` | `(1,[1,2])` | `(2,[0,0,1])` | `(2,[0,1,1])` |
| count in `G` | 1 | 2 | 1 | 0 | 1 | 1 | 0 |
| count in `G'` | 2 | 1 | 0 | 2 | 0 | 0 | 1 |

`φ₂(G) = [1, 2, 1, 0, 1, 1, 0]`,  `φ₂(G') = [2, 1, 0, 2, 0, 0, 1]`

Round 2 is the first round that separates the two trees: in `G` the degree-3 vertex sees
two leaves and one degree-2 vertex, whereas in `G'` it sees one leaf and two degree-2
vertices.

### (b) The two-round WL kernel

| `t` | `φ_t(G)` | `φ_t(G')` | `φ_t(G)ᵀ φ_t(G')` |
| --- | --- | --- | --- |
| 0 | `[6]` | `[6]` | `6·6 = 36` |
| 1 | `[3,2,1]` | `[3,2,1]` | `9 + 4 + 1 = 14` |
| 2 | `[1,2,1,0,1,1,0]` | `[2,1,0,2,0,0,1]` | `1·2 + 2·1 = 4` |

> **Answer.** `K_WL(G, G') = 36 + 14 + 4 = **54**`.

---

## Q3. Reachability indexing

### (a) Minimal 2-hop labelling

**Convention.** `L_out(u)` holds hubs reachable *from* `u`, `L_in(v)` holds hubs that
*reach* `v`, and `u ⇝ v ⟺ L_out(u) ∩ L_in(v) ≠ ∅`.

Every vertex appears in its own two labels. That is *forced*, not a stylistic choice: to
certify a direct edge `u→v` the shared hub must be reachable from `u` and must reach `v`,
and in a DAG the only candidates are `u` and `v` themselves — so either `u ∈ L_out(u)` and
`u ∈ L_in(v)`, or `v ∈ L_out(u)` and `v ∈ L_in(v)`. Either way some vertex sits in its own
label, so each hub `k` inserts itself into `L_in(k)` and `L_out(k)` when processed.
(Consistently, `L_out(k) ∩ L_in(k) = ∅` just before that insertion: a hub `h ≠ k` in both
would mean `k ⇝ h` and `h ⇝ k`, a cycle.)

**Processing order.** Total degree = in-degree + out-degree, decreasing, ties by smaller ID:

| v | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| in | 1 | 1 | 0 | 2 | 3 | 3 | 1 | 2 | 3 | 3 | 2 |
| out | 2 | 3 | 3 | 2 | 3 | 3 | 2 | 1 | 0 | 1 | 1 |
| **total** | 3 | 4 | 3 | 4 | **6** | **6** | 3 | 3 | 3 | 4 | 3 |

> **Order:** `5, 6, 2, 4, 10, 1, 3, 7, 8, 9, 11`

**Algorithm.** Take the hubs `k` in that order. For each `k` run a *backward* BFS (over
reversed edges) and a *forward* BFS. On reaching a vertex `x`, first test whether the
labels built **so far** already prove the corresponding reachability
(`L_out(x) ∩ L_in(k) ≠ ∅` backward, `L_out(k) ∩ L_in(x) ≠ ∅` forward). If they do, **prune**:
add nothing and do not expand past `x` — anything behind `x` is covered through the same
earlier hub. Otherwise add `k` to `L_out(x)` (backward) or `L_in(x)` (forward) and keep
going. This yields a *minimal* cover: no single entry can be deleted without breaking
some reachable pair.

**Round-by-round trace** (`+` = hub added there, `✂` = pruned):

| hub `k` | backward BFS → `L_out` | forward BFS → `L_in` |
| --- | --- | --- |
| **5** | `+1, +2, +3, +5, +6, +7` | `+4, +5, +8, +9, +10` |
| **6** | `+2, +3, +6, +7` | `+6, +11`; ✂ 5, 10 (both already have hub 5, and `5 ∈ L_out(6)`) |
| **2** | `+2, +3` | `+1, +2`; ✂ 4, 5, 6 |
| **4** | `+4`; ✂ 1, 5 (reach 4 via hub 5) | `+4, +8, +9` |
| **10** | `+10, +11`; ✂ 5, 6, 7 | `+8, +9, +10` |
| **1** | `+1`; ✂ 2 (hub 2) | `+1`; ✂ 4, 5 |
| **3** | `+3` | `+3, +7`; ✂ 2, 6, 11 |
| **7** | `+7`; ✂ 3 | `+7`; ✂ 6, 11 |
| **8** | `+8`; ✂ 4 (hub 4), 10 (hub 10) | `+8, +9` |
| **9** | `+9`; ✂ 4, 5, 8 | `+9` |
| **11** | `+11`; ✂ 6, 7 | `+11`; ✂ 10 |

**Final labels.**

| `v` | `L_in(v)` | `L_out(v)` |
| --- | --- | --- |
| 1 | {1, 2} | {1, 5} |
| 2 | {2} | {2, 5, 6} |
| 3 | {3} | {2, 3, 5, 6} |
| 4 | {4, 5} | {4} |
| 5 | {5} | {5} |
| 6 | {6} | {5, 6} |
| 7 | {3, 7} | {5, 6, 7} |
| 8 | {4, 5, 8, 10} | {8} |
| 9 | {4, 5, 8, 9, 10} | {9} |
| 10 | {5, 10} | {10} |
| 11 | {6, 11} | {10, 11} |
| **total** | **23 entries** | **21 entries** |

Index size 44 entries. Sanity checks: `3 ⇝ 9` since `L_out(3) ∩ L_in(9) = {5}`
(`3→2→5→9`); `1 ⇝̸ 11` since `L_out(1) ∩ L_in(11) = {1,5} ∩ {6,11} = ∅`, and indeed
everything reachable from 1 stays in `{4, 5, 8, 9, 10}`.

### (b) Comparing two spanning trees

**Algorithm.** Number the tree vertices by a **left-to-right post-order** traversal; each
vertex `v` gets the interval `I(v) = [min post-number in v's subtree, post(v)]`, which
covers exactly `v`'s tree descendants. Then, in **reverse topological order**, set
`L(u) = I(u) ∪ ⋃_{u→w ∈ E} L(w)` and delete any interval **contained** in another interval
of the same label (only containment is removed — adjacent or partially overlapping
intervals are kept, since merging them would create false reachability). Query:
`u ⇝ v ⟺ post(v)` lies in some interval of `L(u)`.

Both trees are rooted at vertex 1 (the only source of the DAG).

#### Tree 1

Post-order: `9, 5, 6, 2, 7, 3, 14, 12, 13, 10, 11, 8, 4, 1`.

| `v` | post | `I(v)` | final `L(v)` | size |
| --- | --- | --- | --- | --- |
| 1 | 14 | [1,14] | [1,14] | 1 |
| 2 | 4 | [1,4] | [1,4], [7,10] | 2 |
| 3 | 6 | [5,6] | [1,1], [3,3], [5,6], [7,12] | 4 |
| 4 | 13 | [7,13] | [7,13] | 1 |
| 5 | 2 | [1,2] | [1,2], [7,8] | 2 |
| 6 | 3 | [3,3] | [1,1], [3,3], [7,10] | 3 |
| 7 | 5 | [5,5] | [5,5], [7,10] | 2 |
| 8 | 12 | [7,12] | [7,12] | 1 |
| 9 | 1 | [1,1] | [1,1], [7,8] | 2 |
| 10 | 10 | [7,10] | [7,10] | 1 |
| 11 | 11 | [11,11] | [7,7], [9,9], [11,11] | 3 |
| 12 | 8 | [7,8] | [7,8] | 1 |
| 13 | 9 | [9,9] | [7,7], [9,9] | 2 |
| 14 | 7 | [7,7] | [7,7] | 1 |
| | | | **total** | **26** |

#### Tree 2

Post-order: `5, 2, 14, 12, 9, 6, 10, 7, 13, 11, 8, 3, 4, 1`.

| `v` | post | `I(v)` | final `L(v)` | size |
| --- | --- | --- | --- | --- |
| 1 | 14 | [1,14] | [1,14] | 1 |
| 2 | 2 | [1,2] | [1,2], [3,6], [7,7], [9,9] | 4 |
| 3 | 12 | [3,12] | [3,12] | 1 |
| 4 | 13 | [13,13] | [3,4], [7,7], [9,11], [13,13] | 4 |
| 5 | 1 | [1,1] | [1,1], [3,5] | 2 |
| 6 | 6 | [3,6] | [3,6], [7,7], [9,9] | 3 |
| 7 | 8 | [7,8] | [3,4], [7,8], [9,9] | 3 |
| 8 | 11 | [9,11] | [3,4], [7,7], [9,11] | 3 |
| 9 | 5 | [3,5] | [3,5] | 1 |
| 10 | 7 | [7,7] | [3,4], [7,7], [9,9] | 3 |
| 11 | 10 | [9,10] | [3,3], [9,10] | 2 |
| 12 | 4 | [3,4] | [3,4] | 1 |
| 13 | 9 | [9,9] | [3,3], [9,9] | 2 |
| 14 | 3 | [3,3] | [3,3] | 1 |
| | | | **total** | **31** |

#### Which tree is better?

> **Answer.** **Tree 1**, with **26** intervals against Tree 2's **31** — a 16% smaller
> index, and therefore less space and cheaper queries (a query scans `L(u)`).

| | Tree 1 | Tree 2 |
| --- | --- | --- |
| total intervals (index size) | **26** | 31 |
| largest label | 4 (vertex 3) | 4 (vertices 2, 4) |
| vertices with a single interval | 6 — 1, 4, 8, 10, 12, 14 | 5 — 1, 3, 9, 12, 14 |
| label-size distribution (1 / 2 / 3 / 4) | 6 / 5 / 2 / 1 | 5 / 3 / 4 / 2 |
| non-tree edges absorbed with no cost | 2 — `2→9`, `4→13` | 0 |

**Why.** A vertex's label starts as its own interval and grows by inheriting intervals
along non-tree edges. An edge `u→w` costs nothing exactly when `w` already lies in `u`'s
subtree, since then `L(w)`'s intervals are contained in `I(u)` and get deleted. So a
spanning tree is good when it keeps mutually reachable clusters inside one subtree.

- **Tree 1** places the whole "funnel" of the DAG — `4, 8, 10, 11, 12, 13, 14` — into a
  single subtree rooted at 4 (`4→8→{10,11}`, `10→{12,13}`, `12→14`). Every DAG edge among
  those vertices then stays inside that subtree, so `L(4)` collapses to the single
  interval `[7,13]` and `L(8) = [7,12]`, `L(10) = [7,10]` do too. The non-tree edges `2→9`
  and `4→13` point back into the subtree they start from and cost nothing at all.
- **Tree 2** splits the same funnel across three sibling branches under vertex 3
  (`6→9→12→14`, `7→10`, `8→11→13`) and leaves 4 as a **leaf**, so `I(4) = [13,13]` is a
  single point that can absorb nothing: `4→8` and `4→13` force `L(4)` up to 4 intervals.
  The cross-branch edges `6→10`, `8→10`, `10→12`, `10→13` and `13→14` are all non-tree
  edges between different branches, so their intervals propagate upward instead of being
  swallowed — this is why 6 of Tree 2's vertices need 3 or 4 intervals, against 3 in
  Tree 1.

Note the *maximum* label size is 4 for both trees, so the two are separated by the total
index size, not by the worst case.

---

## Q4. One-layer GNN mean aggregation

Graph: `A-B, A-C, B-C, C-D`, with
`h_A⁽⁰⁾ = (1,0)ᵀ`, `h_B⁽⁰⁾ = (0,1)ᵀ`, `h_C⁽⁰⁾ = (1,1)ᵀ`, `h_D⁽⁰⁾ = (2,0)ᵀ`, and

```
W = [ 1  -1   1   0 ]
    [ 0   1  -1   1 ]
```

### (a) Vertices `B` and `C`

**Vertex `B`.** `N(B) = {A, C}`:

```
h_N(B)⁽⁰⁾ = ½·[ (1,0)ᵀ + (1,1)ᵀ ] = (1, 0.5)ᵀ

CONCAT( h_B⁽⁰⁾, h_N(B)⁽⁰⁾ ) = (0, 1, 1, 0.5)ᵀ

W · (0,1,1,0.5)ᵀ = ( 1·0 + (−1)·1 + 1·1 + 0·0.5 ,  0·0 + 1·1 + (−1)·1 + 1·0.5 )ᵀ
                 = ( 0 , 0.5 )ᵀ

h_B⁽¹⁾ = ReLU( (0, 0.5)ᵀ ) = (0.00, 0.50)ᵀ
```

**Vertex `C`.** `N(C) = {A, B, D}`:

```
h_N(C)⁽⁰⁾ = ⅓·[ (1,0)ᵀ + (0,1)ᵀ + (2,0)ᵀ ] = (1, 1/3)ᵀ

CONCAT( h_C⁽⁰⁾, h_N(C)⁽⁰⁾ ) = (1, 1, 1, 1/3)ᵀ

W · (1,1,1,1/3)ᵀ = ( 1 − 1 + 1 + 0 ,  0 + 1 − 1 + 1/3 )ᵀ = ( 1 , 1/3 )ᵀ

h_C⁽¹⁾ = ReLU( (1, 1/3)ᵀ ) = (1.00, 0.33)ᵀ
```

| `v` | `N(v)` | neighbour mean | concatenated input | `h_v⁽¹⁾` |
| --- | --- | --- | --- | --- |
| `B` | `{A, C}` | `(1, 0.5)ᵀ` | `(0, 1, 1, 0.5)ᵀ` | **`(0.00, 0.50)ᵀ`** |
| `C` | `{A, B, D}` | `(1, 1/3)ᵀ` | `(1, 1, 1, 1/3)ᵀ` | **`(1.00, 0.33)ᵀ`** |

### (b) After adding the edge `B–D`

Only `N(B)` and `N(D)` change, and no initial embedding changes, so `A` and `C` receive
exactly the same input vector as before and cannot change. Because `ReLU` can hide a change
by clipping it to 0, we still compute all four vertices explicitly:

| `v` | `N(v)` before → after | `h_v⁽¹⁾` before | `h_v⁽¹⁾` after | changed? |
| --- | --- | --- | --- | --- |
| `A` | `{B,C}` → `{B,C}` | `(1.50, 0.50)ᵀ` | `(1.50, 0.50)ᵀ` | no |
| `B` | `{A,C}` → `{A,C,D}` | `(0.00, 0.50)ᵀ` | **`(0.33, 0.00)ᵀ`** | **yes** |
| `C` | `{A,B,D}` → `{A,B,D}` | `(1.00, 0.33)ᵀ` | `(1.00, 0.33)ᵀ` | no |
| `D` | `{C}` → `{B,C}` | `(3.00, 0.00)ᵀ` | **`(2.50, 0.50)ᵀ`** | **yes** |

The two recomputations:

```
B:  h_N(B)⁽⁰⁾ = ⅓·[ (1,0)ᵀ + (1,1)ᵀ + (2,0)ᵀ ] = (4/3, 1/3)ᵀ
    W · (0, 1, 4/3, 1/3)ᵀ = ( 0 − 1 + 4/3 + 0 , 0 + 1 − 4/3 + 1/3 )ᵀ = (1/3, 0)ᵀ
    h_B⁽¹⁾ = (0.33, 0.00)ᵀ          (was (0.00, 0.50)ᵀ)

D:  h_N(D)⁽⁰⁾ = ½·[ (0,1)ᵀ + (1,1)ᵀ ] = (0.5, 1)ᵀ
    W · (2, 0, 0.5, 1)ᵀ = ( 2 − 0 + 0.5 + 0 , 0 + 0 − 0.5 + 1 )ᵀ = (2.5, 0.5)ᵀ
    h_D⁽¹⁾ = (2.50, 0.50)ᵀ          (was (3.00, 0.00)ᵀ)
```

> **Answer (b).** The affected vertices are **`{B, D}`** — exactly the endpoints of the
> new edge. `A` and `C` are unchanged because a one-layer GNN only sees the 1-hop
> neighbourhood, and neither `N(A)` nor `N(C)` was touched.
