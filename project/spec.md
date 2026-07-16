# COMP9312 26T2 Project

# Project

Fix & Update (updates for the project are listed here):

## Summary


| Required Files | `Q1.ipynb`,`Q1.pdf`,`Q2.ipynb`,`Q2.pdf`.                            |
| -------------- | ------------------------------------------------------------------- |
| Submission     | Submit your files on Moodle (Only the last submission will be used) |
| Deadline       | 9pm Friday 24 July (Sydney Time)                                    |
| Marks          | 25 marks (25% toward your total mark for this course)               |


##### Late penalty

5% of the max accessment mark will be deducted for each additional day (24hr) after the specified submission time and date. No submission is accepted 5 days (120hr) after the deadline.

## Question Entries

Q1 (10 marks)

Q2 (15 marks)

## How to Work on the Project

For each question, a Google Colab notebook link will be provided. You may either download the notebook and work on it locally, or make your own copy and work directly in Google Colab.

##### Option 1: Work locally

1. Open the provided Colab notebook link for the question.
2. Go to File > Download > Download .ipynb.
3. Open the downloaded notebook in your local environment, such as Jupyter Notebook, JupyterLab, or VS Code.
4. Write your code in the required cells and run the notebook locally.
5. Before submission, restart the kernel/session and run all cells from the beginning to make sure the notebook can execute without errors.



##### Option 2: Work in Google Colab

1. Open the provided Colab notebook link for the question.
2. Go to File > Save a copy in Drive. This creates your own editable copy of the notebook.
3. Work only on your own copied notebook, not on the original template.
4. Write your code in the required cells and run the notebook in Colab.
5. When you finish, go to File > Download > Download .ipynb to download your final notebook for submission.



##### How to test your code

1. Run all cells in order from top to bottom. Do not rely on results from cells run out of order.
2. Use any provided sample tests or testing cells in the notebook to check your implementation.
3. For Colab, use Runtime > Restart session and run all. For a local notebook, restart the kernel and run all cells again.
4. Check that the notebook finishes without exceptions and that all required outputs are generated before you export or submit your files.



## Submission Guide

1. Download your completed notebook. In Colab, go to File > Download > Download .ipynb. If you worked locally, save the final notebook from your local environment.
2. Rename your notebooks as`Q1.ipynb` and`Q2.ipynb`, and export the corresponding PDF files as`Q1.pdf` and`Q2.pdf`.
3. Upload`Q1.ipynb`,`Q1.pdf`,`Q2.ipynb` and`Q2.pdf` to Moodle.

---



# Q1 (10 marks)

Design a solution to find the first cycle-causing edge in an undirected graph insertion stream.

## Problem Statement

Given a set of isolated vertices`V = {0, 1, 2, ..., n-1}` and a sequence of undirected and unwighted edges, we insert edges in the sequence one by one. Design an algorithm to compute the first edge that creates a cycle in the graph as well as the cycle.

## Background & Query Output

In the graph below, the labels on the edges show the insertion order. The sixth inserted edge`5--0` is the first edge that creates a cycle.

0 1 2 3 4 5 1 2 4 3 5 6 First cycle: 5 - 4 - 3 - 2 - 1 - 0 - 5

Figure reference: Q1 figure (`assets/Q1_figure.png`).

### Machine-Readable Graph Spec (YAML)

This YAML describes the Q1 figure above.

```yaml
graph_id: q1_first_cycle_edge_example
graph_type: undirected_unweighted
nodes: [0, 1, 2, 3, 4, 5]

# Edge insertion stream in order
edge_stream:
  - order: 1
    edge: [0, 1]
  - order: 2
    edge: [1, 2]
  - order: 3
    edge: [3, 4]
  - order: 4
    edge: [2, 3]
  - order: 5
    edge: [4, 5]
  - order: 6
    edge: [5, 0]
```



##### Example 1 (the above figure): a cycle exists.

Input:

```
n = 6
L = [
    (0, 1),
    (1, 2),
    (3, 4),
    (2, 3),
    (4, 5),
    (5, 0)
]
```

Return value:

```
[[5, 0], [5, 4, 3, 2, 1, 0, 5]]
```



##### Example 2: no edge creates a cycle.

Input:

```
n = 5
L = [
    (0, 1),
    (1, 2),
    (3, 4)
]
```

Return value:

```
None
```



## Doing this Project

