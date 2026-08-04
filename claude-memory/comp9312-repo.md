---
name: comp9312-repo
description: The COMP9312 coursework GitHub repo used as a cross-platform store
metadata: 
  node_type: memory
  type: project
  originSessionId: 69a1bd3a-0476-41f9-bc4f-3c5d44a26c87
---

`C:\Users\A\Desktop\UNSW\9312` is a git repo pushed to the PRIVATE GitHub repo `YangMingUNSW/comp9312-26t2` (default branch `main`). It is the user's cross-platform store for all COMP9312 (Data Analytics for Graphs, UNSW) assignments and projects.

- Each assignment lives in its own top-level folder (`assignment1/`, `assignment2/`, `project/`, …).
- `.claude/` and LaTeX build artifacts are gitignored.
- git user is YangMingUNSW / larryrowau@gmail.com.
- Per-machine clone paths (project key differs per path): `C:\Users\A\Desktop\UNSW\9312` (original machine — moved under `UNSW/` since the first sessions, which used `C:\Users\A\Desktop\9312`); `C:\Users\26616\Desktop\COMP9312` (larry-laptop, cloned via SSH 2026-06-16). On larry-laptop the SSH key is `~/.ssh/id_ed25519` (fingerprint `SHA256:IDyYD/AeDP9YIGqXLOCL54l3cH07ZgVPYFkL3lKRrZM`), added to GitHub; gh CLI is NOT installed here.
- The repo was renamed `comp9312` → `comp9312-26t2`; if `git push` reports "This repository moved", run `git remote set-url origin https://github.com/YangMingUNSW/comp9312-26t2.git`.
- TeX Live 2026 is installed locally (`/c/texlive/2026/bin/windows/pdflatex`), so the `.tex` can be compiled and proofread before the user opens Overleaf.

See [[solution-file-convention]] for the per-assignment two-file output rule.
