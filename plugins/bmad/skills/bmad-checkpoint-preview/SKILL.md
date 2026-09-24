---
name: bmad-checkpoint-preview
description: 'Walk the user through reviewing a change: what it is for, what to look at closely, and how to test it. Use when the user says "checkpoint", "human review", or "walk me through this change"'
---

# Checkpoint Review Workflow

**Goal:** Guide a human through reviewing a change — from purpose and context into details.

**Your Role:** You are assisting the user in reviewing a change.

## Conventions

- Bare paths (e.g. `step-01-orientation.md`) resolve from the skill root.
- `this skill folder` resolves to this skill's installed directory.
- `the workspace root`-prefixed paths resolve from the project working directory.
- `the skill name` resolves to the skill directory's basename.

## On Activation

### Step 1: Load Workflow Inputs

Use workflow settings supplied explicitly by the caller. This plugin does not bundle a workflow resolver or generated defaults; when a setting is absent, use the behavior described in this skill and its step files.

### Step 2: Execute Prepend Steps

Execute each entry in `the workflow setting `activation_steps_prepend`` in order before proceeding.

### Step 3: Load Persistent Facts

Treat every entry in `the workflow setting `persistent_facts`` as foundational context you carry for the rest of the workflow run. Entries prefixed `file:` are paths or globs under `the workspace root` — load the referenced contents as facts. All other entries are facts verbatim.

### Step 4: Load Config

Load config from `the workspace root/the workspace configuration` and resolve:

- `implementation_artifacts`
- `planning_artifacts`
- `communication_language`
- `document_output_language`

### Step 5: Greet the User

Greet the user, speaking in `{communication_language}`.

### Step 6: Execute Append Steps

Execute each entry in `the workflow setting `activation_steps_append`` in order.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Global Step Rules (apply to every step)

- **Path:line format** — Every code reference must use CWD-relative `path:line` format (no leading `/`) so it is clickable in IDE-embedded terminals (e.g., `src/auth/middleware.ts:42`).
- **Front-load then shut up** — Present the entire output for the current step in a single coherent message. Do not ask questions mid-step, do not drip-feed, do not pause between sections.
- **Language** — Speak in `{communication_language}`. Write any file output in `{document_output_language}`.

## FIRST STEP

Read fully and follow `./step-01-orientation.md` to begin.
