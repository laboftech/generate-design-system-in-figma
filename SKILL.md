---
name: generate-design-system-in-figma
description: >-
  Builds a complete, production-grade design system in Figma — at the quality
  bar of an elite North American design agency — using the remote Figma MCP
  server. Creativity is dialed to maximum: bold color theory, typographic
  sophistication, and meticulous craft in every token, style, and component.
  Creates Color Styles, Text Styles, Effect Styles, Number Variables, and all
  14 components (Accordion, Button, Callout, Card, Dropdown, Checkbox, Chip,
  Input, Tabs, Navigation, Radio Button, Tile, Tooltip, Toggle) with full
  variant/state coverage. Use when the user asks to create, build, or set up
  a design system in Figma, or mentions tokens, components, or a Figma
  library.
when_to_use: >-
  Trigger on: "create design system in Figma", "build Figma library", "set up
  tokens in Figma", "add components to Figma", "build the design system",
  "create color styles", "create text styles", or any combination of Figma +
  design system work.
disable-model-invocation: false
---

# Figma Design System Builder

This skill orchestrates building a complete design system in Figma. It defines WHAT to build and in what order. The `figma-use` and `figma-generate-library` skills (from the Figma plugin) govern HOW to call the Plugin API.

## Prerequisites — Load These First

Before ANY `use_figma` call:
1. Load the `figma-use` skill — Plugin API rules (return pattern, colors, fonts, page switching)
2. Load the `figma-generate-library` skill — Phase workflow, state management, naming conventions

The full design system specification lives in [design-system-spec.md](references/design-system-spec.md). Load it during Phase 0 discovery.

---

## Creative & Quality Bar

The design system produced by this skill must be indistinguishable from what an elite North American design agency would deliver. Creativity is at maximum. Every decision — color, type, spacing, shadow, proportion — must be intentional, opinionated, and sophisticated. Generic, template-like, or "safe" defaults are treated as defects.

### Design Philosophy

This is not a starter kit or a Bootstrap clone. The output must feel like a bespoke brand system built for a high-budget client: cohesive, distinctive, and unmistakably crafted by hand. Every token and component should carry a unified visual signature — not a grab-bag of individually acceptable choices.

### Color

- Palette must demonstrate sophisticated color theory: unexpected but harmonious primaries, refined neutrals with subtle warm or cool undertones, gradients that feel dimensional rather than flat.
- Default blues-and-grays are banned. Push for a non-obvious primary palette with reasoning rooted in color relationships (complementary tension, split-complementary richness, analogous warmth, etc.).
- Shadows should carry a subtle colored tint derived from the palette — never pure-black drop shadows.

### Typography

- Choose a distinctive, premium typeface pairing. Display type should have personality and presence; body type should have excellent readability at small sizes with carefully tuned metrics.
- Never use Inter, Roboto, Open Sans, or system-default fonts. These signal "template."
- Letter-spacing and line-height must be intentionally tuned per style — never left at the font's raw defaults. Tighter tracking on large display type, looser tracking on micro/caption text.

### Spacing & Proportion

- Generous whitespace is mandatory. The spacing scale should breathe — components must feel roomy and confident, not cramped.
- Proportional relationships between elements should feel considered: padding-to-font-size ratios, icon-to-label gaps, card-padding-to-content-width. These relationships must be visually harmonious, not arbitrary multiples.

### Component Craft

- Every component must feel like a handcrafted piece: border radii, shadow intensities, padding ratios — all should feel deliberate and cohesive across the entire system.
- States (hover, focus, active, disabled) must have meaningful visual differentiation through considered color shifts, subtle scale or shadow changes — not just opacity reductions.
- Micro-details matter: the exact radius of a chip vs. a button vs. a card, the weight difference between a default and hover border, the shadow progression across elevation levels.

### Details That Separate Good From Elite

- Subtle color temperature shifts between elevation levels (surfaces get slightly warmer or cooler as they rise).
- Consistent optical alignment — elements should look aligned to the human eye, which sometimes means mathematical offsets for icons next to text, or visual centering that differs from geometric centering.
- Border radii should follow a deliberate scale that feels related across components, not random per-component values.
- The overall system should feel like it was designed by someone with strong opinions about visual design — not assembled by someone following a checklist.

---

## MCP Server

This skill requires the **Figma remote MCP server**. Confirm it is connected before starting:
- Server URL: `https://mcp.figma.com/mcp`
- The user must authenticate via Figma's OAuth flow in their MCP client (Cursor: connect via Settings → MCP)
- Available tools via MCP: `use_figma`, `get_figma_data`, `get_metadata`, `get_screenshot`, `search_design_system`

If not connected, direct the user to: https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/

---

## Build Order (Critical — Never Deviate)

```
Phase 0 — DISCOVERY (no writes yet)
  0a. Read design-system-spec.md fully (including Sections 5 + 6)
  0b. Inspect target Figma file: pages, existing styles, variables, components
  0c. Search subscribed libraries (search_design_system) for reusable assets
  0d. Domain discovery: ask the user about their product type, core user
      flows, and any specific components they need beyond the base 14.
      Propose domain-specific components (justified by real user flows).
  0e. Creative direction: ask the user for brand personality keywords
      (e.g. bold, refined, playful, minimal, editorial, brutalist) or
      propose a creative direction if the user has no preference. Then:
      - Propose a distinctive typeface pairing with justification
        (why this combination suits the brand personality).
      - Propose a non-obvious primary color palette with reasoning
        rooted in color theory (not default blue/gray).
      - Present a mood / creative direction summary (personality,
        palette rationale, type rationale, spacing philosophy).
  0f. Lock scope: confirm exact token set + base 14 + domain-specific
      component list + creative direction with user
  ✋ CHECKPOINT: Present full plan including creative direction, await
     explicit approval

Phase 1 — FOUNDATIONS: Tokens & Styles
  1a. Color Styles (Color Palette — NOT variables)
      → Shades → Neutrals → Primary → Gradients → Error → Accents
  1b. Number Variables (Spacing + Container Dimensions)
      Collection "Spacing": 2xs, xs, sm, md, lg, xl, 2xl, 3xl, 4xl
      Collection "Dimensions": max-content-width, breakpoints, border-radius scale
  1c. Text Styles (Typography)
      → Header R/B → Body Large R/U/B → Body Medium R/U/B →
        Body Small R/B → Body XSmall R/B → Body XXSmall R/U/B → Micro R
  1d. Effect Styles (Elevation)
      → Level 0 (none) → Level 1 → Level 2 → Level 3 → Level 4
  → Validate: all styles/variables created, scopes set, code syntax set
  ✋ CHECKPOINT: Show token summary, await approval

Phase 2 — FILE STRUCTURE
  2a. Page skeleton: Cover → Getting Started → Foundations → --- → Components → --- → Utilities
  2b. Foundations docs: color swatches, type specimens, spacing scale bars, elevation demo
  ✋ CHECKPOINT: Show page list + screenshot

Phase 3 — COMPONENTS (parallel tiers, dependency order)

  Components are organized into dependency tiers. Within each tier,
  the coordinator spawns parallel sub-agents (via the Task tool) that
  build their assigned components simultaneously. Each sub-agent
  applies the full CoVE verification process per component (see the
  "CoVE-Based Component Verification" section below).

  Tier 1 — Atoms + Foundation-Only (no inter-component dependencies):
    Batch A (Sub-agent): Radio Button, Checkbox, Toggle, Chip
    Batch B (Sub-agent): Tooltip, Button, Input
    Batch C (Sub-agent): Accordion, Tile, Callout

    → Spawn 3 builder sub-agents in parallel using the Task tool
      (use the Component Builder Sub-Agent Prompt Template below)
    → Each sub-agent reads the master state ledger for foundation IDs
    → Each builds its components sequentially on dedicated pages,
      applying full CoVE per component
    → Each writes results to its own batch state file
    → Coordinator waits for all 3 to complete
    → Coordinator merges batch state files into master ledger
    → Coordinator spawns Image Analysis Sub-Agents for
      per-component re-verification AND cross-batch consistency
      checks (see "Image Analysis Sub-Agent" section below)
      — coordinator NEVER analyzes screenshots itself
    → Coordinator waits for verification sub-agents to complete
    → If defects found, coordinator spawns targeted fix sub-agents
    ✋ CHECKPOINT: Present verification reports + screenshots of
       all 10 Tier 1 components, batch approval

  Tier 2 — Molecules (depend on Tier 1 component instances):
    Batch D (Sub-agent): Dropdown (Input + Chip), Tabs (Button)
    Batch E (Sub-agent): Card (Button + Input + Callout + Chip),
                         Navigation (Button + Input)

    → Spawn 2 builder sub-agents in parallel
    → Each reads master ledger (now includes Tier 1 component IDs)
    → Each composes Tier 1 components via instance creation
    → Wait for both to complete
    → Coordinator merges batch state files into master ledger
    → Coordinator spawns Image Analysis Sub-Agents for
      per-component re-verification AND cross-batch consistency
      — coordinator NEVER analyzes screenshots itself
    → Coordinator waits for verification sub-agents to complete
    → If defects found, coordinator spawns targeted fix sub-agents
    ✋ CHECKPOINT: Present verification reports + screenshots of
       all 4 Tier 2 components, batch approval

  Tier 3 — Domain-Specific (Phase 0d additions):
    → Coordinator assigns domain-specific components to sub-agents
      based on their base-component dependencies
    → Components with the same deps go in the same batch
    → Spawn builder sub-agents in parallel
    → Coordinator merges batch state files into master ledger
    → Coordinator spawns Image Analysis Sub-Agents for
      per-component re-verification AND cross-batch consistency
      — coordinator NEVER analyzes screenshots itself
    → Coordinator waits for verification sub-agents to complete
    → If defects found, coordinator spawns targeted fix sub-agents
    ✋ CHECKPOINT: Present verification reports, batch approval

  Per component (executed by each sub-agent using the CoVE process):
    STEP 1 — CHAIN-OF-REASONING (Pre-Build Planning)
      a. Review the component spec from design-system-spec.md
      b. List every visual element that MUST be present
         (specific icons by name, label text, borders, fills,
          indicators per state)
      c. Identify ALL image containers in this component
         (hero images, thumbnails, avatars, media areas).
         For each, plan the picsum.photos seed URL and dimensions.
         See Section 8 of design-system-spec.md for the URL
         patterns and size table. If the component has zero image
         containers, note that explicitly.
      d. Identify the 3-5 highest-risk failure modes for THIS
         component type (e.g., Checkbox → inner check icon missing
         or outside box or stretched; Toggle → thumb distorted into
         oval; Card → image container blank)
      e. Trace dependencies: which foundation token IDs and icon
         SVGs are needed
      f. Plan the build steps and variant matrix size

    STEP 2 — INITIAL BUILD
      a. Create dedicated page
      b. Load heroicons-svg-reference.md, create needed icon
         components for this component
      c. Build base component with auto-layout + variable bindings
         (spacing, color, radius)
      d. Load placeholder images into ALL image containers
         identified in Step 1c. For each media area, avatar, or
         thumbnail frame, call figma.createImageAsync() with the
         planned picsum.photos seed URL and apply the image as a
         fill with scaleMode 'FILL'. Use different seeds per slot.
         Skip this step ONLY if Step 1c confirmed zero image
         containers for this component.
      e. Create all variant combinations via combineAsVariants,
         grid-layout result
      f. Add component properties (TEXT, BOOLEAN, INSTANCE_SWAP)
      g. Validate structure with get_metadata

    STEP 3 — GENERATE VERIFICATION QUESTIONS
      Generate 3-5 specific, falsifiable questions about THIS
      component. See "CoVE-Based Component Verification" below
      for requirements and examples.

    STEP 4 — INDEPENDENT VERIFICATION (Screenshot-Based)
      For each verification question, take a screenshot and answer
      SOLELY from what is visible — citing specific visual evidence.
      See "CoVE-Based Component Verification" below for the full
      evidence-based assertion format.

    STEP 5 — FINAL REVISED ASSESSMENT
      Compare verification answers against build intent. Fix any
      defects found, re-verify. Max 3 full CoVE cycles. If 3
      cycles exhausted → STOP, report to coordinator with
      screenshot + evidence.

Phase 4 — QA & INTEGRATION
  4a. Accessibility audit (contrast ratios, min touch targets 44×44px)
  4b. Naming audit (no duplicates, consistent casing)
  4c. Unresolved bindings audit (no hardcoded fills/strokes)
  4d. Final screenshots per page
  ✋ CHECKPOINT: Complete sign-off
```