Open the code template file Q1.ipynb and make a copy in your own Google Drive. You need to implement a function`query(n, L)` in the`FirstCycleEdgeQuery` class. The first parameter`n` is the number of vertices, and the second parameter`L` is a list of edges. Each edge is a tuple of two integers representing the endpoints of the edge. The function should return the first cycle-causing edge and a cycle containing it, or`None` if no cycle is created by any edge. You can assume following properties always hold for the input:

- For any vertex`v` appearing in the edge list,`0 ≤ v < n`.
- There is no repeated edge in the edge list.

In addition to the implementation, write a supporting document named as`Q1.pdf`. The document must includes theroretical analysis of your algorithm including the time complexity of the query algorithm, and the space complexity of the data structure you need for query processing. The document should also include an explanation of your algorithm design. Diagrams/figures/examples are are encouraged to illustrate your ideas.

For complexity analysis, you should use`n` for the number of vertices or/and`m` for the number of inserted edges. Any standard mathematical notations are accepted including but not limited to`sum`,`+`,`-`,`*`,`/`,`min`,`max`, and`^`.

## Required Files

Submit`Q1.ipynb` and`Q1.pdf` on Moodle.

## Marking Criteria

Marks will be awarded based on:

- Code correctness and efficiency (with clear explanation and/or comments): 8 marks
- Time complexity analysis: 1 mark
- Space complexity analysis: 1 mark



## Notes:

1. Do not change the input/output format of the provided class or method.
2. Submissions can be tested on larger graphs than those shown in the example.
3. Your explanation should include your algorithm, efficiency discussion, time complexity, and space complexity.
4. You cannot import any external libraries or modules other than the Python Standard Library.

---



# Q2 (15 marks)

Design an index-based solution for k-triangle core queries.

## Problem Statement:

Given a simple, undirected graph`G`, a triangle is a set of three vertices that are pairwise connected. A`k`-triangle core is a maximal (connected) induced subgraph of`G` in which every vertex is contained in at least`k` triangles in the subgraph. Given an integer`k` and a vertex`v`, the problem of`k`-triangle core search aims to find all vertices in the`k`-triangle core containing`v`.

Design an index-based solution for`k`-triangle core search.

## Background & Query Output

Figure 1 shows a connected graph.

Figure reference: Q2 figure (`assets/Q2_figure.png`).

### Machine-Readable Graph Spec (YAML)

This YAML describes the Q2 figure above.

```yaml
graph_id: q2_k_triangle_core_figure1
graph_type: undirected_unweighted
nodes: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

edges:
  # K4 on {0,1,2,3}
  - [0, 1]
  - [0, 2]
  - [0, 3]
  - [1, 2]
  - [1, 3]
  - [2, 3]

  # triangle {3,4,5}
  - [3, 4]
  - [4, 5]
  - [3, 5]

  # bridge
  - [5, 6]

  # K4 on {6,7,8,9}
  - [6, 7]
  - [6, 8]
  - [6, 9]
  - [7, 8]
  - [7, 9]
  - [8, 9]

vertex_triangle_core_number:
  0: 3
  1: 3
  2: 3
  3: 3
  4: 1
  5: 1
  6: 3
  7: 3
  8: 3
  9: 3
```

Example query outputs (the first parameter of the query is the integer k and the second parameter is the vertex ID):

```
query(1, 0) returns [0, 1, 2, 3, 4, 5]
query(1, 6) returns [6, 7, 8, 9]
query(3, 3) returns [0, 1, 2, 3]
query(4, 0) returns []
```



## Doing this Project

Open the code template file Q2.ipynb. The notebook already defines the graph structure, the code template, and local tests. You only need to implement`KTriangleIndex.build()` and`KTriangleIndex.query(k, v)` in the`KTriangleIndex` cell. The`build()` method is for preprocessing the input graph and storing data structures for answering queries. The`query(k, v)` method answers the query using the preprocessed data structures. Add helper methods or classes as needed.

The local tests generate graphs using fixed seeds and compare your outputs with precomputed expected results. The largest local test has 10,000 vertices and 80,000 edges.

In addition to the implementation, write a supporting document named`Q2.pdf`. The document must include theoretical analysis of your algorithm, including the time complexity of the query algorithm, the time complexity of the index construction algorithm, and the space complexity of the data structure needed for query processing. The document should also include an explanation of your algorithm and index design. Diagrams, figures, and examples are encouraged to illustrate your ideas.

For complexity analysis, use the following notations:`n` for the number of vertices,`m` for the number of edges,`d(v)` for the degree of vertex`v`,`max_deg` for the maximum degree,`min_deg` for the minimum degree, and`avg_deg` for the average degree`2m/n`. Use any standard mathematical notations including but not limited to:`sum`,`+`,`-`,`*`,`/`,`min`,`max`, and`^`.

