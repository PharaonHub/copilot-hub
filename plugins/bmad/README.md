# BMAD

`bmad` packages the BMAD Method workflow skills for GitHub Copilot's VS Code agent environment.

## Upstream Reference

The source project is [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD).

This plugin packages BMAD skill prompts and their supporting references, templates, schemas, assets, and selected standard-library Python helpers for a native VS Code workspace. It helps teams explore ideas, shape requirements, design solutions, plan delivery, implement stories, and review results.

## Installation

Install the `bmad` plugin from the Copilot Hub marketplace. The marketplace entry points to this directory:

```text
plugins/bmad
```

After installation, reload VS Code or start a new agent session so the skills are discovered. The skills are then available as workspace-aware agent workflows.

## Usage

Invoke a skill by name when its capability matches the work. Examples:

```text
/bmad-help What BMAD workflow should I use for this request?
/bmad-product-brief Create a concise product brief from these notes.
/bmad-prd Turn the approved brief into a PRD.
/bmad-architecture Create the architecture spine for this feature.
/bmad-create-epics-and-stories Break the approved requirements into implementable stories.
/bmad-build Implement the selected story, review the result, and report verification.
/bmad-review Review this pull request and report actionable findings.
```

Skills may also be selected by the agent when another BMAD workflow routes work to them. Provide the relevant existing artifacts and paths in the workspace. The workflows use explicit user inputs and ordinary workspace files instead of installer-generated configuration.

### Plan A Feature

Use the planning skills in sequence when starting from a new product idea:

```text
/bmad-product-brief Create a brief for a service that lets field engineers report equipment issues from a mobile device.
/bmad-prd Turn the approved product brief into a PRD. Keep the first release small and mark assumptions explicitly.
/bmad-ux Define the key user journeys and screens for the approved PRD.
/bmad-architecture Create an architecture spine for the approved PRD and UX decisions.
/bmad-create-epics-and-stories Break the approved requirements into epics and implementable stories.
/bmad-sprint-planning Prepare sprint tracking from the approved stories.
```

Review each artifact before starting the next stage. The skills write to the workspace locations agreed during the conversation and can resume from existing artifacts.

### Implement a Story

For an existing story or a small change, start directly with the build workflow:

```text
/bmad-build Implement `docs/stories/issue-reporting.md` in this repository. First clarify missing behavior, then show the plan, implement it, review the change, and run the relevant tests.
```

For a hands-off workflow where the request is already sufficiently specified:

```text
/bmad-build-auto Implement the approved story in `docs/stories/issue-reporting.md`, using the repository conventions and reporting every verification step.
```

### Explore An Idea

Use brainstorming before committing to a solution:

```text
/bmad-brainstorming Explore several ways to reduce duplicate equipment issue reports. Stay divergent first, then help me converge on decision criteria.
/bmad-advanced-elicitation Challenge the strongest option and expose assumptions I may have missed.
```

Use `bmad-party-mode` when you need multiple BMAD perspectives on a decision:

```text
/bmad-party-mode Have the analyst, product manager, architect, UX designer, and developer perspectives evaluate this proposal.
```

### Review a Change

Review a branch, pull request, document, or artifact explicitly:

```text
/bmad-review Review the current pull request. Focus on behavioral regressions, edge cases, verification gaps, and unclear documentation.
```

The review skill reports actionable findings with locations, consequences, and concrete guards or fixes.

## Included Skills

### Core

- `bmad-help`
- `bmad-brainstorming`
- `bmad-advanced-elicitation`
- `bmad-forge-idea`
- `bmad-deep-recon`
- `bmad-review`
- `bmad-party-mode`
- `bmad-customize`

### Planning

- `bmad-product-brief`
- `bmad-prfaq`
- `bmad-prd`
- `bmad-ux`
- `bmad-architecture`
- `bmad-create-epics-and-stories`
- `bmad-project-context`
- `bmad-generate-project-context`
- `bmad-spec`
- `bmad-sprint-planning`

### Delivery

- `bmad-build`
- `bmad-build-auto`
- `bmad-code-review`
- `bmad-checkpoint-preview`
- `bmad-qa-generate-e2e-tests`
- `bmad-retrospective`
- `bmad-correct-course`

### Agent Personas

- `bmad-agent-analyst`
- `bmad-agent-pm`
- `bmad-agent-architect`
- `bmad-agent-ux-designer`
- `bmad-agent-dev`

## Runtime And Scope

- Markdown skills run without package dependencies.
- Bundled Python helpers use the Python standard library; workflows identify when they add useful deterministic checks or transformations.
- Workflows use explicit user inputs, existing repository context, and ordinary workspace files.
- Planning and delivery artifacts are written to the workspace paths selected during the workflow, following the repository's existing conventions.
- The plugin provides a complete preview workflow from discovery through implementation review, with reusable prompts, templates, checklists, and validation helpers.