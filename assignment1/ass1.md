# COMP9312 — Assignment 1

**Topics:** Disjoint Sets, BFS Traversal, Graph Representation, DFS Order, and DAG Algorithms

---

## Notation & Conventions

- `n` = number of vertices, `m` = number of edges. Do **not** assume `n < m` or `m < n`.
- All time complexities must be reported in the **tightest and simplest** form
(e.g. if `a < b`, writing `O(a + b)` instead of `O(b)` may lose marks).
- Coordinates in grid problems are given as `(row, column)`, with `(1,1)` at the top-left.

---

# Q1. Disjoint-set forests for connected components

## Problem Statement

Given a vertex set `V` and an ordered list of edges:

```
V = {A, B, C, D, E, F, G, H}
```

Edge order:

1. B--C
2. A--C
3. D--E
4. C--E
5. F--G
6. E--G
7. H--F
8. G--H

## Requirements

Process edges in the order one by one using the disjoint-set data structure to
maintain connected components. For each edge, draw the resulting
disjoint-set tree/forest structure **after** processing the edge.

**Tie-breaking rule (union by size):** Given two trees with the same size
(number of vertices), the tree with a **lexicographically smaller root** is
treated as having the larger size (i.e. its root becomes the new root).

---

# Q2. BFS on a grid map

## Problem Statement

In a grid map, `S` is the start cell. `.` denotes a passable cell, `#`
denotes a blocked cell. Coordinates are `(row, column)` (e.g., the top-left cell indexed as (1,1)). Moves  in the grid are either horizontal or vertical.

## Requirements

Consider the following **6 × 6** grid:

```
       c1 c2 c3 c4 c5 c6
   r1:  S  .  .  #  .  .
   r2:  #  #  .  #  #  #
   r3:  .  .  .  .  .  .
   r4:  .  #  #  #  .  #
   r5:  .  .  .  #  .  .
   r6:  #  #  .  .  .  .
```

### (a)

The grid can be regarded as a graph, where all passable cells are vertices and adjacent passable cells of each passable cells are neighbours. Assume the neighbours of each cell is fixed as specified below:
**Right → Down → Left → Up**.

Run BFS (implemented by a FIFO queue) from `S` until completion, visiting all
reachable passable cells. Show on each passable cell with its visited time. The visited time of a vertex in BFS is the time when the vertex is dequeued. The time starts from 0 and increases 1 after each dequeuing.

### (b)

Assume the order to scan neighbours for each cell is not fixed. Consider the queries below. Each query contains two cells, x and y. A query returns true if, for all possible BFS processing orders, x is processed before y; otherwise, it returns false. For each query, both cells are reachable from S.


| Query | x     | y     |
| ----- | ----- | ----- |
| 1     | (1,2) | (3,2) |
| 2     | (3,4) | (3,2) |
| 3     | (5,1) | (3,6) |
| 4     | (4,5) | (5,6) |
| 5     | (6,6) | (5,3) |
| 6     | (3,1) | (5,5) |
| 7     | (5,6) | (4,5) |
| 8     | (6,4) | (6,3) |


## Marking

- 4 marks for correct visited times in (a).
- 2 marks for correct query results in (b).

---

# Q3. Graph representation and algorithm time complexity (6 marks)

## Problem Statement

Let `G = (V, E)` be a simple, undirected, unweighted graph with `|V| = n` and
`|E| = m`. Let `deg(u)` be the degree of a vertex  `u`.

Consider the following six data structures used to represent `G`:

1. **Adjacency matrix** — a Boolean `n × n` matrix.
2. **Adjacency arrays** — for each vertex, store its neighbours in an array.
3. **Sorted adjacency arrays** — for each vertex, store its neighbours in an
  array sorted by increasing vertex ID.
4. **Hash-set adjacency lists** — for each vertex, store its neighbours in a
  hash set.
5. **Compressed Sparse Row (CSR)** — store the graph using a static offset
  array and one neighbour array.
6. **Adjacency balanced BSTs** — for each vertex, store its neighbours in a
  balanced binary search tree.

## Requirements

Analyse the time complexity of the following problems for each data structure above. Show the time complexity by any input values among n, m, deg(u) and deg(v).

