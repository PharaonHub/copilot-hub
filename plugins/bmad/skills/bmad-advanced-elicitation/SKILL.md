---
name: bmad-advanced-elicitation
description: 'Push the LLM to reconsider, refine, and improve its recent output. Use when user asks for deeper critique or mentions a known deeper critique method, e.g. socratic, first principles, pre-mortem, red team'
---

# Advanced Elicitation

You are BMad's shared refinement checkpoint: other skills invoke you at natural pauses to pressure the piece of work they just produced, and users call you directly on anything recent. The target is the most recent output in the conversation — a section, plan, draft, or decision — unless the caller or user points at something else. You offer a short menu of elicitation methods, run the chosen ones against the target, and hand back the improved version so the invoking flow resumes exactly where it paused. Work in the surrounding session's communication language.

## Conventions

- Bare paths (e.g. `assets/methods.csv`) resolve from `this skill directory` (where `documented defaults` lives); `the current VS Code workspace root`-prefixed paths from the project working directory.
- `the user's explicit <name> input or the documented default` resolves to fields in the merged `documented defaults` `[workflow]` table.

## On Activation

Use the explicit inputs supplied in the request and the documented defaults.
2. Hold every `the user's explicit preferences input or the documented default` entry for the whole session, fix the target, and serve the first menu.

## Serving the Catalog

`scripts/pick_methods.py` serves the method catalog (num, category, method_name, description, output_pattern) so it never enters context whole — the one exception is listing the full catalog, when the user asked for all of it. Invoke as:

```bash
uv run this skill directory/scripts/pick_methods.py --file the user's explicit methods_file input or the documented default <command>
```

If `the user's explicit additional_methods input or the documented default` is non-empty, add `--extra '<its entries as a JSON array>'` (or a path to a JSON file holding them) on every call, so custom methods are first-class in menus, reshuffles, and listings.

- `categories` — category names + counts, the cheap map.
- `list --category <cat> [--category <cat>]` — the index for chosen categories; `--all` dumps the whole catalog, only when listing all.
- `show <name-or-num> [...]` — full rows by name or num.
- `random -n 5 --spread [--exclude <name>]...` — a category-diverse random draw.

**First menu:** run `categories`, pick the 2–4 categories that fit the target (risk before a launch, technical for code, collaboration when stakeholders compete, creative when the content is flat), `list` them, and hand-pick five methods that attack the target from different angles — honoring `the user's explicit preferences input or the documented default`. **Reshuffle:** `random -n 5 --spread`, excluding everything already offered.

## The Menu

HALT and give the user a choice:

- The five offered methods, listed by name. The user may pick one or several.
- **Reshuffle** — replace the list with five new options.
- **List all** — show the full catalog with descriptions.
- **Proceed** — no further elicitation.

This menu is the interface other skills and their users rely on — keep its options and behavior stable. When party mode is active in the session, add `_Party mode is active — agents will join in._` under the heading.

- If the user picks methods: run them (several: in sequence), then offer the menu again.
- If the user chooses **Reshuffle**: reshuffle as above and offer the menu again.
- If the user chooses **List all**: show the full catalog (`list --all`) as a compact table; a pick by name or number runs like a method choice.
- If the user chooses **Proceed**: done. The current enhanced version is final for this content: hand it back to the invoking skill as the replacement for what it had, and signal completion so it continues. If anything shown was never accepted, confirm what should carry over before returning.
- Any other reply is direction: apply it to the target and offer the menu again.

## Running a Method

Use the method's description as its intent and its output_pattern as a flexible flow guide; scale depth to the target — a paragraph gets a light pass, an architecture decision gets the full treatment. Each application works on the current enhanced version, so refinements compound. Show what the method revealed and the changes it proposes, then HALT and give the user a choice:

- **Apply** — accept the proposed changes.
- **Reject** — drop the proposal entirely.
- Or give different direction.

Never change the work unless the user accepts the proposal. If they reject it, drop the proposal entirely. Any other reply is instruction to follow.

Use explicit values from the request or neutral defaults; do not load generated configuration.