---

## Token Quick Reference

Load [design-system-spec.md](references/design-system-spec.md) for exact definitions. Summary:

### Color Styles (not variables — Styles panel)
| Group | Contents |
|-------|----------|
| Shades | Pure white, pure black, dark/5%, dark/30% |
| Neutrals | 8-step scale, near-white → dark gray |
| Primary | Base + vivid variant |
| Gradients | 3+ stops for button states |
| Error | Light tint (backgrounds) + dark shade (text/icons) |
| Accents | Light tint, dark shade, success color, link color |

### Number Variables
| Collection | Tokens |
|------------|--------|
| Spacing | 2xs · xs · sm · md · lg · xl · 2xl · 3xl · 4xl |
| Dimensions | max-content-width · breakpoint/mobile · breakpoint/tablet · breakpoint/desktop · breakpoint/wide · radius/none · radius/sm · radius/md · radius/lg · radius/full |

### Text Styles
Header (R, B) · Body Large (R, U, B) · Body Medium (R, U, B) · Body Small (R, B) · Body XSmall (R, B) · Body XXSmall (R, U, B) · Micro (R)

### Effect Styles
Elevation 0 → 1 → 2 → 3 → 4 (no shadow → max shadow)

---

## Component Quick Reference

Load [design-system-spec.md](references/design-system-spec.md) for full anatomy, states, and variants. Summary:

| # | Component | Key Variants / States | Required Icons (Heroicons) |
|---|-----------|----------------------|---------------------------|
| 1 | Accordion | Closed · Open | `chevron-down` |
| 2 | Button | Primary · Secondary · Outline · Full-width · Default · Small · Default/Hover/Focused/Active/Loading/Disabled | `arrow-path` (loading) |
| 3 | Callout | Inline status · Bubble | `information-circle`, `exclamation-triangle`, `check-circle`, `tag`, `clock`, `currency-dollar` |
| 4 | Card | Media Card · Compact Media · Action Card · Summary Card | `heart`, `share`, `ellipsis-horizontal`, `chevron-left`, `chevron-right`, `bookmark`, `photo` |
| 5 | Dropdown | No label · With label · With leading icon · Default/Hover/Selected + Open menu | `chevron-down`, `check` |
| 6 | Checkbox | Standalone · With label · With label+description · Unchecked/Checked/Indeterminate · Default/Large | `check` (checked), `minus` (indeterminate) |
| 7 | Chip | Standard · Compact · Default/Hover/Focused/Muted/Selected | `x-mark` (removable) |
| 8 | Input | No label · With label · Floating label · Trailing icon · Textarea · Default/Hover/Focused/Filled/Error/Disabled | `eye`, `eye-slash`, `exclamation-circle`, `magnifying-glass`, `x-circle`, `calendar` |
| 9 | Tabs | Underline (icon+label or label, 2 sizes) · Pill · Default/Hover/Active/Disabled/Selected | `home`, `globe-alt`, `bookmark`, `squares-2x2` |
| 10 | Navigation | Top Navbar · Bottom Bar · Sidebar/Drawer · Breadcrumb · Mega Menu | `bars-3`, `magnifying-glass`, `bell`, `cog-6-tooth`, `user-circle`, `home`, `chevron-right` |
| 11 | Radio Button | Standalone · With label · With label+description · Unselected/Selected/Disabled | _(none — Ellipse node for inner dot)_ |
| 12 | Tile | Standard (icon+label) · Icon-only · Detail (icon+title+desc) · Default/Hover/Focused/Selected | `home`, `cog-6-tooth`, `chart-bar`, `document-text`, `star`, `globe-alt` |
| 13 | Tooltip | Light · Dark · Without title · With title · Arrow: Top/Bottom/Left/Right | `x-mark` |
| 14 | Toggle | Standalone · With label · With label+description · Off/On/Disabled Off/Disabled On | `check` (on-state thumb) |
| 15+ | Domain-specific | Identified during Phase 0d based on product type and user flows. Compose base components. | Per component spec |

---

## Key Rules (Enforced from figma-use / figma-generate-library)

### Plugin API rules
- **Colors 0–1 range**, not 0–255
- **Never `ALL_SCOPES`** — set explicit scopes per variable type
- **Semantic variables alias to primitives** — never duplicate raw values
- **INSTANCE_SWAP for icons**, never a variant per icon
- **Variant matrix ≤ 30** — split sub-component if exceeded
- **Sequential `use_figma` calls per agent** — a single agent never sends two `use_figma` calls at the same time. Across parallel sub-agents, the MCP server serializes concurrent requests safely.
- **Never hallucinate node IDs** — always read from state ledger
- **Validate before proceeding** — `get_metadata` after create, `get_screenshot` for CoVE verification after each component
- **All icons MUST be Heroicons SVGs** — created via `figma.createNodeFromSvg()` with actual SVG path data from [heroicons-svg-reference.md](references/heroicons-svg-reference.md). Never use Unicode symbols (`✓`, `▼`, `×`, `☰`, etc.), never use text nodes as icons, never leave blank/empty icon placeholder frames. Every icon slot in every component must contain a visible, correctly rendered Heroicons SVG vector.
- **Fixed sizing for graphical children in auto-layout** — When placing an SVG node (via `createNodeFromSvg()`), an Ellipse, or any fixed-dimension child inside a frame that has `layoutMode` set, the auto-layout engine will stretch the child along the cross-axis by default. After appending any such child to an auto-layout parent, you MUST explicitly set both sizing axes to `'FIXED'`:
  ```javascript
  parent.appendChild(child);
  child.layoutSizingHorizontal = 'FIXED';
  child.layoutSizingVertical = 'FIXED';
  ```
  Without this, circles become ovals, square icons become tall rectangles, and rounded squares become squircles. This applies to: inner dot Ellipse inside radio buttons, check/minus SVG icons inside checkboxes, check SVG inside toggle thumbs, the toggle thumb frame itself, and any SVG icon placed inside any auto-layout container throughout the system. The distortion only appears when a child node is appended and auto-layout sizes it — an empty frame with fixed dimensions keeps its shape.