## Required Files

Submit`Q2.ipynb` and`Q2.pdf` on Moodle.

## Marking Criteria

Marks will be awarded based on:

1. Correctness of the algorithm: 4 marks
2. Efficiency of the algorithm: 8 marks
3. Time and space complexity analysis: 3 marks



## Notes:

1. Do not change the input/output format of the provided class or method.
2. Submissions can be tested on larger graphs than those shown in the example.
3. Your explanation should include your algorithm, index design, efficiency discussion, time complexity, and space complexity.
4. You cannot import any external libraries or modules other than the Python Standard Library.

---



# COMP9312 26T2 Project - 中文版



# 项目说明

更新与修正（项目更新会列在这里）：

## 摘要


| 要提交的文件 | `Q1.ipynb`,`Q1.pdf`,`Q2.ipynb`,`Q2.pdf` |
| ------ | --------------------------------------- |
| 提交方式   | 在 Moodle 提交（只会采用最后一次提交）                 |
| 截止时间   | 7 月 24 日（周五）晚 9 点（悉尼时间）                 |
| 分值     | 25 分（占本课程总评 25%）                        |




##### 迟交扣分

超过截止时间后，每增加一天（24 小时）扣总评满分的 5%。截止后 5 天（120 小时）后不再接受提交。

Q1（10 分）

Q2（15 分）

## 如何完成本项目

每道题会提供一个 Google Colab notebook 链接。你可以下载到本地完成，也可以在 Google Colab 中复制后在线完成。

##### 方式 1：本地完成

1. 打开该题提供的 Colab 链接。
2. 在菜单选择 File > Download > Download .ipynb。
3. 在本地环境打开下载的 notebook，例如 Jupyter Notebook、JupyterLab 或 VS Code。
4. 在要求的单元格中编写并运行代码。
5. 提交前请重启内核/会话并从头运行全部单元，确保 notebook 可无报错执行。



##### 方式 2：在 Google Colab 完成

1. 打开该题提供的 Colab 链接。
2. 在菜单选择 File > Save a copy in Drive，创建你自己的可编辑副本。
3. 只在你自己的副本中工作，不要修改原模板。
4. 在 Colab 中完成并运行代码。
5. 完成后通过 File > Download > Download .ipynb 下载最终版本用于提交。



##### 如何测试代码

1. 按顺序从上到下运行全部单元，不要依赖乱序执行产生的中间结果。
2. 使用 notebook 中提供的样例测试或测试单元验证实现。
3. 在 Colab 用 Runtime > Restart session and run all；在本地请重启内核后运行全部单元。
4. 提交前确认 notebook 能完整运行、无异常，并输出所有要求结果。



## 提交指南

1. 下载完成后的 notebook。Colab 中为 File > Download > Download .ipynb；本地完成则保存最终 notebook 文件。
2. 将 notebook 重命名为`Q1.ipynb`和`Q2.ipynb`，并导出对应 PDF：`Q1.pdf`和`Q2.pdf`。
3. 将`Q1.ipynb`,`Q1.pdf`,`Q2.ipynb`,`Q2.pdf`上传到 Moodle。

---



# Q1（10 分）

设计一个算法，在无向图边插入流中找出第一条导致成环的边，并给出该环。

## 题目描述

给定一组初始孤立点`V = {0, 1, 2, ..., n-1}`，以及一个无向、无权边序列。按顺序逐条插入这些边。请设计算法，找出第一条使图中出现环的边，以及对应的一个环。

## 背景与查询输出

下图中，边上的标签表示插入顺序。第六条插入的边`5--0`是第一条导致成环的边。

0 1 2 3 4 5 1 2 4 3 5 6 First cycle: 5 - 4 - 3 - 2 - 1 - 0 - 5

##### 示例 1（对应上图）：存在环

输入：

```text
n = 6
L = [
    (0, 1),
    (1, 2),
    (3, 4),
    (2, 3),
    (4, 5),
    (5, 0)
]
```

返回值：

```text
[[5, 0], [5, 4, 3, 2, 1, 0, 5]]
```



##### 示例 2：没有任何边导致成环

输入：

```text
n = 5
L = [
    (0, 1),
    (1, 2),
    (3, 4)
]
```

返回值：

```text
None
```



## 做题说明

