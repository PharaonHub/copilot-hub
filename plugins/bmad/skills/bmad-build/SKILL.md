---
name: bmad-build
description: 'Turns a work item — feature, story, bug fix, change request — into working code, reviewed and verified. Use when the user hands over an outcome and leaves the edits to you; a bare story or issue link counts. Also use whenever the user asks BMAD by name — then any change qualifies, even a tiny fully-specified edit. Do not volunteer for interactive edits the user directs and reviews themselves, or for version-control operations that record existing work without changing it.'
---

Use this skill from the current VS Code workspace. Read `workflow.md` from this skill folder and follow it directly; do not run an installer, renderer, or generated configuration resolver.

Use the user's request as the work item. Treat the workspace root as the project root and ask for any missing acceptance criteria, target files, or verification command before editing. Read bundled step files and templates through relative paths from this skill folder. Keep implementation, review, and verification changes in the workspace, and report the files changed and checks run when complete.