- **Placeholder images are mandatory** — Every media container (card images, portfolio images, avatars, thumbnails) must display a real image loaded via `figma.createImageAsync()` from `https://picsum.photos/seed/{name}/{width}/{height}`. Never leave image areas as blank solid-color rectangles. Use different seed values per slot for variety. See Section 8 of design-system-spec.md for full details.

### Quality rules
- **Anti-patterns enforced** — Section 6 of design-system-spec.md lists banned patterns (no emojis, no Inter, no #000000, no fabricated data, Heroicons only, etc.). Load and enforce during every phase.
- **Elite creative quality is non-negotiable** — Every design decision (color, type, spacing, radius, shadow, proportion) must reflect the standard of an elite North American design agency. Default, safe, or generic choices are treated as defects. When in doubt, make the bolder creative choice. Refer to the "Creative & Quality Bar" section above for the full quality mandate.
- **Spacing tokens are mandatory — no hardcoded spacing values** — Every component MUST use the Number Variables declared in the "Spacing" collection (2xs, xs, sm, md, lg, xl, 2xl, 3xl, 4xl) for all padding, gaps, and margins. Hardcoded pixel values for spacing are treated as defects. Related components (e.g. Button, Input, Dropdown) must use the same spacing tokens for equivalent padding/gap roles to ensure visual rhythm across the system. During build, bind spacing via variable references; during verification, confirm spacing consistency both within a component's variants and across sibling components.

### Auto-layout & sizing pitfall rules

These rules address common Figma Plugin API layout failures that produce visual defects. Every builder sub-agent MUST internalize these before writing any `use_figma` code.

- **Background fill only paints the frame's own bounds, never its overflow** — If a container frame is `FIXED` height and its children overflow (because `clipsContent = false`), the frame's fill only paints its declared height — not the overflowing content area. This looks like an accidental colored stripe equal to the frame's padding or declared height. **Fix:** Set `primaryAxisSizingMode = 'AUTO'` (HUG) on the container so it grows to wrap content before applying background fills. Always ensure the frame's declared height matches or exceeds its content.

- **`combineAsVariants` freezes every variant at its current dimensions as FIXED** — `combineAsVariants` locks each variant's width and height to `FIXED` at whatever size the frame had at combine-time. Variants built with `HUG` sizing and short/empty placeholder text get frozen at that small size permanently. **Fix:** After every `combineAsVariants` call, immediately loop through all `compSet.children` and explicitly call `variant.resize(TARGET_W, TARGET_H)` + set `variant.primaryAxisSizingMode = 'FIXED'` on every variant to enforce uniform sizing before proceeding.

- **Never give both children `layoutSizingVertical = 'FILL'` in the same axis** — When two sibling children both use `FILL` on the same axis inside an auto-layout parent, Figma divides the available space equally between them. A label-above-field stack (e.g. Dropdown With Label, Input With Label) looks squashed — field is only half its expected height. Example: a 44px parent with two FILL children gives each only 20px. **Fix:** The label must be `layoutSizingVertical = 'FIXED'` at its text height (e.g. 20px). Only the field/control below it should use `'FILL'` to take the remaining space. Also size the parent variant frame tall enough: `label height + gap + field height` (e.g. 20 + 4 + 44 = 68px).

- **A rotated-square caret requires a diagonal-aware offset, not half the raw width** — A 10×10px square rotated 45° has a rendered bounding box of ~14×14px (diagonal = 10√2 ≈ 14.1px). Its visual center is 7px from any side of the bounding box, not 5px (half the raw width). Using `-5` places the caret too shallow into the tooltip body, causing inconsistent arrow positioning across variants. **Fix:** To place the caret center exactly at a frame edge, use `caret.x = edgeX - 7` and `caret.y = edgeY - 7`.

- **After adding taller content to a variant, always resize the component set and disable its clipping** — `combineAsVariants` sets the component set bounding box to its contents at combine-time and sets `clipsContent = true`. If you later add taller content to a variant (e.g. body text in an Open accordion, an expanded dropdown menu), the component set does not auto-grow — it stays at the original height and silently clips everything below. **Fix:** After any structural change to a variant, always: (1) set `compSet.clipsContent = false`, (2) check `compSet.height` against the tallest variant's `y + height`, (3) call `compSet.resize(w, newHeight)` to fully contain all variants.

- **Always set explicit padding on header/trigger frames; never rely on HUG for breathing room** — If a header frame (accordion trigger, dropdown header, collapsible) uses `primaryAxisSizingMode = 'AUTO'` (HUG) with no padding set, it collapses to exactly the text height (e.g. 20px) with zero breathing room. **Fix:** Always set `paddingTop`, `paddingBottom`, `paddingLeft`, `paddingRight` explicitly on trigger/header frames (e.g. 16px vertical, 20px horizontal), set `primaryAxisSizingMode = 'FIXED'`, and define an explicit height (e.g. 52px = 16 + 20 + 16). Also set `counterAxisAlignItems = 'CENTER'` for vertical centering of text and icons.

- **When applying a background color to a variant, clear white fills on child frames first** — Child frames with their own white `fills = [{type:'SOLID', color:{r:1,g:1,b:1}}]` paint on top of the parent frame's fill, blocking the background color in those regions. Only part of the component shows the intended background. **Fix:** Before or after setting `variant.fills`, walk all descendant `FRAME` nodes (excluding icon/SVG containers and dividers) and set `node.fills = []` on any that have a white or near-white solid fill (`r > 0.92 && g > 0.92 && b > 0.92`). This lets the parent background show through uniformly.

### CoVE verification rules
- **CoVE is mandatory per component** — after building each component, the full CoVE process (Steps 3-5) must be followed. Never skip to "looks good" or use a generic checklist.
- **Evidence-based assertions required** — every verification claim must follow the Question / Evidence / Counter-factual / Verdict format. Unsupported claims like "everything looks correct" are not acceptable.
- **Counter-factual checks required** — for every PASS verdict, describe what the failure would look like and confirm the screenshot does not match.
- **Coordinator MUST NEVER analyze screenshots directly** — all image analysis (per-component re-verification AND cross-batch consistency) is performed by dedicated Image Analysis Sub-Agents spawned via the Task tool. The coordinator reads their written verification reports and acts on PASS/FAIL verdicts, but never examines screenshots itself. This prevents the coordinator from rushing through visual checks to keep progress moving.
- **Image Analysis Sub-Agents are mandatory after every tier** — after each tier's builder sub-agents complete, the coordinator MUST spawn per-component re-verification sub-agents (one per batch) AND one cross-batch consistency sub-agent. These run in parallel. Skipping verification sub-agents to save time is treated as a critical process violation.
- **Double verification by design** — builder sub-agents perform their own CoVE (Steps 3-5) during the build. Then Image Analysis Sub-Agents re-verify each component with fresh eyes after the tier completes. This two-layer verification catches defects that the builder's confirmation bias missed.

### Parallel execution rules
- **State ledger uses per-batch files** — master ledger at `/tmp/dsb-state-{RUN_ID}.json` is written by the coordinator (Phases 0-2) and READ ONLY for sub-agents. Each sub-agent writes to `/tmp/dsb-state-{RUN_ID}-{BATCH_ID}.json`. Coordinator merges batch files into master between tiers.
- **Sub-agents MUST switch pages** — every `use_figma` script in a sub-agent MUST call `await figma.setCurrentPageAsync(page)` to its target page at the start. Page context resets between calls and other sub-agents may have switched pages between your calls.
- **Sub-agents MUST NOT modify foundations** — sub-agents have read-only access to Color Styles, Text Styles, Effect Styles, and Number Variables created in Phase 1. They reference these by ID from the master state ledger but never create, rename, or delete them.
- **Coordinator merges before next tier** — the coordinator MUST merge all batch state files into the master ledger before spawning the next tier's sub-agents. Tier 2 sub-agents need Tier 1 component IDs to create instances.
- **Tier checkpoints replace per-component checkpoints** — the user approves all components in a tier as a batch, not one at a time. The coordinator presents verification reports + screenshots of every component built in the tier together.
- **Image Analysis Sub-Agents are a separate spawned tier** — after builder sub-agents complete and state is merged, the coordinator spawns verification sub-agents before presenting to the user. These are separate Task tool invocations, not inline coordinator work. Verification sub-agents write results to `/tmp/dsb-verify-{RUN_ID}-*.json` files that the coordinator reads.
- **Coordinator is an orchestrator, not an analyzer** — the coordinator's job is: merge state, spawn sub-agents, read reports, present results to the user, and make proceed/fix/retry decisions. It never takes screenshots, never examines images, and never makes visual judgments itself.

---

## Heroicons SVG Implementation

Every icon in the design system MUST be a Heroicons SVG vector node — never a Unicode character, never a text node, never a blank frame.

### Mandatory steps for every component that uses icons

1. **Load the reference.** At the start of each component build (Step 2b), load [heroicons-svg-reference.md](references/heroicons-svg-reference.md) and identify which icons this component needs from the "Required Icons" column in the Component Quick Reference table above.

2. **Create icon components.** For each needed icon, create it as a Figma `ComponentNode` using the helper function from the reference file. This makes icons available for INSTANCE_SWAP properties.

```javascript
// Example: creating the check icon as a component
const checkSvg = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="black" stroke-width="1.5" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" d="m4.5 12.75 6 6 9-13.5"/></svg>`;
const checkComp = createHeroiconComponent(checkSvg, 'check', 24);
```

3. **Embed icons inside components.** Place icon instances or SVG nodes as children of the appropriate frame, centered via auto-layout. For stateful icons (checkbox check, toggle check, radio dot), the icon MUST be a child of the control's box/thumb frame — never floating outside or adjacent to it.

4. **Recolor after import.** SVGs import with black strokes. Use the `recolorIcon()` helper from the reference to change stroke colors (e.g. white for check icons inside filled checkboxes, primary color for active states).

5. **Size proportionally.** Icons inside small controls (checkbox, toggle thumb) should be sized proportionally — typically 12x12 or 16x16 inside 20x20 or 24x24 containers. Full-size icons (navigation, tile, input trailing) remain at 24x24 or 20x20.

### Stateful control icons — critical patterns

| Control | State | What goes INSIDE the control frame |
|---|---|---|
| Checkbox | Checked | White `check` SVG (12-16px), centered inside filled box |
| Checkbox | Indeterminate | White `minus` SVG (12-16px), centered inside filled box |
| Checkbox | Unchecked | Nothing — empty box with border only |
| Radio Button | Selected | Filled Ellipse node (10px), centered inside outer circle |
| Radio Button | Unselected | Nothing — empty circle with border only |
| Toggle | On | White `check` SVG (12px), centered inside thumb circle |
| Toggle | Off | Nothing — plain thumb circle |

**CRITICAL — Prevent auto-layout stretching:** After appending any child (SVG node or Ellipse) to the control's container frame, immediately set `child.layoutSizingHorizontal = 'FIXED'` and `child.layoutSizingVertical = 'FIXED'`. Also set these on the container frame itself (e.g. the toggle thumb, the checkbox box, the radio outer circle) after appending it to its own auto-layout parent. Failure to do this causes the child — and potentially the parent — to stretch along the cross-axis, turning circles into capsules and squares into distorted squircles.

---

## CoVE-Based Component Verification (Mandatory)

After each component is built (after Step 2 in the per-component flow), verification follows the Chain-of-Verification (CoVE) pattern. This replaces the old generic-checklist approach with structured, independent, evidence-based verification that resists confirmation bias.

### Why CoVE — not a checklist

The agent that wrote the code has a natural tendency to "see what it expects" when scanning a screenshot. CoVE breaks this anchoring by requiring:
1. Component-specific verification questions (not a fixed list)
2. Each question answered independently from visual evidence alone
3. Explicit citation of what is visible — position, color, shape
4. Counter-factual thinking: "what would the defect look like?"

### Step 3 — Generate Verification Questions

After the initial build (Step 2), generate 3-5 specific, falsifiable questions about THIS component. Requirements:

- Each question must be answerable ONLY by examining a screenshot — never by reviewing the code you wrote.
- Each must target a specific visual element or relationship, not a vague quality like "does it look good?"
- At least one question must test **cross-variant consistency** (e.g., "Are all checkbox boxes the same size across checked/unchecked/indeterminate variants?").
- At least one must target a **known anti-pattern** for this component type (reference the anti-pattern visual signatures table below).
- **If the component has ANY image containers** (hero images, thumbnails, avatars, media areas), at least one question MUST verify that every image container displays a real photograph (visual texture, colors, detail) rather than a flat solid-color rectangle. This is the most commonly missed defect.
- At least one question MUST verify **spacing consistency** — that padding, gaps, and margins within the component and across its variants use the declared spacing tokens and are visually uniform. Look for asymmetric padding, inconsistent gaps between label and field, or variants where the internal spacing rhythm breaks.
- At least one question MUST verify that **no variant appears chopped off, clipped, or partially hidden** — every variant's content (text, icons, nested elements, expanded states) must be fully visible within its bounds without any truncation, overflow hiding, or content cut-off.
- At least one question MUST verify that **no variant appears unnaturally shrunk** — every variant should render at its intended dimensions, not collapsed to a tiny/miniature size. This commonly occurs when `combineAsVariants` freezes a HUG-sized variant at a small placeholder size, or when auto-layout compresses children to near-zero height. A variant that is significantly smaller than its siblings in the grid is a defect.
- Phrase each question to make the FAILURE case concrete, forcing the verifier to look for both the correct and incorrect outcome.

**Example verification questions by component:**

| Component | Example verification question |
|---|---|
| Checkbox | "Is a white check SVG visible as a vector shape inside the filled blue box in the checked variant, OR is the box solid blue with no icon inside?" |
| Radio Button | "Is the inner dot in the selected state a perfect circle, OR has auto-layout stretched it into a vertical oval/capsule?" |
| Toggle | "Does the on-state thumb contain a white check SVG centered inside it, OR is the thumb a plain circle with no indicator?" |
| Button | "Does the loading state show a vector spinner icon replacing the label, OR does it show a Unicode character or the original label text?" |
| Card | "Does EVERY image container (hero image, compact thumbnail, summary thumbnail) display a real photograph with visible detail and color variation, OR are any of them flat solid-color rectangles with no visual texture?" |
| Navigation | "Does the user avatar area show a photograph or a user-circle SVG icon, OR is it a blank/empty circle or solid-color rectangle?" |
| Accordion | "Is the chevron a crisp SVG vector, OR a thin text-rendered Unicode character like ▼?" |
| Tooltip | "Does every variant (Light/Dark × Without title/With title × all 4 arrow directions) show a visible anchor element (e.g. a button or interactive control) positioned beside the tooltip bubble, OR are any variants missing the anchor entirely or showing only the floating tooltip with nothing to anchor to?" |
| Input | "Are all trailing icon slots populated with visible Heroicons SVGs, OR do any show as empty/blank rectangular frames?" |
| Tile | "Are all tile icons rendered as square shapes (equal width and height), OR has auto-layout stretched any icon into a tall or wide rectangle with visibly distorted proportions?" |
| Any icon-heavy component | "Do ALL SVG icons maintain their original square aspect ratio (e.g. 24x24, 20x20), OR have any been stretched into non-square rectangles by auto-layout? Look for icons whose strokes appear elongated or whose bounding boxes are visibly taller than wide (or vice versa)." |
| Domain-specific | "Do ALL media areas, thumbnails, and avatar slots in this component display loaded photographs from picsum.photos, OR are any showing as flat solid-color fills?" |
| Any component (spacing) | "Is the internal padding (top, bottom, left, right) visually uniform and consistent across ALL variants of this component, OR do some variants have noticeably tighter or looser spacing than others? Compare the gap between labels and fields, the padding around icons, and the margins between repeated elements." |
| Any component (clipping) | "Are ALL variants of this component fully visible — text, icons, borders, and nested content render completely within the variant's bounds — OR does any variant appear chopped off, truncated, or partially hidden at any edge (especially the bottom or right)?" |
| Accordion (clipping) | "In the Open variant, is the full body content (all text lines, nested elements) completely visible below the header, OR is the body text cut off or hidden below the component set's bottom edge?" |
| Dropdown (clipping) | "In the Open/Selected variants, is the full dropdown menu panel (all menu items, checkmarks, scroll area) completely visible, OR is the menu clipped at the bottom because the component set's bounding box was not expanded after adding the menu?" |
| Any component (shrinkage) | "Are ALL variants rendered at their intended full dimensions — matching or comparable to their sibling variants in the grid — OR do any variants appear unnaturally tiny/miniature, collapsed to a fraction of the expected size, with content compressed or barely visible?" |

### Step 4 — Independent Verification (Screenshot-Based)

For each verification question:

**4a. Take screenshots.**
- Take a full-page screenshot first (overview of the entire component page).
- For components with many variants (>8), take additional targeted screenshots of specific variant groups:
  - All "checked/selected/on" states grouped together
  - All "disabled" states grouped together
  - All "hover/focused/active" states grouped together
- This prevents small details from being lost in a zoomed-out full-page view.

**4b. Answer from the screenshot only.**
- Do NOT reference the code you wrote — treat the screenshot as the ONLY evidence.
- If a detail is too small to see clearly in the full-page screenshot, take a targeted screenshot of that region before answering.

**4c. Cite specific visual evidence (mandatory format).**
Every verification claim MUST follow this format:

```
Question: "Is the check icon visible inside the checkbox in the checked state?"
Evidence: "In the variant grid, row 1 column 2 (labeled 'Checked'), I observe
  a blue filled rounded-square with a white vector shape inside it. The shape
  has two angled strokes forming a checkmark pattern, approximately 12x12px
  within the 20x20px box."
Verdict: PASS — the check icon is present and correctly positioned.
```

Or for a failure:

```
Question: "Is the check icon visible inside the checkbox in the checked state?"
Evidence: "In the variant grid, row 1 column 2 (labeled 'Checked'), I observe
  a solid blue rounded-square with no visible content inside it. The box
  appears uniformly filled with no white vector shape."
Counter-factual match: YES — this matches the known failure mode where
  createNodeFromSvg() was called but the SVG node was not appended as a
  child of the box frame, or was appended but has zero opacity.
Verdict: FAIL — check icon is missing from the checked state.
```

**4d. Apply counter-factual check.**
For every PASS verdict, ask: "What would this look like if the defect IS present?" Then verify the screenshot does NOT match that failure description. This catches false positives where the agent glances at the screenshot and assumes correctness.

### Step 5 — Final Revised Assessment

```
a. Compare all verification answers against the build intent
b. List any contradictions between "what I intended" and "what I see"
c. If ANY verification question reveals a defect:
   → Generate a targeted fix script for ONLY the broken parts
   → Execute the fix
   → Return to Step 3 with NEW questions focused on the fixed area
     (do not re-ask questions that already passed)
   → Max 3 full CoVE cycles
d. If all pass → mark component complete, write IDs to batch state file
e. If 3 cycles exhausted and still broken:
   → STOP — report defects to coordinator with screenshot + evidence
   → Include: which questions failed, what evidence was observed,
     what fixes were attempted
```

### Anti-Pattern Visual Signatures

Pre-defined descriptions of what common defects LOOK like in screenshots. Reference these when generating verification questions and when performing counter-factual checks.

| Defect | What it looks like in a screenshot |
|---|---|
| Unicode icon instead of SVG | A text-rendered character (thin, antialiased differently from vectors, often slightly off-center and size-inconsistent with surrounding elements) instead of a crisp vector stroke with uniform weight |
| Auto-layout stretched circle | An ellipse/oval shape where a perfect circle was expected — taller than wide or wider than tall. The aspect ratio is visibly not 1:1 |
| Auto-layout stretched icon/rectangle | A square icon (e.g. 24x24) stretched into a tall or wide rectangle by auto-layout — the icon appears elongated along one axis, with strokes and shapes visibly distorted. Commonly affects SVG icons placed inside auto-layout containers without fixed sizing. The icon's bounding box is clearly not square |
| Missing inner indicator | A solid filled shape (checkbox box, radio circle, toggle thumb) with no visible icon or dot inside it — uniformly one color |
| Blank icon placeholder | An empty rectangular frame outline where an icon should be — no visible strokes or fills inside, just a thin gray border or nothing at all |
| Image container without image | A flat solid-color rectangle (usually gray or white) with sharp edges where a photograph should be — no visual texture or variation |
| Clipped text | Text that appears cut off mid-character at the bottom or right edge of its container, or text that runs into adjacent elements |
| Wrong color style | A color that doesn't match any swatch in the design system palette — e.g., pure black (#000) borders, generic untinted gray fills, or neon-bright accents |
| Squircle distortion | A rounded rectangle whose corner radii appear exaggerated or inconsistent — typically caused by auto-layout stretching the frame, making it look like a "squircle" instead of a subtly rounded square |
| Detached state indicator | An icon or dot that is visually adjacent to but clearly outside the control frame — e.g., a checkmark floating to the right of the checkbox box instead of centered inside it |
| Chopped-off / clipped variant | A variant whose content (text, icons, expanded body, menu panel) is abruptly cut off at the bottom or right edge — the component set's bounding box is too small and `clipsContent = true` hides the overflow. Often affects Open accordion, expanded dropdown, or any variant with dynamically taller content added after `combineAsVariants` |
| Background fill stripe | A thin horizontal band of color where a full-component background was intended — the frame's fill only paints its declared (FIXED) height while children overflow beyond it. Looks like an accidental colored stripe at the top of the component |
| Inconsistent spacing | Padding or gaps that are visibly uneven across variants of the same component — e.g., a Default variant has generous internal padding but the Hover variant appears cramped, or the gap between label and field differs between "No Label" and "With Label" variants of the same component |
| White child frame blocking background | A region of white within a component that should show a background color — caused by a child frame retaining a white fill that paints over the parent's background. Typically appears as a white rectangle covering part of the intended background |
| Unnaturally shrunk variant | A variant that is significantly smaller than its siblings in the component set grid — appearing as a tiny/miniature frame with compressed or barely visible content. Caused by `combineAsVariants` freezing a HUG-sized variant at its small placeholder dimensions, or auto-layout collapsing children to near-zero height |

### Common Defects by Component

Use these as input when generating verification questions (Step 3):

| Component | Highest-risk failure modes |
|---|---|
| Checkbox | Inner check/minus icon missing or outside the box; box stretched into squircle; large vs default size identical |
| Radio Button | Inner dot stretched into vertical capsule/oval; dot outside the circle; selected and unselected variants visually identical |
| Toggle | Thumb distorted into oval; check icon missing from on-state thumb; check icon overflowing/stretching beyond thumb bounds |
| Chip | Close/remove x-mark is a Unicode × text character instead of SVG; standard vs compact variants identical size |
| Tooltip | Close button is a text × instead of SVG x-mark; arrow direction not visible or pointing wrong direction; **tooltip anchor missing or not present across variants** — each variant must display an anchor element (e.g. a button or interactive control) to show the tooltip in context; verify the anchor is visible and consistent across all arrow-direction variants (Top/Bottom/Left/Right) and Light/Dark variants |
| Button | Loading spinner is a Unicode character or missing; hover/active states not visually distinct from default |
| Input | Trailing icon slots are empty/blank frames or **icons stretched into non-square rectangles**; floating label variant has no visible label; error state indistinguishable from default |
| Accordion | Chevron is a Unicode ▼ text node instead of SVG; open and closed states visually identical |
| Tile | Icon slots are blank frames; **icons stretched into tall rectangles by auto-layout** (should be square 24x24); default vs hover vs selected states not visually distinct |
| Callout | Status icons are blank frames or Unicode characters; **status icons stretched into non-square rectangles**; inline vs bubble variants structurally identical |
| Dropdown | Chevron is Unicode text; selected-item checkmark missing from menu panel; menu panel not visible |
| Tabs | Tab icons are blank frames or **stretched into tall/wide rectangles**; active/selected state has no underline or pill highlight |
| Card | **Image containers are flat solid-color rectangles instead of loaded photos from picsum.photos** (this is the single most common Card defect — every hero image, compact thumbnail, and summary thumbnail must show a real photograph); action icons (heart, share) are blank; pagination dots missing or uniform color |
| Navigation | Hamburger icon is manually drawn lines instead of bars-3 SVG; notification bell is blank; **user avatar area is a blank circle or solid rectangle instead of a loaded photo or user-circle SVG** |
| Any component with media areas | **Image containers left as flat solid-color rectangles** — the agent builds the frame structure correctly but forgets to call `figma.createImageAsync()` to load placeholder photos. This is the most frequently missed step across all components with image slots. |
| Accordion | **Open variant body content chopped off** — the component set was not resized after adding body text to the Open variant, so `clipsContent = true` hides everything below the original bounding box. Also: header/trigger frame has zero padding, making the label look squashed with no breathing room |
| Dropdown | **Open menu panel clipped or invisible** — menu items extend beyond the component set's bottom edge and are silently clipped. Also: "With Label" variant squashed because both label and input field use `layoutSizingVertical = 'FILL'` |
| Tooltip | **Caret/arrow inconsistently positioned across variants** — using raw width/2 offset instead of diagonal-aware offset for the rotated square causes some carets to be too shallow and others to appear misaligned |
| Any component (spacing) | **Inconsistent internal spacing across variants** — one variant uses generous padding while another of the same component type has cramped spacing, or gap between label and field varies across variants. Caused by hardcoded spacing values instead of shared spacing variable references |
| Any component (clipping) | **Variant content partially hidden** — text, icons, or nested content is cut off at the bottom/right edge of the variant frame or the component set bounding box. Especially common after `combineAsVariants` followed by content additions, or when the component set `clipsContent` was not disabled |

---

## Parallel Execution Architecture

Phase 3 uses parallel sub-agents to build components concurrently. The coordinator (the main agent) handles Phases 0-2 sequentially, then orchestrates sub-agents for Phase 3 tiers, then runs Phase 4 itself.

### How parallelism works with `use_figma`

The Figma plugin API is single-threaded and `use_figma` calls must be sequential. However, when multiple sub-agents send concurrent requests, the MCP server queues and serializes them — each script executes atomically and fully before the next begins. This is safe because:

1. Each script calls `setCurrentPageAsync()` to its own page at the start
2. Each script is self-contained (no cross-script page state assumptions)
3. Scripts return all created node IDs — no shared mutable state during execution

The real speedup comes from overlapping agent thinking time: while Sub-agent A is analyzing a screenshot and generating verification questions, Sub-agent B's `use_figma` call is executing. The LLM reasoning (reading specs, generating JS, analyzing screenshots, running CoVE) is the dominant cost per component, not the API call itself.

### State management across sub-agents

```
/tmp/dsb-state-{RUN_ID}.json           ← master ledger (coordinator writes Phases 0-2,
                                          sub-agents READ ONLY)
/tmp/dsb-state-{RUN_ID}-batchA.json    ← sub-agent A writes its created IDs
/tmp/dsb-state-{RUN_ID}-batchB.json    ← sub-agent B writes its created IDs
/tmp/dsb-state-{RUN_ID}-batchC.json    ← sub-agent C writes its created IDs
...
```

- Sub-agents read the master ledger at startup for foundation token IDs but NEVER write to it.
- Each sub-agent writes only to its own batch state file.
- After each tier completes, the coordinator merges all batch state files into the master ledger before spawning the next tier.
- For Tier 2+, sub-agents need component IDs from previous tiers — these are available in the merged master ledger.

### Tier checkpoints (replaces per-component checkpoints)

After each tier's builder sub-agents complete:
1. Coordinator merges batch state files into master ledger
2. Coordinator spawns **Image Analysis Sub-Agents** (see template below):
   - One sub-agent per batch for per-component re-verification
     (re-runs CoVE Steps 3-5 on each component's screenshot with
     fresh eyes — this agent did NOT build the component)
   - One sub-agent for cross-batch consistency checks
     (compares screenshots across all batches in this tier)
   - All verification sub-agents run in parallel
3. Coordinator waits for ALL verification sub-agents to complete
4. Coordinator collects verification reports (PASS/FAIL per
   component + cross-batch consistency verdicts)
5. If ANY component has FAIL verdicts:
   → Coordinator spawns targeted fix sub-agents for broken
     components, then re-spawns verification sub-agents for
     ONLY the fixed components (max 2 fix-verify cycles)
6. Coordinator presents verification reports + screenshots
   together for batch approval:
   - After Tier 1: "Here are all 10 atom components — approve before building molecules?"
   - After Tier 2: "Here are all 4 molecule components — approve before domain-specific?"
   - After Tier 3: "Here are all domain-specific components — approve?"
7. If user rejects any component, coordinator spawns fix sub-agents
   (never fixes directly — always delegates)

**CRITICAL: The coordinator MUST NEVER analyze screenshots directly.**
It reads verification reports from sub-agents, presents them to the user,
and makes orchestration decisions — but all visual inspection is performed
by dedicated Image Analysis Sub-Agents.

### Fallback and error recovery

If a sub-agent fails or a `use_figma` race condition is detected:
1. Coordinator retries the failed batch as a sequential single-agent build (no parallelism for that batch)
2. If the retry also fails, fall back to fully sequential mode for all remaining components in the tier
3. Log the failure pattern (error message, which batch, which component) for debugging
4. Never leave a tier half-built — either all batches in a tier complete or the coordinator finishes them sequentially

---

## Coordinator-Level CoVE (Cross-Batch Verification) — Delegated to Sub-Agents

After each tier's builder sub-agents complete and state files are merged, the coordinator **spawns dedicated Image Analysis Sub-Agents** for all verification. The coordinator NEVER analyzes screenshots itself — it only reads verification reports from sub-agents and acts on them.

### Why the coordinator must not analyze images directly

The coordinator has accumulated massive context (spec details, state ledger contents, prior build decisions) that creates pressure to rush through visual verification in order to keep progress moving. This leads to:
- Superficial "looks good" assessments that miss real defects
- Confirmation bias from the coordinator's knowledge of what was built
- Skipped counter-factual checks in the interest of speed
- Missed cross-component inconsistencies because the coordinator scans too quickly

Dedicated Image Analysis Sub-Agents have a single focused job: examine screenshots thoroughly. They have no build context to rush past, no next phase to hurry toward.

### What the coordinator spawns after each tier

```
After builder sub-agents complete and state is merged:

  1. Per-Component Re-Verification Sub-Agents (one per batch):
     → Each receives the list of components built by that batch
     → Each takes fresh screenshots and runs full CoVE Steps 3-5
       with fresh eyes (this agent did NOT build the components)
     → Each writes a verification report to:
       /tmp/dsb-verify-{RUN_ID}-{BATCH_ID}.json
     → These run in PARALLEL with the cross-batch sub-agent

  2. Cross-Batch Consistency Sub-Agent (one per tier):
     → Receives the full list of ALL components built in this tier
     → Takes screenshots of every component page
     → Generates 3-5 cross-component consistency questions:
       - "Do all components use the same border radius scale, or
         do some have ad-hoc radius values that don't match the
         foundation tokens?"
       - "Are icon sizes consistent across components? (24x24 for
         standard icons, 12-16px for small control indicators)"
       - "Do all hover states use the same visual language (color
         shift, border appearance, shadow change), or are they
         inconsistent across components?"
       - "Are all disabled states using the same opacity/muting
         approach (e.g., 0.4 opacity), or do some use a different
         disabled treatment?"
       - "Do color fills across all components reference the same
         Color Styles, or have some sub-agents used slightly
         different shades?"
     → Answers each question using the evidence-based format:
       Question / Evidence / Counter-factual / Verdict
     → Cites specific evidence from MULTIPLE components
       (e.g., "Button hover adds a border while Chip hover
        changes the fill — these use different hover languages")
     → Flags inconsistencies with component names + screenshot refs
     → Writes report to:
       /tmp/dsb-verify-{RUN_ID}-crossbatch-tier{N}.json

  3. Coordinator reads ALL verification reports:
     → If ALL pass → proceed to user checkpoint
     → If ANY fail → spawn targeted fix sub-agents, then
       re-spawn verification sub-agents for ONLY the fixed
       components (max 2 fix-verify cycles per tier)
     → Present final verification reports + screenshots to user
```

---

## Component Builder Sub-Agent Prompt Template

When the coordinator spawns a **builder** sub-agent via the Task tool, use this template. Replace placeholders in `{BRACES}` with actual values.

```
You are a Figma Design System Component Builder sub-agent.

## Your Assignment
Build these components in Figma: {COMPONENT_LIST}
Batch ID: {BATCH_ID}
Tier: {TIER_NUMBER}

## Prerequisites — Load These BEFORE Any use_figma Call
1. Load the figma-use skill (Plugin API rules)
2. Load the figma-generate-library skill (phase workflow, naming)
3. Read the design system spec: {WORKSPACE}/references/design-system-spec.md
4. Read the Heroicons SVG reference: {WORKSPACE}/references/heroicons-svg-reference.md

## Figma File
File key: {FILE_KEY}

## Foundation Token IDs (from Phase 1 — READ ONLY, do not modify)
{PASTE FULL CONTENTS OF MASTER STATE LEDGER HERE}

## For Tier 2+ Only — Component IDs from Previous Tiers
{PASTE COMPONENT IDS FROM MERGED MASTER LEDGER, OR "N/A" FOR TIER 1}

## Build Process (per component, in order)

For each component in your assignment, follow these steps:

STEP 1 — CHAIN-OF-REASONING (Pre-Build Planning)
  Before writing any code:
  a. Review the component spec from design-system-spec.md
  b. List every visual element that MUST be present
  c. Identify ALL image containers in this component (hero images,
     thumbnails, avatars, media areas). For each, plan the
     picsum.photos seed URL and dimensions. If zero image
     containers, note that explicitly.
  d. Identify the 3-5 highest-risk failure modes for THIS component
  e. Trace dependencies: which foundation token IDs and icon SVGs needed
  f. Plan build steps and variant matrix size

STEP 2 — INITIAL BUILD
  a. Create a dedicated page for the component
  b. Create needed Heroicon components from heroicons-svg-reference.md
  c. Build base component with auto-layout + variable bindings
  d. LOAD PLACEHOLDER IMAGES — For every image container identified
     in Step 1c, call figma.createImageAsync() with the planned
     picsum.photos seed URL and apply as an image fill with
     scaleMode 'FILL'. Use different seeds per slot. Example:
       const img = await figma.createImageAsync(
         'https://picsum.photos/seed/card1/600/400');
       frame.fills = [{ type: 'IMAGE', scaleMode: 'FILL',
         imageHash: img.hash }];
     Skip ONLY if Step 1c confirmed zero image containers.
  e. Create all variant combinations via combineAsVariants, grid-layout
  f. Add component properties (TEXT, BOOLEAN, INSTANCE_SWAP)
  g. Validate structure with get_metadata

STEP 3 — GENERATE VERIFICATION QUESTIONS
  Generate 3-5 specific, falsifiable questions about THIS component.
  - Each answerable ONLY by examining a screenshot
  - Each targets a specific visual element, not vague quality
  - At least one tests cross-variant consistency
  - At least one targets a known anti-pattern for this component
  - If the component has ANY image containers, at least one question
    MUST verify that every image area shows a real photograph (not
    a flat solid-color rectangle)
  - At least one MUST verify spacing consistency — that padding,
    gaps, and margins use the declared spacing tokens and are
    visually uniform across all variants
  - At least one MUST verify that NO variant appears chopped off,
    clipped, or partially hidden — every variant's content (text,
    icons, expanded states) must be fully visible within its
    bounds without truncation or overflow hiding
  - At least one MUST verify that NO variant appears unnaturally
    shrunk — every variant should render at its intended full
    dimensions, comparable to its siblings in the grid. A variant
    that is significantly smaller than others is a defect (often
    caused by combineAsVariants freezing HUG-sized placeholders)
  - Phrase each to make the FAILURE case concrete