打开代码模板文件 `Q1.ipynb`，并在你自己的 Google Drive 中复制一份。你需要在 `FirstCycleEdgeQuery` 类中实现函数 `query(n, L)`。第一个参数 `n` 是顶点数量，第二个参数 `L` 是边列表，每条边是由两个整数组成的端点元组。函数应返回第一条导致成环的边以及一个包含该边的环；若无任何边导致成环，则返回 `None`。可以假设输入始终满足：

- 边列表中出现的任意顶点 `v` 都满足 `0 ≤ v < n`；
- 边列表中没有重复边。

除代码实现外，还需提交一份说明文档 `Q1.pdf`。文档必须包含算法的理论分析，包括查询算法时间复杂度、为查询处理所需数据结构的空间复杂度，以及算法设计说明。鼓励使用图示/示例辅助说明。

复杂度分析中，应使用 `n` 表示顶点数、`m` 表示已插入边数（可单独或同时使用）。可使用任意标准数学记号，包括但不限于 `sum`、`+`、`-`、`*`、`/`、`min`、`max`、`^`。

## 需提交文件

在 Moodle 提交 `Q1.ipynb` 和 `Q1.pdf`。

## 评分标准

评分依据：

- 代码正确性与效率（配有清晰解释和/或注释）：8 分
- 时间复杂度分析：1 分
- 空间复杂度分析：1 分



## 注意事项

1. 不要修改给定类和方法的输入/输出格式。
2. 评测时可能使用比示例更大的图。
3. 说明文档应包含算法、效率讨论、时间复杂度与空间复杂度。
4. 除 Python 标准库外，不可导入任何外部库或模块。

---



# Q2（15 分）

设计一个基于索引的 `k`-triangle core 查询方案。

## 题目描述

给定一个简单无向图 `G`。三角形是三个两两相连顶点组成的集合。`k`-triangle core 是 `G` 的一个极大（连通）诱导子图，且子图中每个顶点都至少属于 `k` 个三角形。给定整数 `k` 和顶点 `v`，`k`-triangle core search 的目标是找出包含 `v` 的 `k`-triangle core 中的所有顶点。

请设计一个面向 `k`-triangle core search 的索引化方案。

## 背景与查询输出

图 1 展示了一个连通图。

0 1 2 3 4 5 6 7 8 9 kt=3 kt=3 kt=3 kt=3 kt=1 kt=1 kt=3 kt=3 kt=3 kt=3 K4 on {0,1,2,3} triangle {3,4,5} and bridge 5--6 K4 on {6,7,8,9} Figure 1.

示例查询输出（查询第一个参数为整数 `k`，第二个参数为顶点 ID）：

```text
query(1, 0) returns [0, 1, 2, 3, 4, 5]
query(1, 6) returns [6, 7, 8, 9]
query(3, 3) returns [0, 1, 2, 3]
query(4, 0) returns []
```



## 做题说明

打开代码模板文件 `Q2.ipynb`。该 notebook 已给出图结构、代码模板与本地测试。你只需在 `KTriangleIndex` 单元格中实现 `KTriangleIndex.build()` 与 `KTriangleIndex.query(k, v)`。`build()` 用于对输入图做预处理并构建用于查询的数据结构；`query(k, v)` 使用预处理结果回答查询。你可以按需添加辅助方法或辅助类。

本地测试会使用固定随机种子生成图，并将你的输出与预计算标准答案比较。最大本地测试规模为 10,000 个顶点和 80,000 条边。

除代码实现外，还需提交 `Q2.pdf` 说明文档。文档必须包含理论分析：查询算法时间复杂度、索引构建算法时间复杂度、查询数据结构空间复杂度；还需包含算法与索引设计说明。鼓励使用图示、示例来说明思路。

复杂度分析中，使用如下记号：`n` 表示顶点数，`m` 表示边数，`d(v)` 表示顶点 `v` 的度，`max_deg` 表示最大度，`min_deg` 表示最小度，`avg_deg` 表示平均度 `2m/n`。可使用任意标准数学记号，包括但不限于 `sum`、`+`、`-`、`*`、`/`、`min`、`max`、`^`。

## 需提交文件

在 Moodle 提交 `Q2.ipynb` 和 `Q2.pdf`。

## 评分标准

评分依据：

1. 算法正确性：4 分
2. 算法效率：8 分
3. 时间与空间复杂度分析：3 分



## 注意事项

1. 不要修改给定类和方法的输入/输出格式。
2. 评测时可能使用比示例更大的图。
3. 说明文档应包含算法、索引设计、效率讨论、时间复杂度与空间复杂度。
4. 除 Python 标准库外，不可导入任何外部库或模块。

