---
name: ui-spec
description: >
  Two modes: (1) FEATURE — take a confirmed Design Brief and work out the
  full UI design with the user: for each screen, which elements exist (buttons,
  inputs, lists, content blocks), their positions and hierarchy, the states
  (default/empty/loading/error/success), the interactions, and the visual/UI
  details that matter for the feature — resolved against the user's design
  guideline where one exists. This is where the UI is actually decided, so it
  is free to commit to concrete visual values (color usage, sizing, spacing,
  radius, type, borders) whenever they are what the discussion is about; it
  asks the UI questions the feature calls for rather than a fixed checklist.
  Generates a UI-SPEC.md focused purely on UI concerns.
  It contains NO scenarios and NO Given/When/Then — acceptance criteria live
  solely in the Design Brief; ui-spec only references them for coverage.
  (2) FOUNDATIONS — analyze UL-MAP.md and existing design to produce or update
  .sdd/DESIGN-FOUNDATIONS.md (layout system, component inventory, interaction &
  accessibility conventions).
  Triggered by: "ui-spec", "UI design", "畫面規格", "UI 設計", "/ui-spec",
  "/ui-spec foundations".
---

## Mode Detection

Determine mode **before** any other step:

- If the user invoked `/ui-spec foundations`, or said "design foundations / design system overview / 設計基礎" → **FOUNDATIONS mode** (go to [Foundations Pre-flight](#foundations-pre-flight)).
- Otherwise → **FEATURE mode** (go to [Feature Pre-flight](#feature-pre-flight)).

---

## The UI-design level (applies everywhere)

> **A UI spec is where the UI is designed — stay focused on UI concerns.** It defines what is on each screen, where each element sits, its hierarchy, its states, and how it behaves. Because this is where the UI is actually decided, it *may and should* commit to concrete visual values — color usage, sizing, spacing, radius, type, borders, elevation — but only to the depth the feature and the discussion warrant: decide what matters, don't mechanically fill every dimension of every element. **Ask the UI questions the feature calls for**, judged case by case, rather than running a fixed checklist.
>
> **Resolve against the design guideline.** When the user has a design guideline (tokens / design-system export), any concrete value you do commit to should trace to it; where the guideline is silent on something you decided to pin down, mark it `assumed`, and where a needed decision genuinely can't be made yet, mark it `TBD`. Reference components and labels by their **UL-MAP names** and keep copy in the product's established language.
>
> **Two hard exclusions.** (1) This spec contains **no acceptance criteria and no Given/When/Then** — every scenario lives in `DESIGN-BRIEF.md`; here you only *reference* them for coverage. (2) This spec does not *build* anything — producing real screens/prototypes is `design-build`.

---

## Question Format (applies to every question in both modes)

Whenever you ask the user a question, present it as a multiple-choice question:

- Offer **at least three concrete options**, labelled `A`, `B`, `C`, …
- Make the options distinct and mutually exclusive; base them on what you learned during investigation.
- Put the option you recommend first.
- Always add a final option: **`Other — type your own answer`**.

Example:

```
1. Where should the primary action sit on the checkout screen?
   A. <recommended, investigation-based option>
   B. <option>
   C. <option>
   D. Other — type your own answer
```

---

## FOUNDATIONS mode

### Foundations Pre-flight

1. Locate `UL-MAP.md` at `.sdd/UL-MAP.md`.
2. **If not found** → stop. Tell the user:
   > "No UL-MAP.md found. Please run the `ubiquitous-language-mapping` skill first."
3. **If found** → read it. Extract concepts, components/patterns, labels, and states/tokens.
4. Scan existing design signals (if present): existing screens/prototypes, a design-system or component library, brand/style references, any `README`/design docs → infer the current layout system, component inventory, and interaction conventions.
5. **Read sibling design docs.** List `.sdd/` and read `DESIGN-BRIEF.md` / `UI-SPEC.md` in existing feature folders. Mine them for recurring layout patterns, component usage, and interaction conventions.
6. Check if `.sdd/DESIGN-FOUNDATIONS.md` already exists.
   - **If found** → **UPDATE mode**: preserve existing content, only fill gaps or refresh stale sections.
   - **If not found** → **CREATE mode**: draft from scratch.

### Foundations Analysis & Draft

Draft (or update) `DESIGN-FOUNDATIONS.md` using [references/design-foundations-template.md](references/design-foundations-template.md):

- **Design Principles:** leave as `TBD` — must come from the user/design lead.
- **Layout System:** infer grid, breakpoints, and page regions from existing screens.
- **Component Inventory:** pull from UL-MAP components and existing screens.
- **Interaction & Motion Conventions:** infer from existing prototypes; mark gaps `TBD`.
- **Accessibility Baseline:** state the target (e.g. WCAG AA) or `TBD`.

Present the draft; clearly mark every auto-inferred field and every `TBD`.

### Foundations Gap Interview

Investigate first, then ask **only** about sections that remain `TBD` or genuinely uncertain — never about something existing design or sibling docs already establish. One section at a time, using the multiple-choice [Question Format](#question-format-applies-to-every-question-in-both-modes).

### Foundations Output

1. Write (or overwrite) `.sdd/DESIGN-FOUNDATIONS.md`.
2. Report: "DESIGN-FOUNDATIONS.md written to `.sdd/DESIGN-FOUNDATIONS.md`."

---

## FEATURE mode

### Feature Pre-flight

1. Locate the feature folder `.sdd/{yyyy-MM-dd}-{feature-slug}/` and read its `DESIGN-BRIEF.md`.
   - **If no brief is found** → stop. Tell the user:
     > "No DESIGN-BRIEF.md found for this feature. Please run the `ux-spec` skill first."
   - The brief is the primary source. Pay special attention to its **User Flow** and **Scenarios (Specification by Example)** — those scenarios are the feature's **acceptance criteria and stay in the brief**. Use them to make sure every state and interaction they imply has a home on some screen; **do not copy, reformat, or re-derive them into the UI spec**, and write no Given/When/Then of your own.
2. Read `.sdd/UL-MAP.md` **if it exists**. Extract concepts/components, labels, actions/interactions, and states/tokens. Store as **design vocabulary** to name every element and label in the established language.
   - A UL-MAP often does **not** exist yet at this stage — `ubiquitous-language-mapping` is optional and usually runs *after* `ui-spec`. If absent, coin clear terms as you go and **keep a running list of the new terms** so the user can capture them later (surfaced in the final report).
3. Read `.sdd/DESIGN-FOUNDATIONS.md` if it exists — anchor layout, component, and interaction choices to it instead of re-deciding.
4. **Investigate existing design.** Search existing screens/prototypes for layouts and components this feature will touch or resemble. Note conventions and visual values to reuse instead of asking.
5. **Read sibling design docs.** List `.sdd/` and read `UI-SPEC.md` in overlapping feature folders. Reuse their established screen structures, interaction patterns, and resolved visual values.
6. **Build a pre-flight context summary.** Map what each interview section below (including the concrete visual values) can already be answered from the brief, the design guideline, UL-MAP, foundations, and sibling docs vs. what is still a genuine gap. Tell the user which sections are already covered, then only ask about the gaps.

---

### Design Guideline Intake

`ui-spec` is where concrete visual values are decided, so before the interview establish the guideline they trace to. Ask, as the first question:

```
1. Do you have a design guideline (design tokens / design-system export / style guide) I should resolve the UI-SPEC's visual values against?
   A. Yes — I'll provide it (file path or paste)
   B. No — establish a minimal token set with me now
   C. No — infer from DESIGN-FOUNDATIONS.md / existing screens and proceed
   D. Other — type your own answer
```

Handle the answer per [references/design-guideline-intake.md](references/design-guideline-intake.md): normalize whatever the user provides into the token model, run the short token interview if they have none, or derive-and-list-assumptions from foundations. Keep the resolved tokens at hand — every concrete value in the spec must trace back to one (or be marked `assumed` / `TBD`).

---

### UI Design Interview

Interview the user **only for the gaps** identified in pre-flight. If the brief, the design guideline, foundations, UL-MAP, and sibling docs already answer a section, pre-fill it and state your assumption for the user to confirm rather than asking from scratch.

Ask **one section at a time**, using the [Question Format](#question-format-applies-to-every-question-in-both-modes) and the design vocabulary. Work through the table in order, **skipping sections already resolved by investigation**.

| # | Section | Key Questions |
|---|---------|---------------|
| 1 | **Screen Inventory & Navigation** | Which screens/views realize the brief's flow? How does the user move between them (entry points, back, success exit)? |
| 2 | **Layout & Regions** | For each screen, what are the major regions (header, nav, content, actions, footer)? What is the reading/priority order? |
| 3 | **Elements per Screen** | What controls and content live in each region — buttons, inputs, lists, cards, messages? (Name them with UL-MAP terms.) |
| 4 | **Position & Hierarchy** | Where does each element sit relative to the others? Which is the primary action vs. secondary vs. tertiary? |
| 5 | **States** | For each screen: default, empty, loading, error, success, and permission-restricted — what changes in each? |
| 6 | **Interactions & Transitions** | What happens on each action (feedback, navigation, what appears/disappears)? Any transitions or motion intent? |
| 7 | **Content & Copy** | Labels, placeholders, empty-state text, error messages — in the product's established language. |
| 8 | **Accessibility & Responsive** | Focus order, keyboard/touch, and how the layout reflows on small vs. large screens. |
| 9 | **Visual details (as the feature warrants)** | The UI/visual decisions that actually matter for this feature — e.g. how a state looks different, which emphasis/color role an element takes, sizing or spacing where it affects layout or usability, a component's shape where it carries meaning. Pin down what matters; **do not** mechanically ask for every element's radius/hex/size. Where you commit to a value, take it from the design guideline. |

**Clarification rules:**
- **Ask the UI questions the feature calls for — judgment, not a checklist.** Row 9 is a menu of *possible* concerns, not a mandatory sweep. Only pin down a visual value when it matters for this feature (it disambiguates a state, sets emphasis, affects layout/usability) or when the user wants that level of detail. Don't interrogate the user for the radius/hex/size of every element just because the columns exist.
- **Acceptance criteria stay in the brief — never re-ask for them and never write scenarios or Given/When/Then here.** The brief's scenarios are the AC; the interview only fleshes out the *screens, elements, states, and UI details* those scenarios need. Ask about a state or interaction only when the brief implies one you have nowhere to put on screen (e.g. the brief covers success but you have no loading state).
- **When you do commit to a visual value, resolve it to the design guideline.** If it's missing from the guideline, propose one and flag it `assumed`; if it genuinely cannot be decided yet, mark it `TBD`.
- Ask every question as multiple choice — ≥3 concrete options plus "Other — type your own answer". For a visual value, options should be concrete candidates drawn from the guideline (e.g. `A. radius.md = 8px`).
- If an answer introduces a component, label, state, pattern, or token not yet named, note it in the running new-terms list.
- If an answer is ambiguous, ask one targeted follow-up before moving on.
- If the user says "skip" or "N/A", record it as `N/A` and continue.

---

## Output — Generate UI-SPEC

1. Ensure the feature folder `.sdd/{yyyy-MM-dd}-{feature-slug}/` exists.
2. Write `.sdd/{yyyy-MM-dd}-{feature-slug}/UI-SPEC.md` using [references/ui-spec-template.md](references/ui-spec-template.md).
   - Fill every section from the interview, the brief, and the resolved design guideline.
   - Use UL-MAP vocabulary (or your running new-terms list) for every element name and label.
   - Represent each screen's layout as a **region breakdown plus an ASCII wireframe sketch** (see the template) so position and hierarchy are unambiguous.
   - **Record any design tokens you resolved** (only the ones the feature actually uses — color / typography / spacing / radius / elevation) in the spec's Design Tokens section, noting the source guideline and flagging every `assumed` / `TBD` value.
   - **Record the visual details you decided** in the per-element visual spec: capture the values that matter for the feature (state appearance, emphasis/color role, sizing/spacing where it counts, shape where it carries meaning). Prefer a reference to a resolved token (`radius.md`) over a bare number; flag `assumed` / `TBD` as needed. It's fine to leave a dimension unstated when the feature doesn't turn on it — don't pad the table for completeness' sake.
   - **No acceptance criteria, no scenarios, no Given/When/Then** anywhere in the spec. Instead, the AC live in `DESIGN-BRIEF.md`; add a short **Acceptance Criteria** pointer section that references the brief and confirms every scenario's states/interactions have a home in these screens.
   - Mark any section you left open `TBD`.
3. **Focus & no-scenario check.** Re-read the finished spec and confirm: (a) it stays on UI concerns and every visual value you *did* commit to traces to a token or is flagged `assumed`/`TBD` (values you deliberately left to `design-build` are fine — just don't leave a value you implied but never stated); and (b) the spec contains **zero scenarios / Given/When/Then** — those belong only to the brief. Fix any violation before reporting.
4. Report: "UI-SPEC written to `.sdd/{yyyy-MM-dd}-{feature-slug}/UI-SPEC.md`."
   - List any new components/labels/states/tokens you introduced, then add: "Optionally run `ubiquitous-language-mapping` now to capture this vocabulary into `.sdd/UL-MAP.md` (recommended if this vocabulary will be reused across features)."
   - Then: "Next: run `design-build` to build the actual design from this spec."
