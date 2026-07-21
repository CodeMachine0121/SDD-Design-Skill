# SDD Design Skills

**English** · [繁體中文](README.zh-TW.md)

A set of **design-oriented Spec-Driven Design (SDD)** skills that carry a fuzzy design requirement all the way to an experienceable screen. Four skills each own one job and hand off in sequence; every step produces one document under `.sdd/` that becomes the sole input to the next.

Core idea: **acceptance criteria have exactly one home, UI decisions are made only at the UI stage, and the build stage never re-does design decisions.**

And the pipeline is **not a one-way street**: when a downstream skill finds a defect that actually belongs upstream (e.g. `design-build` is halfway through and discovers a scenario has no screen to place it on), a **feedback loop** routes the problem back to the owning skill for the fix, then re-syncs forward — and every document has exactly one skill that may edit it.

> 📊 **Interactive flow diagram**: to grasp the whole pipeline and its feedback loop quickly, open [`docs/flow.html`](docs/flow.html) (中文版：[`docs/flow.zh-TW.html`](docs/flow.zh-TW.html)). It has a diagram you can filter by flow direction, plus a step-by-step animation of “how one handback closes the loop”.

---

## Installation (via Claude Code commands)

Two steps in Claude Code install the plugin.

**1. Add the marketplace** (register this repo as a plugin source):

```
/plugin marketplace add CodeMachine0121/SDD-Design-Skill
```

**2. Install the plugin**:

```
/plugin install sdd-design-skills@SDD-Design-Skill
```

> `sdd-design-skills` is the plugin name; `SDD-Design-Skill` is the marketplace name added in the previous step.

After installing, restart Claude Code (or run `/plugin` to check status). The four skills' triggers (e.g. `/ux-spec`, `/ui-spec`, `/design-build`) are then available.

**Manage & update**:

```
/plugin                                        # open the plugin manager: enable / disable / inspect
/plugin marketplace update SDD-Design-Skill    # update to the latest marketplace version
/plugin uninstall sdd-design-skills@SDD-Design-Skill
```

---

## Pipeline overview

```
                 handoff ──▶ (downstream takes the upstream doc as its sole input)
   ux-spec              ui-spec                (optional)              design-build
 ┌──────────┐        ┌──────────┐          ┌────────────────────┐   ┌──────────────┐
 │ need +   │  ───▶  │ UI design │  ───▶    │ ubiquitous-        │   │ real screens+│
 │ flow +   │        │ + values  │          │ language-mapping   │──▶│ playable     │
 │ AC scen. │        │ (per guide)│         │ (shared vocab, opt)│   │ (HTML/Figma) │
 └──────────┘        └──────────┘          └────────────────────┘   └──────────────┘
 DESIGN-BRIEF.md      UI-SPEC.md              UL-MAP.md                 build/
      ▲                    ▲                                                │
      │                    └──────── handback: screen/state/value missing or unbuildable ──┤
      └──────────────────────────── handback: need/flow/scenario(AC) wrong or infeasible ──┘
                 ◀── Handback (to the owner) + Re-sync forward (incremental) after the fix
```

| Stage | Skill | Output | In one line |
|---|---|---|---|
| 1 | **ux-spec** | `DESIGN-BRIEF.md` | Settle the need and flow in **experience language**, pinned down with scenarios (Specification by Example) — those scenarios **are the acceptance criteria (AC)** |
| 2 | **ui-spec** | `UI-SPEC.md` | Purely **UI concerns**: screens, elements, hierarchy, states, interactions, plus the visual values **this feature actually needs** (resolved from the design guideline) |
| 3 | **ubiquitous-language-mapping** | `UL-MAP.md` | **Optional**. After ui-spec, gather the vocabulary the design landed on into one shared dictionary |
| 4 | **design-build** | `build/` (HTML or Figma) | **Pure implementation**: build the screens from UI-SPEC, verified against the brief's AC |

---

## The skills

### 1. `ux-spec` — requirement & experience exploration
- **What it does**: reach 100% intent consensus with the user — intent, underlying need, target experience, end-to-end flow — then pin the intent down with **concrete scenarios** (happy path / boundaries / exceptions).
- **Key principles**:
  - Stays entirely in **experience language** — no UI elements, layout, or visual values.
  - Scenarios = **acceptance criteria**. This brief is the single source for every requirement example and AC; downstream only references them, never rewrites them.
- **Output**: `.sdd/{yyyy-MM-dd}-{feature-slug}/DESIGN-BRIEF.md`
- **Triggers**: `ux-spec`, `探討需求`, `設計探索`, `/ux-spec`

### 2. `ui-spec` — UI design
- **What it does** (FEATURE mode): take the confirmed brief and work out, with the user, each screen's elements, positions, hierarchy, states, and interactions. Because UI is decided here, it **may and should** commit to concrete visual values (color usage, sizing, spacing, radius, type, borders) — but only **to the depth the feature actually needs**, asking the questions the feature calls for rather than mechanically probing every element's radius/hex.
- **Design guideline**: ui-spec is where the guideline is consumed. If the user provides design tokens / a design system, committed values trace to it; anything the guideline is silent on is flagged `assumed`, and anything not yet decidable is `TBD`.
- **Does NOT**: contain **any** scenarios or Given/When/Then — AC stay in the brief; here they are only referenced for coverage.
- **Also a FOUNDATIONS mode**: `/ui-spec foundations` analyzes UL-MAP and existing design to produce `.sdd/DESIGN-FOUNDATIONS.md` (layout system, component inventory, interaction & accessibility conventions).
- **Output**: `.sdd/{yyyy-MM-dd}-{feature-slug}/UI-SPEC.md`
- **Triggers**: `ui-spec`, `UI 設計`, `畫面規格`, `/ui-spec`, `/ui-spec foundations`