STEP 4 — INDEPENDENT VERIFICATION (Screenshot-Based)
  For each verification question:
  a. Take a screenshot (get_screenshot) of the component page.
     For components with >8 variants, take additional targeted
     screenshots of specific variant groups.
  b. Answer SOLELY from what is visible in the screenshot.
     Do NOT reference the code you wrote.
  c. Cite specific visual evidence (mandatory format):

     Question: "{the question}"
     Evidence: "{describe exactly what you see — position in grid,
       colors observed, shapes observed, presence/absence of elements}"
     Counter-factual: "{describe what the defect would look like
       and confirm the screenshot does NOT match that description}"
     Verdict: PASS or FAIL — {one-line justification}

  d. For every PASS verdict, apply counter-factual check:
     "What would this look like if the defect IS present?"
     Verify the screenshot does NOT match the failure description.

STEP 5 — FINAL REVISED ASSESSMENT
  a. Compare all verification answers against build intent
  b. If ANY question reveals a defect:
     → Fix ONLY the broken parts
     → Return to Step 3 with NEW questions for the fixed area
     → Max 3 full CoVE cycles
  c. If all pass → component complete
  d. If 3 cycles exhausted → STOP, report with screenshot + evidence

## Anti-Pattern Visual Signatures (reference during verification)
| Defect | What it looks like |
|---|---|
| Unicode icon instead of SVG | Thin text-rendered character, antialiased differently from vectors, slightly off-center |
| Auto-layout stretched circle | Oval where a circle was expected, aspect ratio visibly not 1:1 |
| Auto-layout stretched icon/rectangle | Square icon stretched into a tall or wide rectangle — strokes and shapes visibly elongated along one axis, bounding box clearly not square |
| Missing inner indicator | Solid filled shape with no icon/dot inside — uniformly one color |
| Blank icon placeholder | Empty rectangular frame outline, no strokes/fills inside |
| Image container without image | Flat solid-color rectangle where a photo should be |
| Clipped text | Text cut off mid-character at container edge |
| Squircle distortion | Exaggerated/inconsistent corner radii from auto-layout stretching |
| Chopped-off / clipped variant | Variant content abruptly cut off at bottom or right edge — component set bounding box too small and clipsContent hides overflow |
| Background fill stripe | Thin band of color where a full background was intended — frame fill only paints its FIXED height while children overflow |
| Inconsistent spacing | Padding or gaps visibly uneven across variants — one variant cramped while another generous, or label-to-field gap varies |
| White child frame blocking background | White rectangle covering part of intended background color — child frame retains white fill over parent's background |
| Unnaturally shrunk variant | Variant significantly smaller than siblings in the grid — tiny/miniature frame with compressed or barely visible content, caused by combineAsVariants freezing HUG-sized variant at small placeholder dimensions |

