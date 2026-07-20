# DESIGN-BRIEF.md Template (Step 5 output)

Write `.sdd/{yyyy-MM-dd}-{feature-slug}/DESIGN-BRIEF.md` using this template. Everything stays at the experience level — no UI elements, layouts, positions, or visual values.

```markdown
# <Feature Name> — Design Brief

## Goal
<one-paragraph summary of the user need this experience serves and why it matters>

## Target Users
- <role / persona> — <the context they act in: when, where, on what, in what state of mind>

## User Intent & Needs
- <what the user is really trying to accomplish (the job to be done), stated in their words>

## Experience Goals
<the qualities the experience must have — e.g. fast, low-friction, reassuring, trustworthy. These become the yardstick ui-spec and design-build are judged against.>
- <quality> — <what it means concretely for this feature>

## User Flow (conceptual)
<the end-to-end journey as a sequence of steps and decision points — conceptual, not specific screens or controls>
1. <entry point>
2. <step>
3. <decision point → branches>
4. <success / exit>

## Scenarios (Specification by Example) — Acceptance Criteria
These scenarios **are the acceptance criteria** for the feature. This is the single, canonical home for every requirement example and AC condition; `ui-spec` and `design-build` consume them but never restate them. Each scenario lists **only** the situational facts that change the flow or the experience — nothing more.

### Flow / rule: <the branch this set pins down>
| # | Given (only relevant situation) | When (user action) | Then (experience / outcome) |
|---|---|---|---|
| 1 (happy)     | <situation> | <action> | <experience> |
| 2 (boundary)  | <situation> | <action> | <experience> |
| 3 (exception) | <situation> | <action> | <experience> |

## Out of Scope
- <item>

## Open Decisions
Items ui-spec (layout, screens, and concrete visual values) or design-build (implementation) should resolve:
- <item>

## Context / Background
<any relevant notes from the clarification conversation — prior art, constraints, references>
```
