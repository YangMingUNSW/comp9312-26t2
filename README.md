<div align="center">

# COMP9312 · Data Analytics for Graphs
### 图数据分析 · UNSW 课程作业仓库

![UNSW](https://img.shields.io/badge/UNSW-COMP9312-FFD200?style=flat-square&labelColor=000000)
![Topic](https://img.shields.io/badge/Topic-Graph%20Analytics-4c8bf5?style=flat-square)
![LaTeX](https://img.shields.io/badge/LaTeX-Overleaf-008080?style=flat-square&logo=latex&logoColor=white)
![Markdown](https://img.shields.io/badge/Docs-Markdown-083fa1?style=flat-square&logo=markdown&logoColor=white)

</div>

> **EN** — Personal cross-platform workspace for all assignments and projects in **COMP9312 — Data Analytics for Graphs** (UNSW).
>
> **中文** — 这是 **COMP9312（图数据分析）** 的个人作业仓库,跨平台同步课程里所有的作业与项目。

---

## 📖 Overview · 简介

This repository collects coursework for COMP9312. Each deliverable lives in its
own top-level folder; written solutions are kept both as readable Markdown and as
LaTeX source ready for Overleaf.

本仓库汇总 COMP9312 的全部课程产出。每个作业 / 项目各占一个顶层文件夹;每份书面解答
同时提供「可读的 Markdown 版」和「可直接在 Overleaf 编译的 LaTeX 版」。

## 📂 Structure · 目录结构

| Folder · 文件夹 | Contents · 内容 |
| --- | --- |
| [`assignment1/`](assignment1/) | **Assignment 1** — disjoint sets, BFS, graph representations, DFS order, DAG longest path.<br>**作业一** — 并查集、BFS、图的表示、DFS 序、DAG 最长路径。<br>题面 `ass1.md`,解答 `ass1_solution.md`,提交版 `ass1_z5565446.tex` / `.pdf`。 |
| [`project/`](project/) | **Project** — Q1 first cycle-causing edge in an insertion stream (union-find); Q2 index-based k-triangle core search (triangle-core peeling + edge-level DFS).<br>**项目** — Q1 边插入流中的首个成环边（并查集）；Q2 基于索引的 k-triangle core 查询（三角核剥离 + 边层级 DFS）。<br>提交版 `Q1.ipynb`/`Q1.tex`、`Q2.ipynb`/`Q2.tex`,解答 `Q*_solution.md`,题面 `spec.md`。 |
| [`claude-memory/`](claude-memory/) | Claude Code 跨机记忆镜像(见下方说明)。 |

> 后续作业 / 项目会以新的顶层文件夹形式加入(如 `assignment2/`)。

## 🧠 `claude-memory/`

A mirror of Claude Code's persistent memory for this project, committed to the repo
so prior context follows you across machines.

这是 Claude Code 针对本项目的持久记忆镜像,随仓库一起提交,让 AI 的历史上下文能跨机器
跟随你。在新机器上 clone 后,把这些文件复制到 Claude Code 的项目记忆目录
(`~/.claude/projects/<project-key>/memory/`,`project-key` 由该机器上的项目绝对路径推导)。

## 📝 File Naming Convention · 文件命名约定

For every assignment, each solution comes in two forms ——
每个作业的解答都提供两种形式:

- **`*_solution.md`** — readable Markdown solution · 可读的 Markdown 解答。
- **`<submission>.tex`** — the same content as **LaTeX source**, named to match the
  submission PDF (e.g. `ass1_z5565446.tex`); upload to Overleaf and compile to get
  the PDF for submission · 同样内容的 **LaTeX 源文件**,文件名与提交 PDF 对应,上传
  Overleaf 编译即得提交用 PDF。