## Critical Rules
- Colors 0-1 range, not 0-255
- Never ALL_SCOPES — explicit scopes per variable type
- INSTANCE_SWAP for icons, never a variant per icon
- Variant matrix ≤ 30 — split if exceeded
- Every use_figma script MUST call setCurrentPageAsync() to your
  target page at the start — page context resets between calls
- Do NOT create or modify any foundation tokens (Color Styles,
  Text Styles, Effect Styles, Number Variables) — read only
- All icons MUST be Heroicons SVGs via createNodeFromSvg()
- Fixed sizing for graphical children in auto-layout:
  child.layoutSizingHorizontal = 'FIXED';
  child.layoutSizingVertical = 'FIXED';
- PLACEHOLDER IMAGES ARE MANDATORY — Every image container (hero
  images, thumbnails, avatars, media areas) MUST display a real
  photo loaded via figma.createImageAsync() from picsum.photos.
  Never leave image areas as flat solid-color rectangles. This is
  the most commonly missed step — do it in Step 2d, not as an
  afterthought. Use different seed values per slot:
    const img = await figma.createImageAsync(
      'https://picsum.photos/seed/{name}/{width}/{height}');
    frame.fills = [{ type: 'IMAGE', scaleMode: 'FILL',
      imageHash: img.hash }];
