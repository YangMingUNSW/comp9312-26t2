---
name: solution-file-convention
description: Two-file output convention for every COMP9312 assignment solution
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 69a1bd3a-0476-41f9-bc4f-3c5d44a26c87
---

For every COMP9312 assignment, produce the solution in TWO files (do not delete the readable one):

- `<name>_solution.md` — readable Markdown solution.
- `<submission>.tex` — the SAME content as a full LaTeX document (`\documentclass … \end{document}`), as a real `.tex` file named to match the submission PDF (e.g. `ass1_z5565446.tex`), so the user can upload it to Overleaf and compile to PDF for submission.

**Why:** the user submits PDFs built in Overleaf; using a true `.tex` file (not a `.md` that secretly holds LaTeX) removes the extension confusion and uploads cleanly. The `.tex` is the single source for the PDF — edit it to fix the PDF, then regenerate in Overleaf. (Older assignments used a `<name>_solution_pdf.md`; that pattern is retired.)

**How to apply:** target Overleaf's default pdfLaTeX — use ASCII art inside `verbatim` (no Unicode box-drawing or `∞`), math mode for all complexities, and `\resizebox{\textwidth}{!}{…}` for wide tables. See [[comp9312-repo]].
