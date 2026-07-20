# Design token model & build-target mapping

The tokens are **already resolved in `UI-SPEC.md`** (its Design Tokens table + per-element visual values) — `ui-spec` handled the guideline intake, so `design-build` does **not** interview for them. This doc is the shared model for their shape/naming and how to map them onto each build target. Use it to emit `tokens.css` and to normalize any value you had to fill for a spec `TBD` during pre-flight.

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
    "button": { "paddingX": 16, "paddingY": 10, "radius": 8 },
    "input":  { "paddingX": 12, "paddingY": 10, "radius": 8, "borderWidth": 1 }
  }
}
```

The UI spec won't pin down every detail — for the ones it left open, commit a value here consistent with the guideline/foundations and record it as a build-time assumption. Only a high-impact, genuinely ambiguous missing value warrants asking the user. Do not run a full guideline interview; that belongs to `ui-spec`.

## Mapping to build targets

- **HTML (single file):** emit each token from the spec's Design Tokens table as a CSS custom property under `:root` in the inlined `<style>` (`--color-brand-primary`, `--space-4`, `--radius-md`, …) and reference them everywhere. One theme block; support a `prefers-color-scheme: dark` override only if the spec includes dark values.
- **Figma:** map to Figma styles/variables where the connected MCP supports them (color styles, text styles, number variables for space/radius). Otherwise apply the raw values from the spec and note that they are not yet abstracted into styles.