- USE DECLARED SPACING TOKENS — All padding, gaps, and margins
  must reference the Spacing collection Number Variables (2xs, xs,
  sm, md, lg, xl, 2xl, 3xl, 4xl). Never hardcode spacing pixel
  values. Related components must use the same spacing tokens for
  equivalent padding/gap roles.
- AFTER combineAsVariants, resize every variant to uniform target
  dimensions and set primaryAxisSizingMode = 'FIXED'. The API
  freezes variants at their current size as FIXED.
- AFTER adding taller content to any variant post-combine, set
  compSet.clipsContent = false and resize the component set to
  fully contain all variants.
- Never give two sibling children FILL sizing on the same axis
  (e.g. label + field both FILL vertical). The label must be
  FIXED height; only the field uses FILL.
- Set explicit padding on header/trigger frames (accordion, dropdown).
  Never rely on HUG alone for breathing room — always set
  paddingTop/Bottom/Left/Right explicitly.
- When applying background fills to a variant, clear white fills
  on child FRAME nodes first (set fills = [] on any child with
  r > 0.92, g > 0.92, b > 0.92) so the parent background shows
  through uniformly.

## State Output
Write all created IDs (pages, components, icon components) to:
  /tmp/dsb-state-{RUN_ID}-{BATCH_ID}.json

