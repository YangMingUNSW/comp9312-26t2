---
name: readability-first
description: Optimise for readability/correctness of the final Overleaf-rendered PDF; the user iterates the render, you don't compile
metadata:
  node_type: memory
  type: feedback
---

For COMP coursework solutions, optimise for the **readability and correctness of the final submitted PDF**, which a human tutor grades. Favour clear, professional presentation (e.g. graphical `forest` trees over ASCII art when it reads better) even when the LaTeX is more involved.

**Why:** the user compiles the `.tex` on their own Overleaf and does multiple rendering passes before submitting, so polished-but-uncompiled LaTeX is fine — local compile-risk is NOT a reason to hold back or pick a plainer option. The deliverable is a human-graded PDF, not the source.

**How to apply:** don't default to the lowest-risk ASCII/plain option just to avoid Overleaf round-trips; choose the most readable presentation and let the user iterate the render. Still keep redundant textual cross-checks (e.g. `Components: {...}` lines beside tree diagrams) so a grader can't misread a figure. See [[solution-file-convention]].
