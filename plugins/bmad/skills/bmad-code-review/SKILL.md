---
name: bmad-code-review
description: 'Review code changes with several independent reviewers in parallel, then triage and present the findings. Use when the user says "run code review" or "review this code"'
---

# Code Review Workflow

**Goal:** Review code changes adversarially. No noise, no filler.

Subagents, when the capability is available, are an important part of this workflow. Use them as directed by the workflow steps.
If you need an explicit user instruction to run them, ask once now for the whole workflow run.

## Conventions

- Bare paths (e.g. `checklist.md`) resolve from the skill root.
- `this skill folder` resolves to this skill's installed directory.
- `the workspace root`-prefixed paths resolve from the project working directory.
- `the skill name` resolves to the skill directory's basename.

## On Activation

### Step 1: Resolve the Workflow Block

Use workflow settings supplied explicitly by the caller. This plugin does not bundle a workflow resolver or generated defaults; when a setting is absent, use the behavior described in this skill and its step files.

### Step 2: Execute Prepend Steps

Execute each entry in `the workflow setting `activation_steps_prepend`` in order before proceeding.

### Step 3: Load Persistent Facts

Treat every entry in `the workflow setting `persistent_facts`` as foundational context you carry for the rest of the workflow run. Entries prefixed `file:` are paths or globs under `the workspace root` — load the referenced contents as facts. All other entries are facts verbatim.

### Step 4: Load Config

Load config from `the workspace root/the workspace configuration` and resolve:

- `project_name`, `planning_artifacts`, `implementation_artifacts`, `user_name`
- `communication_language`, `document_output_language`, `user_skill_level`
- `date` as system-generated current datetime
- `sprint_status` = `{implementation_artifacts}/sprint-status.yaml`
- `project_context` = `**/project-context.md` (load if exists)
- YOU MUST ALWAYS SPEAK OUTPUT in your Agent communication style with the config `{communication_language}`

### Step 5: Greet the User

Greet `{user_name}`, speaking in `{communication_language}`.

### Step 6: Execute Append Steps

Execute each entry in `the workflow setting `activation_steps_append`` in order.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## WORKFLOW ARCHITECTURE

This uses **step-file architecture** for disciplined execution:

- **Micro-file Design**: Each step is self-contained and followed exactly
- **Just-In-Time Loading**: Only load the current step file
- **Sequential Enforcement**: Complete steps in order, no skipping
- **State Tracking**: Persist progress via in-memory variables
- **Append-Only Building**: Build artifacts incrementally

### Step Processing Rules

1. **READ COMPLETELY**: Read the entire step file before acting
2. **FOLLOW SEQUENCE**: Execute sections in order
3. **WAIT FOR INPUT**: Halt at checkpoints and wait for human
4. **LOAD NEXT**: When directed, read fully and follow the next step file

### Critical Rules (NO EXCEPTIONS)

- **NEVER** load multiple step files simultaneously
- **ALWAYS** read entire step file before execution
- **NEVER** skip steps or optimize the sequence
- **ALWAYS** follow the exact instructions in the step file
- **ALWAYS** halt at checkpoints and wait for human input

## FIRST STEP

Read fully and follow: `./steps/step-01-gather-context.md`
