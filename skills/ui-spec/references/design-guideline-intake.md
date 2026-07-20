# Design Guideline Intake — token model & interview

`ui-spec` decides the concrete visual values, so it resolves them against a design guideline first. Whether the user provides a guideline, you interview for one, or you default it, normalize to the token model below and record the resolved values in the UI-SPEC's **Design Tokens** section. Every concrete value in the spec then references a token here (or is flagged `assumed` / `TBD`).

## Token model

```json
{
  "color": {
    "brand":      { "primary": "#…", "onPrimary": "#…" },
    "surface":    { "background": "#…", "card": "#…", "border": "#…" },
    "text":       { "primary": "#…", "secondary": "#…", "onBrand": "#…" },
    "status":     { "success": "#…", "warning": "#…", "danger": "#…", "info": "#…" }
  },
  "typography": {
    "fontFamily": "…",
    "scale": { "display": 32, "title": 20, "body": 16, "caption": 13 },
    "weight": { "regular": 400, "medium": 500, "bold": 700 },
    "lineHeight": { "tight": 1.2, "normal": 1.5 }
  },
  "space": { "unit": 4, "scale": [0, 4, 8, 12, 16, 24, 32, 48] },
  "radius": { "sm": 4, "md": 8, "lg": 16, "pill": 999 },
  "elevation": { "card": "0 1px 3px rgba(0,0,0,.12)", "overlay": "0 8px 24px rgba(0,0,0,.16)" },
  "component": {
    "button": { "paddingX": 16, "paddingY": 10, "radius": 8, "height": 48 },
    "input":  { "paddingX": 12, "paddingY": 10, "radius": 8, "borderWidth": 1, "height": 44 }
  }
}
```

## Handling the intake answer

- **A — user provides a guideline:** ask for the file path or pasted contents. Parse it into the model above (color, typography, spacing, radius, elevation, component styles). If the shape is unfamiliar, map it as best you can and **confirm the mapping** with the user before using it. Record the source path/name in the UI-SPEC header.
- **B — no guideline, establish a minimal set now:** run the token interview below and record the answers in the UI-SPEC's Design Tokens section (source: `interviewed`).
- **C — infer defaults:** derive tokens from `DESIGN-FOUNDATIONS.md` and existing screens; where they are silent, use accessible, neutral defaults and **flag every one `assumed`** so the user can override.

## Where the values land

- Fill the UI-SPEC **§2 Design Tokens** table with the resolved values you actually use and their source — no need to transcribe the whole guideline.
- In the per-element visual table, record only the details that matter for the feature (see the template). Where you do commit a value, prefer a **token reference** (`radius.md`, `brand.primary`) over a bare number; use a raw value only for a genuine one-off and flag it `assumed`.
- `design-build` consumes whatever the spec committed to — it does not re-ask for a guideline.

## Token interview (option B — when the user has no guideline)

Ask these as multiple-choice (≥3 concrete options + "Other — type your own answer"), one at a time. Keep it to the minimum needed to give every element in the spec a concrete value — do not design a full system unless asked.

1. **Base palette** — brand/primary color and neutral surface direction (light-first / dark-first / high-contrast).
2. **Type** — font family (system sans / geometric sans / serif) and a base body size.
3. **Spacing unit** — 4px / 8px / other base grid.
4. **Corner radius** — sharp (0–2) / soft (8) / rounded (16+).
5. **Status colors** — accept conventional (green/amber/red/blue) or specify.
6. **Control sizing** — default control height and padding (e.g. 44/48px touch target).
