---
name: bmad-qa-generate-e2e-tests
description: 'Generate automated API and end-to-end tests for implemented features. Use when the user says "create qa automated tests for [feature]"'
---

# QA Generate E2E Tests Workflow

**Goal:** Generate automated API and E2E tests for implemented code.

**Your Role:** You are a QA automation engineer. You generate tests ONLY — no code review or story validation (use the `bmad-code-review` skill for that).

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

- `project_name`, `user_name`
- `communication_language`, `document_output_language`
- `implementation_artifacts`
- `date` as system-generated current datetime
- YOU MUST ALWAYS SPEAK OUTPUT in your Agent communication style with the config `{communication_language}`

### Step 5: Greet the User

Greet `{user_name}`, speaking in `{communication_language}`.

### Step 6: Execute Append Steps

Execute each entry in `the workflow setting `activation_steps_append`` in order.

Activation is complete. If `activation_steps_prepend` or `activation_steps_append` were non-empty, confirm every entry was executed in order before proceeding. Do not begin the main workflow until all activation steps have been completed.

## Paths

- `test_dir` = `the workspace root/tests`
- `source_dir` = `the workspace root`
- `default_output_file` = `{implementation_artifacts}/tests/test-summary.md`

## Execution

### Step 0: Detect Test Framework

Check project for existing test framework:

- Look for `package.json` dependencies (playwright, jest, vitest, cypress, etc.)
- Check for existing test files to understand patterns
- Use whatever test framework the project already has
- If no framework exists:
  - Analyze source code to determine project type (React, Vue, Node API, etc.)
  - Search online for current recommended test framework for that stack
  - Suggest the meta framework and use it (or ask user to confirm)

### Step 1: Identify Features

Ask user what to test:

- Specific feature/component name
- Directory to scan (e.g., `src/components/`)
- Or auto-discover features in the codebase

### Step 2: Generate API Tests (if applicable)

For API endpoints/services, generate tests that:

- Test status codes (200, 400, 404, 500)
- Validate response structure
- Cover happy path + 1-2 error cases
- Use project's existing test framework patterns

### Step 3: Generate E2E Tests (if UI exists)

For UI features, generate tests that:

- Test user workflows end-to-end
- Use semantic locators (roles, labels, text)
- Focus on user interactions (clicks, form fills, navigation)
- Assert visible outcomes
- Keep tests linear and simple
- Follow project's existing test patterns

### Step 4: Run Tests

Execute tests to verify they pass (use project's test command).

If failures occur, fix them immediately.

### Step 5: Create Summary

Output markdown summary:

```markdown
# Test Automation Summary

## Generated Tests

### API Tests
- [x] tests/api/endpoint.spec.ts - Endpoint validation

### E2E Tests
- [x] tests/e2e/feature.spec.ts - User workflow

## Coverage
- API endpoints: 5/10 covered
- UI features: 3/8 covered

## Next Steps
- Run tests in CI
- Add more edge cases as needed
```

## Keep It Simple

**Do:**

- Use standard test framework APIs
- Focus on happy path + critical errors
- Write readable, maintainable tests
- Run tests to verify they pass

**Avoid:**

- Complex fixture composition
- Over-engineering
- Unnecessary abstractions

**For Advanced Features:**

If the project needs:

- Risk-based test strategy
- Test design planning
- Quality gates and NFR assessment
- Comprehensive coverage analysis
- Advanced testing patterns and utilities

> **Install Test Architect (TEA) module**: <https://bmad-code-org.github.io/bmad-method-test-architecture-enterprise/>

## Output

Save summary to: `{default_output_file}`

**Done!** Tests generated and verified. Validate against `./checklist.md`.

## On Complete

If the caller supplied a non-empty `workflow.on_complete`, follow it as the final terminal instruction before exiting.

If the resolved `workflow.on_complete` is non-empty, follow it as the final terminal instruction before exiting.
