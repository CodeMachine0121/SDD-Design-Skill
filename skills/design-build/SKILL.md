---
name: design-build
description: >
  Turn a confirmed Design Brief + UI Spec into an actual design. Implementation
  stage: reads the feature's UI-SPEC.md (the UI design and whatever visual
  values it committed to, resolved from the design guideline) and DESIGN-BRIEF.md
  (the acceptance-criteria scenarios), plus UL-MAP.md / DESIGN-FOUNDATIONS.md,
  then builds real screens and interactions via one of two targets: Figma
  (through the connected Figma MCP) or self-contained HTML+CSS files. Honors
  every value the spec fixed; for visual details the spec deliberately left
  open, commits sensible choices at build time (consistent with the guideline /
  foundations) and lists them — without re-asking for a design guideline.
  Verifies every acceptance-criteria scenario in the brief is realized.
  Triggered by: "design-build", "build the design", "生成設計", "實作設計",
  "/design-build".
---

## What this skill does

This is the implementation stage of the design pipeline. Upstream, `ux-spec` produced the need, flow, and the **acceptance-criteria scenarios**; `ui-spec` produced the full UI design — structure **and** the concrete visual values (colors, radius, sizes, fonts, spacing) resolved from the design guideline; `ubiquitous-language-mapping` (optional) captured the vocabulary. This skill produces the **real design** — laid-out, styled screens with wired interactions — from those documents. It decides nothing new about the look: every visual value is already in the UI spec.

- **UI-SPEC.md is the source of truth for structure, behavior, and the visual values it committed to.** Build exactly what its Design Tokens, per-element visual details, and states dictate; where the spec deliberately left a visual detail open, commit it here consistent with the guideline/foundations and note it — do not contradict anything the spec fixed.
- **The DESIGN-BRIEF.md scenarios are the oracle.** Every Specification-by-Example scenario there must be realized and verifiable in what you build. (The UI spec restates none of them — read them from the brief.)

---

## Step 1 — Pre-flight

1. Locate the feature folder `.sdd/{yyyy-MM-dd}-{feature-slug}/` and read:
   - `UI-SPEC.md` — **required**. If missing → stop:
     > "No UI-SPEC.md found for this feature. Please run the `ui-spec` skill first."
     Extract its **Design Tokens** table and every per-element visual-values table — these are the values you build with.
   - `DESIGN-BRIEF.md` — **required for the oracle**: its Scenarios are the acceptance criteria you will verify against, plus the goal and experience goals to uphold.
