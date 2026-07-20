# Figma build (via Figma MCP)

Goal: recreate the UI spec's screens as Figma frames **and wire a complete, playable prototype** — driven through the connected Figma MCP — so that when the designer presses **Play**, they can experience the entire user flow end to end (every screen, transition, and the brief's happy path / boundaries / exceptions), not just view static frames.

## Before building — capability check

1. Confirm the Figma MCP is connected. If it requires auth, run its `authenticate` flow first and have the user complete it.
2. Discover the tools the connected Figma MCP actually exposes (via ToolSearch). You need tools that **create/write** (frames, layers, text, components, styles, prototype links). 
3. **If only read/auth tools are available** (no create/write): stop and tell the user plainly:
   > "The connected Figma MCP can authenticate/read but does not expose tools to create or write a Figma file, so I can't build the design directly in Figma right now. Options: (A) enable a write-capable Figma MCP and I'll proceed, or (B) I build it as HTML+CSS instead."
   Do not pretend a file was created.

## Build order (when write tools exist)

1. **Set up tokens** — create Figma color styles, text styles, and variables (space/radius) from the UI spec's **Design Tokens** where the MCP supports them; otherwise apply the spec's raw values and note it.
2. **One frame per screen** at a sensible device size from the spec's responsive notes. Lay out regions and elements per the spec's wireframe and element table — position, hierarchy, primary/secondary actions — applying each element's concrete visual values from the spec.
3. **Name every layer/component** using UL-MAP vocabulary; reuse components for repeated patterns (cards, buttons, inputs).
4. **Build every state** the spec lists (default/empty/loading/error/success/restricted) as separate frames or component variants, with each state's visual deltas.
5. **Wire the full prototype for play-through** — this is a required deliverable, not optional polish:
   - Connect the frames along the brief's **user flow** so a single Play session walks entry → each step → success, and also reaches every boundary and exception path.
   - Add prototype interactions per the spec's Interactions & Transitions (triggers, navigation, overlays) and apply the spec's transition/animation intent (Smart Animate / dissolve / move-in with the timing & easing the spec records) so motion is experienced, not just implied.
   - Set the correct **flow starting point(s)** so pressing Play begins at the entry screen.

## After building

Run the Step 4 verification against the **brief's acceptance-criteria scenarios** using the prototype: for each scenario, start from the scenario's situation frame/state, follow the user action through the wired prototype, confirm the outcome. Record the coverage table and return the file/page reference the MCP gives you.
