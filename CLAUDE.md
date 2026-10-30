# CLAUDE.md

This is a trimmed study copy of *Learn Harness Engineering* (branch `focus`). The full upstream course, with 15 languages, the VitePress site and the Electron apps, is on branch `main`.

## What's here

- `my-harness-course/` — the user's own course: build an agent harness from scratch in Python. This is the main work.
- `docs/en/lectures/` — 14 reference lectures (English only).
- `docs/en/projects/`, `docs/en/resources/`, `docs/en/harness-designs/`, `docs/en/skills/` — project descriptions, templates, design breakdowns.
- `projects/project-NN/` — harness files only (AGENTS.md, CLAUDE.md, feature_list.json, init.sh, progress/handoff/checklist docs). The Electron app code was removed; it's on `main`.
- `skills/harness-creator/`, `tools/audit-harness.sh` — harness-file templates and an audit script.

## Conventions

- New code for the user's course is Python 3.12, managed with `uv` (no Node).
- Put the harness project and agent run folders in the Linux filesystem (e.g. `~/my-harness`), not under `/mnt/c`.
