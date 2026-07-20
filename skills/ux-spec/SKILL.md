---
name: ux-spec
description: >
  Reach 100% intent consensus with the user on a design problem — the user's
  intent, the underlying need, the target UX experience, and the end-to-end
  user flow — then pin it down with concrete flow scenarios that cover the
  happy path, boundaries, and exceptions. These scenarios double as the
  feature's acceptance criteria — this brief is the single home for all
  requirement examples and AC; downstream skills consume them, never restate
  them. Compile a Design Brief written purely in user/experience language for
  handoff to ui-spec.
  Produces no design artifacts and edits no files other than the brief.
  Triggered by: "ux-spec", "探討需求", "設計探索", "UX 探索", "/ux-spec".
---

## Core Rule

**Produce no design, edit no code.** The only output this skill produces is a Design Brief written to `.sdd/`. This skill does not draw wireframes, choose visuals, or build anything — those belong to `ui-spec` and `design-build`. The only accepted confirmation phrases are **"Confirm"** or **"Go"**.

**Stay at the experience level.** The brief describes *what the user needs and what experience they should have*, never *how the screen is laid out* (that is `ui-spec`) or *how it looks / is built* (that is `design-build`). Do not specify concrete UI elements, positions, component names, colors, fonts, sizes, spacing, or any visual/implementation detail. Write in the vocabulary a user or a non-designer stakeholder would use (align with `.sdd/UL-MAP.md` when it exists). If a fact can only be stated as a visual/layout decision, it is a decision for `ui-spec`, not the brief.

**What ux-spec *does* cover:** the user's intent and underlying need, the jobs they are trying to get done, the qualities the experience must have (e.g. fast, low-friction, trustworthy, reassuring), and the **end-to-end user flow** — the sequence of steps and decision points a user moves through, described conceptually (not as specific screens or controls).

---

## Consensus Loop

### Step 1 — Investigate First

When the user presents a request, **investigate before asking anything**. This step is read-only — open no editor tool, produce no design.

1. **Scan existing design & product context** relevant to the request: existing screens, flows, prototypes, design-system docs, and any prior art for similar experiences. Note how similar journeys are already handled.
2. **Read sibling design docs.** List `.sdd/` and read related `DESIGN-BRIEF.md` / `UI-SPEC.md` files in other feature folders, plus `.sdd/UL-MAP.md` and `.sdd/DESIGN-FOUNDATIONS.md` if they exist. Reuse decisions, conventions, personas, and vocabulary already established there instead of re-asking.
3. **Write down** your understanding of the user's goal in plain language, and an explicit list of what you now know from investigation vs. what is still genuinely unknown.

---

### Step 2 — Clarification (only if needed)

After investigating, decide whether questions are actually necessary:

- If existing designs and docs already answer everything and the intent is unambiguous → **skip to Step 3** and state that no clarification was needed.
- Otherwise, ask **only** about what investigation could not resolve. Never ask about something you could have learned from existing designs or sibling docs.

Focus questions on: the user's real intent and underlying need, who the users are and their context, the experience qualities that matter, and the shape of the end-to-end flow (entry points, key steps, decision points, exit/success). Present using the exact structure in [references/clarification-format.md](references/clarification-format.md).

Wait for the user's answers before continuing.

---

### Step 3 — Concrete Flow Scenarios (Specification by Example)

Once you believe you have captured the user's intent, do **not** jump straight to abstract requirements. First make the intent concrete: turn the experience into a short list of **real, specific flow scenarios**, then present them for confirmation. A wrong assumption is far easier to catch in a concrete walk-through than in a prose rule.

**These scenarios are the feature's acceptance criteria.** This brief is the single, canonical home for every requirement example and AC condition. `ui-spec` and `design-build` consume them for coverage but never restate, re-derive, or reformat them — there are no scenarios or Given/When/Then anywhere downstream.

For each part of the flow, cover at minimum:

- the **happy path** — the ordinary, expected journey;
- every **boundary** — the exact edge where the experience changes (first-time vs. returning user, empty vs. full, the limit/threshold, the last valid step);
- every **exception** — situations that send the user down a different path (invalid input, no permission, interruption, offline/failure, nothing found).

If the flow has a branch, there must be a scenario on each side of it. A branch with no accompanying scenario is not yet agreed.

**Data minimality — the key principle.** Each scenario carries **only the situational facts that change the flow or the experience** — nothing more. Do not describe every detail of the user or the screen; describe the facts that make *this* scenario lead to *this* experience. Irrelevant detail hides the rule. If a fact does not change the outcome, leave it out (or mark it `—`).

Phrase every scenario in user/experience language (see the Core Rule) — describe the situation, the user's action, and the experience they get, not the UI that delivers it.

Present using the format in [references/examples-format.md](references/examples-format.md).

Wait for the user to confirm or correct the scenarios. Any correction may surface a new branch or hidden need → return to **Step 2**.

---

### Step 4 — Proposal

After ambiguities are resolved and the scenarios are confirmed, present the brief outline using the format in [references/proposal-format.md](references/proposal-format.md).

If the user's response introduces new needs or changes scope → return to **Step 2**.

---

### Step 5 — Output

Only after receiving **"Confirm"** or **"Go"**:

1. Determine today's date in `yyyy-MM-dd` format and the feature slug (e.g., `checkout-flow`).
2. Create the feature folder `.sdd/{yyyy-MM-dd}-{feature-slug}/` (create `.sdd/` first if absent).
3. Write `.sdd/{yyyy-MM-dd}-{feature-slug}/DESIGN-BRIEF.md` using [references/design-brief-template.md](references/design-brief-template.md).
4. Before finishing, re-read the brief and confirm it stays at the **experience level** — no UI elements, layouts, positions, component names, or visual values leaked in. Strip or rephrase anything that jumped ahead to the solution.
5. Report: "Design Brief written to `.sdd/{yyyy-MM-dd}-{feature-slug}/DESIGN-BRIEF.md`. Next: run `ui-spec` to work out the UI design (layout + concrete visual values). `ubiquitous-language-mapping` is optional and best run *after* `ui-spec`, to capture the shared vocabulary the design settled on."

---

## Hard Constraints

| Constraint | Rule |
|---|---|
| No design, no code | `Edit` / `Write` on anything other than the brief, and any design artifact, are blocked |
| Only output | The sole file written is `DESIGN-BRIEF.md` inside the feature folder |
| Experience language only | The brief contains zero solution detail — no UI elements, layouts, positions, component names, colors, fonts, sizes; only user/experience language |
| Scenarios before prose | Intent must be pinned with concrete flow scenarios (happy path + every boundary + every exception) before the proposal; an un-scenarioed branch is not agreed |
| Data minimality | Every scenario carries only the facts that change the flow or experience — never a full dump of user or screen detail |
| Re-loop on new info | Any new need or branch from the user resets to Step 2 |
| Multiple-choice questions | Every open question offers ≥3 concrete options plus a final "Other — type your own answer" option |