Format:
{
  "batchId": "{BATCH_ID}",
  "tier": {TIER_NUMBER},
  "components": {
    "ComponentName": {
      "pageId": "...",
      "componentSetId": "...",
      "variantIds": ["..."],
      "iconComponentIds": {"iconName": "id", ...}
    }
  },
  "completedComponents": ["ComponentName1", "ComponentName2"],
  "failedComponents": [],
  "verificationLog": {
    "ComponentName": {
      "cycles": 1,
      "allPassed": true,
      "questions": ["...", "..."],
      "verdicts": ["PASS", "PASS"]
    }
  }
}

When all components in your assignment are complete (or stopped due
to max CoVE cycles), write the final state file and return a summary
of what was built, what passed verification, and what (if anything)
needs coordinator attention.
```

---

## Image Analysis Sub-Agent Prompt Templates

The coordinator spawns two types of image analysis sub-agents after each tier. These agents ONLY analyze screenshots — they never build or modify components. Their sole purpose is thorough, unbiased visual verification.

### Per-Component Re-Verification Sub-Agent

Use this template when spawning a sub-agent to re-verify components built by a specific batch. This agent did NOT build the components, so it has no confirmation bias.

```
You are a Figma Design System Image Verification Agent.

## Your Role
You ONLY analyze screenshots. You did NOT build these components.
Your job is to examine each component's visual output with extreme
thoroughness and report defects. You have no build context to rush
past — take your time and be meticulous.

## Your Assignment
Re-verify these components: {COMPONENT_LIST}
Batch ID: {BATCH_ID}
Tier: {TIER_NUMBER}
Figma file key: {FILE_KEY}

## Component Specifications (what each component SHOULD look like)
{PASTE RELEVANT SECTIONS FROM design-system-spec.md FOR EACH
 COMPONENT IN THE ASSIGNMENT}

## Anti-Pattern Visual Signatures (what defects look like)
| Defect | What it looks like |
|---|---|
| Unicode icon instead of SVG | Thin text-rendered character, antialiased differently from vectors, slightly off-center |
| Auto-layout stretched circle | Oval where a circle was expected, aspect ratio visibly not 1:1 |
| Auto-layout stretched icon/rectangle | Square icon stretched into a tall or wide rectangle — strokes and shapes visibly elongated along one axis, bounding box clearly not square |
| Missing inner indicator | Solid filled shape with no icon/dot inside — uniformly one color |
| Blank icon placeholder | Empty rectangular frame outline, no strokes/fills inside |
| Image container without image | Flat solid-color rectangle where a photo should be |
| Clipped text | Text cut off mid-character at container edge |
| Squircle distortion | Exaggerated/inconsistent corner radii from auto-layout stretching |
| Detached state indicator | Icon or dot visually adjacent to but clearly outside the control frame |
| Chopped-off / clipped variant | Variant content abruptly cut off at bottom or right edge — component set bounding box too small and clipsContent hides overflow |
| Background fill stripe | Thin band of color where a full background was intended — frame fill only paints its FIXED height while children overflow |
| Inconsistent spacing | Padding or gaps visibly uneven across variants — one variant cramped while another generous, or label-to-field gap varies |
| White child frame blocking background | White rectangle covering part of intended background color — child frame retains white fill over parent's background |
| Unnaturally shrunk variant | Variant significantly smaller than siblings in the grid — tiny/miniature frame with compressed or barely visible content, caused by combineAsVariants freezing HUG-sized variant at small placeholder dimensions |

