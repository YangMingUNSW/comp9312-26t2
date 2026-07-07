# COMP9312 Project — Q2: k-Triangle Core Index

**Student:** YUHUI LUO (z5565446)

## 1. Problem

Given a simple undirected graph `G`, a *triangle* is a set of three pairwise
adjacent vertices. A **k-triangle core** is a maximal connected induced subgraph
in which every vertex is contained in at least `k` triangles *inside that
subgraph*. Given `k` and a vertex `v`, `query(k, v)` returns all vertices of the
k-triangle core containing `v`. We must answer many such queries with an index
built once in `build()`.

## 2. Two ingredients

The k-triangle core is the **vertex analogue of the k-core**, with "degree"
replaced by "number of triangles a vertex lies in". Answering queries needs two
things:

1. **How deep is each vertex?** — the *triangle-core number* `tc(v)`, the largest
   `k` for which `v` belongs to some k-triangle core.
2. **Which vertices are together?** — the correct notion of connectivity.

### Why plain connectivity is wrong (the bridge)

In the spec's Figure 1 (a `K4` on `{0,1,2,3}`, a triangle `{3,4,5}`, a bridge
`5–6`, and a `K4` on `{6,7,8,9}`), `query(1,0)` returns `{0,1,2,3,4,5}` — **not**
`{0,…,9}`, even though the whole graph is connected and every vertex lies in a
triangle. The bridge edge `(5,6)` is in **no** triangle, so it must not connect
the two sides. Connectivity for triangle cores is **triangle connectivity**: two
vertices belong to the same k-core only if linked by a chain of triangles (each
sharing a vertex with the next) whose vertices all survive to level `k`.

We capture this with an **edge level**:

```
level(u, w) = max over triangles (u, w, x) of  min( tc(u), tc(w), tc(x) )
```

i.e. the highest `k` at which edge `(u,w)` sits inside a triangle whose three
vertices all have `tc ≥ k`. An edge in no triangle has level `0`.

Then:

```
query(k, v):  if tc(v) < k:  return []
              else:          return sorted{ vertices reachable from v using
                                            only edges with level ≥ k }
```

(The reachable set is collected in a hash set, so we `sorted()` it before
returning to give a canonical ascending vertex order that matches the spec's
example outputs.)

This reachable set is exactly the triangle-connected k-core component of `v`.

## 3. Computing tc(v): triangle-core peeling

`tc(v)` is obtained by **peeling** (a degeneracy process on triangle counts),
exactly analogous to k-core decomposition:

1. Count `t(v)` = number of triangles containing `v` in `G`.
2. Repeatedly remove the vertex `v` with the **smallest** current `t(v)`.
   Removing `v` destroys every triangle through it, so for each such triangle
   `(v,a,b)` decrement `t(a)` and `t(b)`.
3. `tc(v)` = the value of `t(v)` at the moment `v` is removed (kept monotone by
   the removal order, which is the "max with the running level" rule).

We implement this with the **Batagelj–Zaversnik** bucket structure so each
removal and each triangle destruction is `O(1)`, giving `O(n + T)` peeling where
`T` is the number of triangles.

### Triangle enumeration (once, cheaply)

Orient every edge from the lower-rank to the higher-rank endpoint, where
`rank = (degree, id)`. Each triangle is then discovered exactly once, from its
lowest-rank vertex, by intersecting forward-neighbour lists. Cost:
`sum over edges (u,w) of min(d(u), d(w)) = O(m^1.5)`.

## 4. Index and query

`build()` stores:
- `tc[v]` for every vertex — `O(n)`.
- `edge_level[]`, laid out **parallel to** `graph.indices` (the flat adjacency
  array), so a query is a plain threshold DFS — `O(m)`.

`query(k, v)` returns `[]` if `tc(v) < k`; otherwise it DFS/BFS from `v`, crossing
only edges with `edge_level ≥ k` (whose endpoints automatically have `tc ≥ k`),
and returns the visited set.

### Worked example (spec Figure 1)

Triangle-core numbers: `tc = 3` for `{0,1,2,3,6,7,8,9}` (each in a `K4`, giving
`C(3,2)=3` triangles) and `tc = 1` for `{4,5}` (each in the single triangle
`{3,4,5}`).

- `query(1,0)`: from `0`, all `K4` edges and edges `(3,4),(3,5),(4,5)` have level
  `1` (triangle `{3,4,5}` has level `min(3,1,1)=1`); the bridge `(5,6)` has level
  `0`. → `{0,1,2,3,4,5}`. ✓
- `query(3,3)`: edges `(3,4),(3,5)` have level `1 < 3`, so `4,5` are excluded;
  only the `K4` edges (level `3`) are crossed. → `{0,1,2,3}`. ✓
- `query(4,0)`: `tc(0)=3 < 4`. → `[]`. ✓

## 5. Correctness (sketch)

- **Peeling = tc.** Identical to k-core correctness with "triangle count" as the
  monotone quantity: removing the current minimum never lets a later-removed
  vertex have a smaller core number, so the removal value equals `tc(v)`.
- **Edge level captures triangle connectivity.** If a triangle `(a,b,c)` has
  `min(tc) ≥ k`, all three of its edges get level `≥ k`, so its vertices are
  mutually reachable at level `k`; conversely an edge only reaches level `k` via
  such a triangle. Chaining triangles that share a vertex therefore chains the
  DFS, so the reachable set is precisely the triangle-connected k-core component.
- Every vertex with `tc(v) ≥ k` lies in at least one triangle of level `≥ k`
  (that is what `tc(v) ≥ k` means), so it has an incident edge of level `≥ k` and
  is reached — the returned component always contains `v`.

## 6. Complexity

Let `n` = vertices, `m` = edges, `d(v)` = degree, `T` = number of triangles
(`T = O(m^1.5)`), and let `C` be the vertex set returned by a query.

### Index construction time — `O(m^1.5 + n)`
- Triangle listing: `sum over edges (u,w) of min(d(u),d(w)) = O(m^1.5)`.
- Peeling (Batagelj–Zaversnik): `O(n + T)`; each vertex removal and each triangle
  destruction is `O(1)` amortised.
- Edge levels: `O(T)` to aggregate + `O(n + m)` to scatter into the array.

Total: `O(m^1.5 + n + m + T) = O(m^1.5 + n)` since `T = O(m^1.5)`.

### Query time — `O( sum over x in C of d(x)  +  |C|·log|C| )`
A single DFS over the induced subgraph of the answer scans each returned vertex's
adjacency once (`sum over x in C of d(x)`); a final `sorted()` on the `|C|`
returned vertices adds `O(|C|·log|C|)` for a canonical order. Both terms are
**output-sensitive**; the traversal term dominates in a genuine triangle core
(every returned vertex has degree ≥ 2), so the query is effectively
output-linear and bounded above by `O(n + m)`.

### Space — `O(n + m)`
The persistent index is `tc[]` (`O(n)`) plus `edge_level[]` (`O(m)`), i.e.
`O(n + m)`. Construction additionally uses `O(n + T)` transient memory
(triangle list and per-vertex incidence), which can be released after `build()`.