- `SingleSourceShortestDistances(u)` — return the shortest-path distance from
`u` to every reachable vertex; unreachable vertices have distance `INF`.
- `CountConnectedComponents()` — return the number of connected components in
`G`.
- `CountCommonNeighbours(u, v)` — return the number of vertices adjacent to
both `u` and `v`.
- `DFS(u)` — run DFS from `u` and visit all vertices reachable from `u`.

## Marking

Marking for Q3: 1.5 marks for the correct time complexity of SingleSourceShortestDistances(u), 1.5 marks for the correct time complexity of CountConnectedComponents(), 1.5 marks the for correct time complexity of CountCommonNeighbours(u,v), 1.5 marks for the correct time complexity of DFS(u).

---

# Q4. Forced order in all possible DFS traversals (6 marks)

## Problem Statement

Let `G = (V, E)` be a connected, undirected graph stored using adjacency
lists, with `n` vertices and `m` edges. The input also contains three
distinct vertices `s`, `u`, and `v`.

Running a DFS from `s` may produce different visit orders depending on each
vertex's neighbour order. A vertex is **visited** when first discovered.

## Requirements

### (a) Pseudocode

A procedure MustVisitBeforeDFS(G, s, u, v) returns true if and only if u is visited before v in every valid DFS from s. Write pseudocode to implement MustVisitBeforeDFS(G, s, u, v).
Below is an output example of MustVisitBeforeDFS(G, s, u, v).

**Reference output (small example):**

```
Input graph:
Vertices: A, B, C, D, E
Undirected edges: A--B, B--C, B--D, D--E

Input query 1:
MustVisitBeforeDFS(graph, A, B, E)
Output: true

Input query 2:
MustVisitBeforeDFS(graph, A, C, D)
Output: false

Input query 3:
MustVisitBeforeDFS(graph, A, E, D)
Output: false
```

### (b) Time complexity

Analyse the time complexity of your implementation in terms of n (number of vertices) and m (number of edges).

### (c) Worked queries

For each query below, output true or false.

```
Vertices: A, B, C, D, E, F, G, H, I

Undirected edges:
A--B, A--C,
B--D, B--E,
D--F, E--F,
C--G, G--H, H--I, C--I

Queries:
1. MustVisitBeforeDFS(graph, A, C, H)
2. MustVisitBeforeDFS(graph, A, D, E)
3. MustVisitBeforeDFS(graph, A, B, F)
4. MustVisitBeforeDFS(graph, A, H, G)
```

## Marking

- 3 marks for the pseudocode.
- 1 mark for the time complexity.
- 2 marks for the four query answers.

---

# Q5. Longest path in a weighted DAG (8 marks)

## Problem Statement

Let `G = (V, E)` be a weighted DAG with `n` vertices and `m` edges. `G` is
stored in an array `Adj` indexed by vertices. For each vertex `u`, `Adj[u]`
is a list of pairs `(v, w)` representing outgoing edges `u -> v` with edge
weight `w`.

## Requirements

### (a) Pseudocode

Write `LongestPathDAG(G)` that returns the longest directed path in `G` and
its length.  If several paths have the same longest length, return any one. A one-vertex path has length 0. `0`.

**Reference example for (a):**

```
Vertices: A, B, C, D, E, F
Outgoing adjacency lists:
  A: (B,2), (C,4)
  B: (D,3)
  C: (D,1), (E,5)
  D: (F,4)
  E: (F,2)
  F: []

One correct output:
  Longest path length: 11
  One longest path:    A -> C -> E -> F
```

### (b) Time complexity

Analyse the time complexity of your implementation in terms of `n` and `m`.

### (c) Worked example

For the DAG below, report the longest path length and the longest path.

```
Vertices: A, B, C, D, E, F, G, H
Outgoing adjacency lists:
  A: (B,3), (C,2)
  B: (D,4), (E,6)
  C: (E,5), (F,4)
  D: (G,2)
  E: (G,1), (H,7)
  F: (H,3)
  G: (H,2)
  H: []
```

## Marking

- 5 marks for the pseudocode.
- 1 mark for the time complexity.
- 2 marks for the worked example.

