# HTML build conventions

Goal: **one self-contained HTML file** that realizes the UI spec exactly, styled from the UI spec's Design Tokens and per-element visual values, and lets a reviewer walk the **entire user flow** — every screen and every conditional state — without any tooling or external assets.

## Output — a single file

- Write **one file**: `.sdd/{yyyy-MM-dd}-{feature-slug}/build/index.html`.
- **Everything is inlined**: CSS in one `<style>`, JS in one `<script>`, no external requests (no CDN, no linked stylesheet, no remote fonts/images — embed assets as data URIs; use a system font stack if no font file is provided). The file must open and work offline by double-click.
- Put the resolved design tokens at the top of the `<style>` as CSS custom properties under `:root` (see design-guidelines-schema.md for the model / naming), then reference them everywhere.

## Driving the flow with mock data

The single file stands in for the running feature, so it fakes the backend with **mock data**:

1. Define a small **mock-data object** in the inline `<script>` — the records/fixtures each screen renders (e.g. an order list, a user, a permission flag), with enough variants to reach every state.
2. **Render screens/states from that data**, not from hard-coded markup — switching a mock value (empty list, error flag, loading, no-permission) drives the screen into the matching state the UI spec lists.
3. Provide a lightweight **flow/state control** — a small on-screen switcher or menu — so a reviewer can jump to any screen and force any condition (default / empty / loading / error / success / restricted), and can walk the brief's happy path, boundaries, and exceptions end-to-end.
4. Wire the spec's **interactions** so acting on a control advances the flow the way the real feature would (navigate to the next screen, show feedback, reveal/hide, apply the state's visual deltas).

## Rules

1. **Structure follows the spec's wireframe** — regions in the reading order the spec gives; primary/secondary/tertiary emphasis reflected in the markup and token usage.
2. **Every visual value is a token reference** — no hard-coded hex/px in the markup except through `var(--…)`. If you need a value the spec's tokens don't have, add it to `:root` and note it as an assumption.
3. **Semantic, accessible HTML** — real `<button>`, `<label>`+`<input>`, landmarks (`header`/`main`/`nav`), logical focus order, visible focus, and the accessibility target from the spec (labels, contrast).
4. **Copy from UL-MAP** — use the established labels and messages; match the spec's Content & Copy section.
5. **Interactions in vanilla JS** — dependency-free, inline; navigation between screens (show/hide sections in the one file), feedback on actions, state transitions.
6. **Responsive** — reflow per the spec's Accessibility & Responsive section using relative units and flex/grid; the body must never scroll horizontally.

## After building

Run the Step 4 verification against the **brief's acceptance-criteria scenarios**: for each scenario, use the mock-data/flow control to reach the scenario's situation, perform the user action, confirm the outcome is observable. Record the coverage table.
