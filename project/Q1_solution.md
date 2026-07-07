# COMP9312 Project — Q1: First Cycle-Causing Edge

**Student:** YUHUI LUO (z5565446)

## 1. Problem

We start from `n` isolated vertices `V = {0, …, n−1}` and insert an
*undirected, unweighted* edge stream `L = [(u₁,v₁), (u₂,v₂), …]` one edge at a
time. We must return the **first** inserted edge that closes a cycle, together
with **one** cycle that contains it, or `None` if the whole stream stays acyclic.

## 2. Key observation

Before any cycle appears, the accepted edges form a **forest** (an acyclic graph):
every edge we keep connects two vertices that were previously in *different*
trees, merging them. An edge `(u, v)` creates a cycle **iff** `u` and `v` are
already in the **same** tree, because a tree already contains a unique path
between them and the new edge closes that path into a cycle.

So the task reduces to *maintaining connected components under edge insertions*
and testing, for each new edge, whether its endpoints are already connected.
This is exactly what a **Disjoint Set Union (DSU / union–find)** supports.

## 3. Algorithm

**Data structures**
- `parent[]`, `rank[]` — DSU with *path compression* + *union by rank*.
- `forest[]` — adjacency lists of the accepted (non-cycle) edges only, i.e. the
  spanning forest. It never holds more than `n − 1` edges.

**Processing the stream.** For each edge `(u, v)`:
1. If `find(u) == find(v)` → `u, v` are already connected. `(u, v)` is the first
   cycle-causing edge. Recover the cycle and return.
2. Otherwise accept the edge: add it to `forest`, `union(u, v)`, continue.

If the loop finishes without a collision, return `None`.

**Recovering the cycle.** When the collision edge `(u, v)` is found, `u` and `v`
sit in the *same tree of the spanning forest*. A single **BFS from `u`** inside
`forest` finds the unique tree path `u → … → v`; appending `u` closes the cycle.
The returned value is `[[u, v], [u, …, v, u]]`.

### Worked example (spec Example 1)

`n = 6`, `L = [(0,1),(1,2),(3,4),(2,3),(4,5),(5,0)]`.

The first five edges are all accepted and build the path
`0—1—2—3—4—5` (one tree). The sixth edge `(5, 0)` finds `find(5) == find(0)`
→ first cycle edge. BFS in the forest from `5` reaches `0` along
`5—4—3—2—1—0`; closing with `5` gives the cycle
`[5, 4, 3, 2, 1, 0, 5]`. Output: `[[5, 0], [5, 4, 3, 2, 1, 0, 5]]`. ✓

## 4. Correctness

- **First cycle detection.** By induction the accepted edges are always a forest,
  so components = trees. `find(u) == find(v)` is true exactly when a path already
  exists between `u` and `v`, i.e. exactly when `(u, v)` would form a cycle. We
  test edges in stream order and return on the first such edge, so it is *the*
  first cycle-causing edge.
- **Valid cycle.** At the moment of the collision the forest is unchanged by
  `(u, v)`, so the BFS path `u → … → v` uses only real, previously-inserted edges;
  together with `(u, v)` it is a genuine simple cycle containing the new edge.

## 5. Complexity

Let `n` = number of vertices, `m = |L|` = number of inserted edges.

### Time — `O(m · α(n) + n)`
- Each edge triggers two `find`s and at most one `union`; with path compression +
  union by rank each costs amortised `O(α(n))` (inverse-Ackermann, ≈ constant).
  Over the stream this is `O(m · α(n))`.
- The cycle-recovery BFS runs **once**, over a forest of at most `n − 1` edges,
  so it costs `O(n)`.

Total: **`O(m · α(n) + n)`**, i.e. effectively linear in the input.

### Space — `O(n)`
- `parent[]`, `rank[]`: `O(n)`.
- `forest[]`: it stores only accepted edges, and a forest on `n` vertices has at
  most `n − 1` edges → `O(n)` (independent of `m`).
- BFS `prev` map and queue: `O(n)`.

Total: **`O(n)`**. Notably the working memory is `O(n)`, not `O(n + m)`: we never
need to store the whole edge set, only the growing spanning forest.