2. Read `.sdd/UL-MAP.md` (component/label/state vocabulary) and `.sdd/DESIGN-FOUNDATIONS.md` if present (layout system, component inventory, interaction & accessibility conventions).
3. **Take stock of the UI spec's visual values.** Whatever it committed to is fixed — do **not** re-ask for a Design Guidelines JSON. The spec intentionally won't pin down every detail; for the gaps, plan to commit sensible build-time values consistent with the guideline/foundations (see the token model in [references/design-guidelines-schema.md](references/design-guidelines-schema.md)) and record them as assumptions. Only stop to ask the user when a *missing* value is both high-impact and genuinely ambiguous — never re-litigate a value the spec already fixed.
4. Build the **build list**: every screen in the UI spec × its states (default/empty/loading/error/success/restricted), plus the interactions, and the brief scenarios each screen must satisfy.
5. **Detect re-sync mode.** If `build/` already exists for this feature, an upstream doc was likely amended (the brief or the UI spec changed) — plan an **incremental** rebuild of only the affected screens/states rather than rebuilding everything (see the [Feedback Loop](#feedback-loop)).

---

## Step 2 — Choose the target medium

Ask which medium to build in:

```
1. How should I build the design?
   A. HTML — one self-contained .html file (inlined CSS + JS, mock data) that plays the whole flow
   B. Figma — frames plus a fully wired, playable prototype of the whole flow, via the connected Figma MCP
   C. Other — type your own answer
```

- **HTML (A):** follow [references/html-build.md](references/html-build.md). Output is a **single** `index.html` with CSS and JS inlined and mock data driving every screen/state, so a reviewer can walk the entire user flow — happy path, boundaries, exceptions — in one file.
- **Figma (B):** follow [references/figma-build.md](references/figma-build.md). Besides the screens, you **wire the full prototype** so pressing Play walks the whole user flow end to end (transitions/animation included). First confirm the Figma MCP is connected and authenticated; if only auth tools are available and no create/write tools are exposed, tell the user the connected Figma MCP cannot write to a file yet, and offer to (i) proceed once they enable a write-capable Figma MCP, or (ii) fall back to HTML.

---

## Step 3 — Build

Work screen by screen from the build list. For each screen:

1. Lay out the regions and elements exactly as the UI spec's wireframe and element table dictate (relative position, hierarchy, primary/secondary actions).
2. Name layers/components and copy using **UL-MAP** vocabulary.
3. Apply the UI spec's **committed visual values verbatim** — the colors, radius, sizes, fonts, spacing, and borders it fixed in its Design Tokens and per-element visual details. Reproduce those, don't re-decide them. For details the spec left open, commit a value now consistent with the guideline/foundations and add it to the assumptions list — never silently contradict the spec, and never leave an element unstyled.
4. Build **every state** the spec lists, not just the default, applying each state's visual deltas.
5. Wire the **interactions and transitions** from the spec (navigation, feedback, what appears/disappears).

Keep a running progress note of screens/states completed.

---

## Step 4 — Verify against the brief's acceptance criteria

Before reporting done, walk the **DESIGN-BRIEF.md scenarios** (the acceptance criteria) and confirm each is realized in what you built:

- For each scenario, put the design in the scenario's situation (`Given`), perform the user action (`When`), and confirm the experience/outcome (`Then`) is actually observable in the built screen.
- Produce a short **coverage table**: scenario → realized? (yes / partial / no) → note.
- Also confirm every screen state in the UI spec exists and matches its specified visual values, and flag anything you could not build and why.

**Any scenario that comes out `partial` or `no` is a defect, not a footnote.** Classify each one with the escalate-vs-patch rule in the [Feedback Loop](#feedback-loop): fix it in place only if it is a genuinely low-impact, unambiguous build-time gap; otherwise **hand it back** to the skill that owns the fix, and report it as blocked rather than declaring done.

---

## Step 5 — Report

Report:
- The target medium and where the output is (folder path for HTML+CSS; file/page link or key for Figma).
- The visual-value source: committed values taken from `UI-SPEC.md`; list the build-time values you committed for details the spec left open.
- The coverage table from Step 4.
- Any **handbacks** raised (see Feedback Loop) — each with its routing and proposed correction — and, if any block completion, say so plainly instead of reporting done.

---

## Feedback Loop

`design-build` is the bottom of the pipeline and **owns only `build/`** — it never edits `UI-SPEC.md` or `DESIGN-BRIEF.md`. When building surfaces a defect in an upstream doc, the fix routes back to that doc's owner; the build never silently paints over it.

### Escalate-vs-patch decision

For every gap or defect you hit while building or verifying, classify it:

- **Patch in place** — a low-impact, unambiguous visual detail the spec deliberately left open (a spacing value, an unspecified hover shade). Commit a value consistent with the guideline/foundations and record it in the assumptions list. *(This is the existing build-time-assumption behavior — unchanged.)*
- **Hand back** — anything **structural or AC-affecting**:
  - a brief scenario cannot be realized because the spec has **no screen / state / interaction** for it;
  - a value the UI spec **committed to** is contradictory or unbuildable;
  - realizing a scenario would require **inventing UI the spec doesn't describe**, or **changing the scenario itself**.

  Do not guess a structural fix and do not fabricate a passing coverage row. Stop, emit a handback, and mark the affected scenarios blocked.

### Handback format

> **⤴ Handback → `<ui-spec | ux-spec>`**
> - **Defect:** <missing screen/state/interaction, or contradictory/unbuildable value, or unrealizable scenario>
> - **Traces to:** <brief scenario ref and/or UI-SPEC element/state>
> - **Why it can't be resolved here:** <why it needs a spec change, not a build-time guess>
> - **Proposed correction:** <the concrete change the owning skill should make>
> - **Then:** run `<owning skill>` to amend → re-run `design-build` to re-sync.

### Routing (which skill owns which defect)

| Defect class | Owning skill | Doc |
|---|---|---|
| Screen / element / state / interaction / visual value missing, contradictory, or unbuildable | `ui-spec` | `UI-SPEC.md` |
| Need / flow / scenario (AC) wrong, missing, or infeasible **as an experience** | `ux-spec` | `DESIGN-BRIEF.md` |
| New component / label / state / token coined while building | `ubiquitous-language-mapping` | `UL-MAP.md` |

When you coined new vocabulary to fill a gap, list it and suggest `ubiquitous-language-mapping` (UPDATE) to fold it into `UL-MAP.md`.

### Re-sync mode (existing `build/`)

When an upstream doc was amended and `build/` already exists, do **not** rebuild everything: diff the amended `UI-SPEC.md` / `DESIGN-BRIEF.md` against the current build, rebuild only the affected screens/states/interactions, re-run Step 4 verification for the touched scenarios, and note in the report what was re-synced.

**Keep the loop convergent.** Prefer patch-in-place for genuine low-impact gaps; escalate only structural/AC-affecting defects; batch all handbacks into one report rather than raising them one at a time.

---

## Hard Constraints

| Constraint | Rule |
|---|---|
| UI spec is the source of truth | Build exactly the screens, elements, states, and committed visual values in `UI-SPEC.md`; do not invent screens, drop states, or contradict a value the spec fixed |
| Brief scenarios are the oracle | Every acceptance-criteria scenario in `DESIGN-BRIEF.md` is verified against the built design |
| No guideline re-intake | Do not re-ask for a Design Guidelines JSON; committed values come from the UI spec. Commit build-time values for the details it left open; only ask the user when a missing value is high-impact and genuinely ambiguous |
| Vocabulary | Layer/component names and copy use `UL-MAP.md` terms |
| Values are explicit | Every visual value either traces to the UI spec or is listed as a build-time assumption; no element ships unstyled |
| Verify before done | Every brief scenario is checked and reported in a coverage table |
| Escalate, don't paper over | Structural / AC-affecting defects are handed back to the owning skill, not silently patched or hidden with a fabricated coverage row |
| Never edit upstream docs | `design-build` edits only `build/`; changes to `UI-SPEC.md` / `DESIGN-BRIEF.md` go through their owning skills via handback |
| Re-sync, don't rebuild | When `build/` already exists after an upstream amendment, rebuild only the affected screens/states |
| Honest fallback | If the Figma MCP cannot write, say so plainly and offer HTML+CSS — never pretend a Figma file was created |