## Common Defects by Component
{PASTE THE RELEVANT ROWS FROM THE "Common Defects by Component"
 TABLE FOR EACH COMPONENT IN THE ASSIGNMENT}

## Verification Process

For EACH component in your assignment:

### Step 1 — Take Screenshots
a. Take a FULL-PAGE screenshot of the component page
   (get_screenshot with the component page node ID).
b. For components with >8 variants, take ADDITIONAL targeted
   screenshots of specific variant groups:
   - All "checked/selected/on" states
   - All "disabled" states
   - All "hover/focused/active" states
c. For components with image containers (cards, navigation with
   avatars, etc.), take ZOOMED-IN screenshots of each image area
   to verify real photos vs solid-color rectangles.

### Step 2 — Generate Verification Questions
Generate 5-7 specific, falsifiable questions (MORE than the builder
agent's 3-5, because you are the dedicated verifier).

Requirements:
- Each question answerable ONLY from the screenshot
- Each targets a specific visual element or relationship
- At least one tests cross-variant consistency
- At least one targets a known anti-pattern for this component
- If the component has ANY image containers, at least TWO
  questions MUST verify image content (one for overview, one
  zoomed-in)
- At least one question tests spacing/padding consistency
  across variants — verify that declared spacing tokens are
  used uniformly and no variant has cramped or asymmetric spacing
- At least one question MUST verify that NO variant appears
  chopped off, clipped, or partially hidden — every variant's
  content (text, icons, expanded states, nested elements) must
  be fully visible within its bounds without truncation
- At least one question MUST verify that NO variant appears
  unnaturally shrunk — every variant should render at its
  intended full dimensions, comparable to its siblings. A
  variant significantly smaller than others is a defect.
- Phrase each to make the FAILURE case concrete

### Step 3 — Answer Each Question (Evidence-Based)
For EVERY question, use this MANDATORY format:

  Question: "{the question}"
  Evidence: "{describe EXACTLY what you see — position in grid,
    pixel-level detail, colors observed, shapes observed,
    presence/absence of elements, approximate dimensions}"
  Counter-factual: "{describe what the defect WOULD look like
    and confirm the screenshot does NOT match that description}"
  Verdict: PASS or FAIL — {one-line justification}

CRITICAL RULES:
- Spend at least 2-3 sentences on Evidence. Vague evidence like
  "it looks correct" is NOT acceptable.
- For every PASS, you MUST write the counter-factual. If you
  cannot describe what the failure would look like, change
  your verdict to UNCERTAIN.
- If a detail is too small to see clearly, take a zoomed-in
  screenshot BEFORE answering. Never guess.
- When in doubt, verdict is FAIL, not PASS.

### Step 4 — Write Verification Report
Write results to: /tmp/dsb-verify-{RUN_ID}-{BATCH_ID}.json

Format:
{
  "batchId": "{BATCH_ID}",
  "tier": {TIER_NUMBER},
  "verifierType": "per-component",
  "components": {
    "ComponentName": {
      "overallVerdict": "PASS" or "FAIL",
      "questionCount": 7,
      "passCount": 6,
      "failCount": 1,
      "questions": [
        {
          "question": "...",
          "evidence": "...",
          "counterFactual": "...",
          "verdict": "PASS" or "FAIL",
          "justification": "..."
        }
      ],
      "defectSummary": "One-sentence summary of any defects found",
      "screenshotsTaken": ["full-page", "checked-states", "zoomed-image-area"]
    }
  }
}

Return a summary of all verification results. For any FAIL verdicts,
include: which component, which question failed, what evidence was
observed, and a recommended fix action.
```

### Cross-Batch Consistency Sub-Agent

Use this template when spawning a sub-agent to check visual consistency across all components built in a tier. This catches systemic issues that no single per-component verifier can see.

```
You are a Figma Design System Cross-Batch Consistency Verifier.

## Your Role
You ONLY analyze screenshots for CROSS-COMPONENT CONSISTENCY.
You compare components built by DIFFERENT sub-agents to verify
they look like they belong to the same design system. You have
no build context — take your time and be meticulous.

## Your Assignment
Verify cross-component consistency for Tier {TIER_NUMBER}.
All components in this tier: {FULL_COMPONENT_LIST_FOR_TIER}
Figma file key: {FILE_KEY}

## Foundation Tokens (what all components should reference)
{PASTE COLOR STYLES, TEXT STYLES, EFFECT STYLES, SPACING VALUES
 FROM MASTER STATE LEDGER — these are the "source of truth" for
 visual consistency}

## Verification Process

### Step 1 — Take Screenshots of EVERY Component
For each component page built in this tier, take a full-page
screenshot using get_screenshot. You need ALL of them visible
to compare side-by-side.

### Step 2 — Generate Cross-Component Consistency Questions
Generate 5-7 specific questions that compare visual elements
ACROSS components. Required topics:

a. Border radius consistency:
   "Do all components use the same border radius scale (from
   foundation tokens), or do some have ad-hoc radius values?"

b. Icon sizing consistency:
   "Are icon sizes consistent? (24x24 standard, 12-16px for
   small control indicators like checkbox/toggle/radio)"

c. Hover/focus/active state language:
   "Do all hover states use the same visual treatment (e.g.,
   color shift + subtle shadow), or do some components use a
   different hover language?"

d. Disabled state treatment:
   "Are all disabled states visually consistent (same opacity,
   same muting approach), or do some components handle
   disabled differently?"

e. Color style adherence:
   "Do all component fills/strokes reference the same palette
   from Color Styles, or have some sub-agents used slightly
   different shades or hardcoded colors?"

f. Spacing token adherence:
   "Do all components use the declared Spacing collection tokens
   (2xs, xs, sm, md, lg, xl, 2xl, 3xl, 4xl) for their padding
   and gaps, or have some sub-agents hardcoded pixel values?
   Compare equivalent spacing roles across sibling components
   (e.g., Button, Input, and Dropdown should all use the same
   horizontal padding token for equal visual rhythm)."

g. Typography consistency:
   "Do all components use Text Styles from the foundation, or
   do some have hardcoded font sizes/weights?"

h. Variant completeness (no clipping or shrinkage):
   "Are ALL variants across ALL components fully visible at their
   intended dimensions — no content chopped off, clipped, or
   hidden at any edge, and no variant unnaturally shrunk to a
   tiny/miniature size compared to its siblings — or do any
   components have variants where content is truncated, or where
   a variant is significantly smaller than the others in its
   component set grid?"

### Step 3 — Answer Each Question (Evidence-Based)
For EVERY question, use this MANDATORY format:

  Question: "{the question}"
  Evidence: "{cite specific observations from AT LEAST 3
    different components — name each component and describe
    what you observe in each}"
  Counter-factual: "{describe what inconsistency would look
    like and confirm the screenshots do NOT match}"
  Verdict: CONSISTENT or INCONSISTENT — {justification}

For INCONSISTENT verdicts, specify:
  - Which components are consistent with each other
  - Which components diverge
  - What the divergence looks like
  - Recommended fix (which component should change to match)

### Step 4 — Write Verification Report
Write results to:
  /tmp/dsb-verify-{RUN_ID}-crossbatch-tier{TIER_NUMBER}.json

Format:
{
  "tier": {TIER_NUMBER},
  "verifierType": "cross-batch-consistency",
  "overallVerdict": "CONSISTENT" or "INCONSISTENT",
  "questionCount": 7,
  "consistentCount": 5,
  "inconsistentCount": 2,
  "questions": [
    {
      "question": "...",
      "evidence": "...",
      "counterFactual": "...",
      "verdict": "CONSISTENT" or "INCONSISTENT",
      "affectedComponents": ["ComponentA", "ComponentB"],
      "recommendedFix": "..."
    }
  ],
  "systemicIssues": [
    "One-sentence description of any pattern-level issue"
  ]
}

Return a summary of consistency results. For any INCONSISTENT
verdicts, include: which components diverge, what the divergence
looks like, and which component should be fixed to match the
majority / foundation tokens.
```

---

## Additional Resources

- Full design system specification: [design-system-spec.md](references/design-system-spec.md)
- Heroicons SVG reference (all icon SVG markup): [heroicons-svg-reference.md](references/heroicons-svg-reference.md)
- Plugin API rules: load `figma-use` skill
- Phase workflow & state management: load `figma-generate-library` skill
- Remote MCP server setup: https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/
