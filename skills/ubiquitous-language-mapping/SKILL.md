---
name: ubiquitous-language-mapping
description: >
  Manage a Ubiquitous Language Map for design — the single shared vocabulary that
  bridges the user's/domain term, the component or pattern name, the user-facing UI
  label, and the design token, within one product area. Optional and best run *after*
  ui-spec has settled the screens, components, labels, and visual values — to capture
  the vocabulary the design actually landed on — then kept in sync as later features
  and design-build introduce new components, labels, and states. Recommended when the
  vocabulary will be reused across features; skippable for a one-off.
  Use when: initializing a new map, or updating/reconciling an existing one.
  Triggered by: "ubiquitous language", "UL map", "設計語言", "共用語言", "design glossary",
  "/ul-init", "/ul-update".
---

## Purpose

A design team drifts when the same thing is called three names — the user says "我的訂單", the design file has a `OrderCard`, and the screen shows the label "近期購買". The UL Map is the one dictionary that keeps the **user's word**, the **component/pattern name**, the **UI label**, and the **design token** pointing at the same concept, so briefs, wireframes, and built designs all speak the same language.

It is an **optional** step that runs **after `ui-spec`**: once the UI design has settled the screens, components, labels, and visual values, run it to capture that vocabulary into one dictionary, then update it whenever a later feature or `design-build` coins a new component, label, state, or token. Skip it for a one-off feature whose vocabulary won't be reused; run it when consistency across features matters.

---

## Mode

| Signal | Mode |
|--------|------|
| "init/create/new", no map file found | **INIT** |
| "update/add/sync/reconcile", map file exists | **UPDATE** |

---

## INIT

1. Confirm: product name, product area (bounded context), maintainer, and sources to scan — the feature's `UI-SPEC.md` (primary, since this usually runs after ui-spec: mine its element names, labels, states, and Design Tokens), the `DESIGN-BRIEF.md`, existing screens/prototypes, and design-system docs.
2. Read the sources. Extract candidates for all 4 sections: concepts/objects, actions/interactions, ambiguities/conflicts, states/tokens.
3. Create the `.sdd/` folder at project root if it does not exist.
4. Generate `.sdd/UL-MAP.md` using [references/ul-map-template.md](references/ul-map-template.md). Pre-fill rows from step 2. Mark unverified entries `Archeology`.
5. Report counts: how many `Archeology` vs `Confirmed`. Remind the user to verify with the design lead / product owner.

---

## UPDATE

1. Read the existing map file at `.sdd/UL-MAP.md` (project root).
2. Apply the requested change — add / refine / resolve / deprecate.
3. Rules:
   - Never delete a row — set status `To Be Deleted` instead.
   - Update `Last Updated` date.
   - When resolving a conflict, fill the Resolution column and promote status to `Confirmed`.
4. Report the diff: rows added / modified / deprecated.

---

## Status Values

| Status | Meaning |
|--------|---------|
| `Archeology` | Found in an existing screen/file; meaning or canonical wording unverified |
| `Confirmed` | Verified with the design lead / product owner |
| `To Be Deleted` | Deprecated; keep the row as history |

---

## Feedback Loop

The UL Map is the vocabulary spine of the pipeline. It is **fed by** the other skills and, when a naming problem is really a deeper problem, **routes it back** to the owning skill.

**Fed from downstream.** `ui-spec` and `design-build` each keep a *running new-terms list* of components/labels/states/tokens they coined. When they finish, run this skill in **UPDATE** mode to fold that list into `.sdd/UL-MAP.md` — this is the normal way the map stays in sync as features land.

**This skill owns only `UL-MAP.md`.** It never edits a brief or a UI spec. If reconciling a term surfaces something bigger than wording, hand it to the owner instead of resolving it here:

| What the conflict really is | Route to |
|---|---|
| Two terms differ because the underlying **need or flow** is ambiguous | `ux-spec` (amend `DESIGN-BRIEF.md`) |
| Two terms differ because a **screen element / label / state** is inconsistent across the design | `ui-spec` (reconcile `UI-SPEC.md`) |
| Pure wording / synonym / canonical-name choice | resolve here (UPDATE), promote to `Confirmed` |

When you resolve a term that a spec used under a different name, note it so the user can re-run the relevant skill to align the doc's wording with the confirmed term.