### 3. `ubiquitous-language-mapping` — shared design vocabulary (optional)
- **What it does**: maintain a dictionary that keeps “user's term ↔ component/pattern name ↔ UI label ↔ design token” pointing at one concept.
- **When**: **optional**, and best run **after ui-spec** — once the UI design has settled the screens, components, labels, and values, capture the vocabulary that landed. Worth doing when vocabulary is reused across features; skippable for a one-off.
- **Output**: `.sdd/UL-MAP.md`
- **Triggers**: `ubiquitous language`, `UL map`, `設計語言`, `/ul-init`, `/ul-update`

### 4. `design-build` — produce the real design
- **What it does**: turn UI-SPEC into something operable. **Pure implementation**, no new visual decisions: build the values the spec fixed; for details the spec deliberately left open, commit sensible build-time values consistent with the guideline/foundations and list them.
- **Oracle**: the **DESIGN-BRIEF scenarios (AC)** — walked one by one and confirmed observable in the built result.
- **Two output targets**:
  - **HTML**: a **single self-contained `index.html`** with inlined CSS + JS and mock data driving every screen/state, so a reviewer can walk the whole flow in one file.
  - **Figma**: besides the screens, a **fully wired, playable prototype** — pressing Play walks the whole user flow (transitions included).
- **Output**: `.sdd/{yyyy-MM-dd}-{feature-slug}/build/`
- **Triggers**: `design-build`, `build the design`, `生成設計`, `/design-build`

---

## `.sdd/` output structure

```
.sdd/
├── UL-MAP.md                          # cross-feature shared vocabulary (optional)
├── DESIGN-FOUNDATIONS.md              # cross-feature design foundations (optional, from ui-spec foundations)
└── {yyyy-MM-dd}-{feature-slug}/       # one folder per feature
    ├── DESIGN-BRIEF.md                # ux-spec: need + flow + AC scenarios
    ├── UI-SPEC.md                     # ui-spec: UI design + visual values
    └── build/                         # design-build: single HTML file or Figma link
```

---

## Typical flow

1. **`ux-spec`** — settle what to solve, how the user moves through it, and all the scenarios (= AC). Get `DESIGN-BRIEF.md`.
2. **`ui-spec`** — design the screens and UI details (bring in a design guideline if you have one). Get `UI-SPEC.md`.
3. *(optional)* **`ubiquitous-language-mapping`** — gather this design's vocabulary. Get/update `UL-MAP.md`.
4. **`design-build`** — pick HTML or Figma, build the design into an experienceable result, and verify against the brief's AC.

---

## Feedback loop (Handback & Re-sync)

The pipeline is bidirectional. When a downstream skill finds a defect that actually belongs upstream, it never patches over it and never cross-edits an upstream document. Instead:

- **Handback (upward)**: the downstream skill stops and emits a structured handback block (what's wrong, which scenario/element in which document it traces to, the proposed fix), and **routes it to the skill that owns that document**.
- **Re-sync (downward)**: after the owner amends its document, the downstream skill detects that its own output is stale and reconciles by **incremental revision** (only the affected parts) rather than rebuilding.

**Handback routing:**

| Nature of the defect | Routes back to | Document fixed |
|---|---|---|
| Need / flow / scenario (AC) wrong, missing, or **infeasible as an experience** | `ux-spec` | `DESIGN-BRIEF.md` |
| Screen / element / state / interaction / visual value missing, contradictory, or **unbuildable** | `ui-spec` | `UI-SPEC.md` |
| Vocabulary drift / a new term to capture | `ubiquitous-language-mapping` | `UL-MAP.md` |

**Two rules that keep it safe:**

- **Single ownership**: each document is edited by exactly one skill (brief→ux-spec, UI-SPEC→ui-spec, UL-MAP→ul-mapping, build→design-build). Feedback works through handback + re-sync, never cross-editing — which is exactly what lets “AC has one home” hold.
- **Convergence first**: if it can be resolved locally with low impact, don't hand it back (e.g. design-build just commits sensible values for details the spec left open); only **structural / AC-affecting** defects escalate, and handbacks are batched into one pass to avoid back-and-forth.

---

## Design principles (consistent across skills)

- **AC has one home**: every requirement example and acceptance criterion lives in `DESIGN-BRIEF.md`; downstream references, never duplicates.
- **Language layering per stage**: ux-spec in experience language; ui-spec talks UI and visual values; design-build only implements.
- **Ask by judgment, not a fixed checklist**: ui-spec only asks about the UI details this feature actually needs.
- **Build doesn't re-do design**: design-build follows the spec and, for open details, commits sensible values marked as assumptions.
- **Bidirectional feedback, single ownership**: downstream defects are handed back to the owner via Handback, then Re-synced forward; no skill edits a document it doesn't own.
- **Multiple-choice questions**: whenever the user is asked something, offer ≥3 concrete options plus “Other”.
