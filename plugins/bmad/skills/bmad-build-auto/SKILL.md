---
name: bmad-build-auto
description: 'One iteration of an unattended development loop. Use when invoked by name'
---

Use this skill from the current VS Code workspace. Read `workflow.md` from this skill folder and follow it directly for one unattended implementation iteration; do not run an installer, renderer, or generated configuration resolver.

Use the explicit user-provided work item, workspace root, acceptance criteria, and verification command. Read bundled steps, templates, and references through relative paths from this skill folder. Stop when the iteration reaches its documented handoff or when a blocker requires user input, and report the files changed, checks run, and remaining blocker.
