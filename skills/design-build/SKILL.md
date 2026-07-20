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

---

## Step 5 — Report

Report:
- The target medium and where the output is (folder path for HTML+CSS; file/page link or key for Figma).
- The visual-value source: committed values taken from `UI-SPEC.md`; list the build-time values you committed for details the spec left open.
- The coverage table from Step 4.
- Any open items handed back (e.g. scenarios that need a spec clarification, or `TBD` values that need a decision).

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
| Honest fallback | If the Figma MCP cannot write, say so plainly and offer HTML+CSS — never pretend a Figma file was created |
