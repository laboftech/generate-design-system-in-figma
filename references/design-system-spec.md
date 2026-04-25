# Design System Specification

> This document defines the structure and components a design system must include.
> It is intended to be used as a generative prompt across multiple systems.

---

## 1. Color Palette

> **Figma:** All colors in this section must be created as **Color Styles** in Figma (not variables), so they are available in the Styles panel for reuse across components and assets.

### 1.1 Shades
- Pure white and pure black as the two anchors.
- Include semi-transparent variants of the dark shade (e.g. 5% and 30% opacity) for overlays, dividers, and subtle backgrounds.
- **Usage:** Backgrounds, text, dividers.

### 1.2 Neutrals
- A graduated scale of at least 8 steps ranging from near-white to dark gray.
- Each step should have a meaningful perceptual difference from its neighbors.
- **Usage:** Backgrounds, text, dividers, borders.

### 1.3 Primary
- Two closely related primary brand colors (e.g. a base and a slightly more saturated/vivid variant).
- **Usage:** Logos, icons, key interactive elements.

### 1.4 Gradients
- At least 3 gradient stops derived from the primary palette.
- **Usage:** Primary button states (default, hover, pressed).

### 1.5 Error
- Two error colors: one very light tint for error backgrounds, one dark saturated shade for error text/icons.
- **Usage:** Error states, validation messages.

### 1.6 Accents
- A light accent tint and a dark accent shade from the brand palette.
- A distinct color for success/positive states (e.g. confirmations, badges).
- A distinct color for links/interactive text.
- **Usage:** Icons, status badges, hyperlinks, highlighted labels.

---

## 2. Typography

> **Figma:** All type styles in this section must be created as **Text Styles** in Figma, so they are available in the Styles panel for consistent application across all text layers.

### 2.1 Type Scale
Define the following named text styles, each with regular and bold weight variants at minimum:

| Style | Variants |
|---|---|
| Header | Regular, Bold |
| Body Large | Regular, Underline, Bold |
| Body Medium | Regular, Underline, Bold |
| Body Small | Regular, Bold |
| Body XSmall | Regular, Bold |
| Body XXSmall | Regular, Underline, Bold |
| Micro | Regular |

### 2.2 Guidelines
- Each style should specify: font family, font size, line height, font weight, and letter spacing.
- Underline variant is required for styles used in interactive/link contexts.
- Micro text is the smallest legible size, used for labels, captions, and metadata.

---

## 3. Foundations

### 3.1 Spacing

> **Figma:** All spacing values in this section must be created as **Variables** (Number type) in Figma, so they can be bound directly to padding, gap, and margin properties on frames and components.

A consistent spacing scale used for padding, margins, gaps, and layout rhythm throughout the system.

- Define a base unit (e.g. 4px or 8px) and build a scale as multiples of that unit.
- The scale should cover at minimum: 2xs, xs, sm, md, lg, xl, 2xl, 3xl, 4xl steps.
- All component internal padding, gap between elements, and section spacing must reference tokens from this scale — never use arbitrary values.

### 3.2 Elevation (Drop Shadows)

> **Figma:** All elevation levels in this section must be created as **Effect Styles** in Figma, so they are available in the Styles panel and can be applied consistently to surfaces across the design system.

Elevation communicates the layering and depth of UI surfaces through drop shadows.

- Define at least 4–5 elevation levels, from flat (no shadow) to high (large, diffused shadow).
- Each level should specify: x-offset, y-offset, blur radius, spread, and color (typically a semi-transparent dark shade).
- **Usage guidelines:**
  - Level 0 — No shadow. Flat, inline elements.
  - Level 1 — Subtle shadow. Cards, dropdowns in resting state.
  - Level 2 — Moderate shadow. Popovers, date pickers, floating panels.
  - Level 3 — Prominent shadow. Modals, dialogs.
  - Level 4 — Maximum shadow. Toasts, overlays that sit above all other content.

### 3.3 Container Dimensions

> **Figma:** All dimension values in this section (max widths, breakpoints, border radii, and component-level sizes) must be created as **Variables** (Number type) in Figma, enabling consistent application and easy global updates across layouts and components.

Standardized width and height constraints that ensure consistent layout across breakpoints.

- **Max content width** — The maximum width a content area should span (e.g. page wrapper).
- **Breakpoints** — Define named breakpoints for responsive behavior: mobile, tablet, desktop, wide.
- **Component-level dimensions** — Key components (inputs, buttons, cards, modals) should have defined min/max widths and fixed heights per size variant.
- **Border radius scale** — A set of named radius tokens (none, sm, md, lg, full/pill) applied consistently across all components.

---

## 4. Components

### 4.1 Accordion
An accordion presents information in a compact, organized manner by allowing users to show or hide content within labeled sections.

**States:**
- **Closed** — Only the header label and a trailing chevron (down) icon are visible. Content is hidden.
- **Open** — Header and chevron remain; body content expands below. May include an inline link within the expanded content.

**Anatomy:**
- Header row (label + trailing chevron icon)
- Divider between items
- Expanded content area (body text + optional link)

**Behavior:**
- One or multiple items can be open simultaneously (configurable).
- Chevron rotates to indicate open/closed state.
- Items are stacked vertically with dividers separating them.

**Required Icons:** The trailing chevron MUST be a Heroicons `chevron-down` SVG (created via `figma.createNodeFromSvg()`). For the open state, rotate the chevron 180 degrees. Never use a Unicode character (e.g. `▼` or `▲`) or a text node as a chevron substitute.

---

### 4.2 Button
A button allows users to trigger an action or navigate when clicked.

**Types:**
- **Primary** — Brand-colored fill. Highest visual emphasis. Used for the main CTA.
- **Secondary** — Dark/black fill. Used for secondary actions requiring strong emphasis.
- **Outline / Ghost** — Bordered, no fill. Used for tertiary actions or alongside primary buttons. Supports a leading icon (e.g. social login).

**Sizes:**
- **Full-width** — Stretches to fill its container.
- **Default** — Fixed width, standard padding.
- **Small** — Compact, reduced padding and font size.

**States** (required for all types):
- Default
- Hover
- Focused
- Active (pressed)
- Loading (spinner replaces label)
- Disabled

**Anatomy:**
- Label text (centered)
- Optional leading icon + label
- Loading spinner (replaces content during loading state)

**Required Icons:** The loading spinner MUST be a Heroicons `arrow-path` SVG icon (created via `figma.createNodeFromSvg()`). For the Outline/Ghost variant with a leading icon, use an appropriate Heroicons SVG (e.g. a social logo or action icon). Never use Unicode characters or text nodes as icon substitutes.

---

### 4.3 Callout
A callout highlights or emphasizes specific information such as a warning, message, or important feature in a noticeable manner.

**Variants:**
- **Inline status callout** — Appears within a card or container. Displays a colored status label, a title, and supporting metadata.
- **Bubble callout** — Standalone container with bold lead text, supporting body text, and a trailing icon. Used for surfacing contextual highlights or promotional messages.

**Anatomy:**
- Status label (uppercase, accent-colored)
- Title / primary message
- Supporting text or metadata
- Optional trailing icon (from a defined set of callout icons)

**Callout Icons:**
A curated set of brand-styled outline icons used exclusively within callout contexts. Should cover concepts like value, time, and categorization.

**Required Icons:** All callout icons MUST be Heroicons SVGs (created via `figma.createNodeFromSvg()`): `information-circle` (info), `exclamation-triangle` (warning), `check-circle` (success), `tag` (categorization), `clock` (time), `currency-dollar` (value/pricing). Never use Unicode symbols, emoji, or blank placeholders.

---

### 4.4 Card
A card displays information and actions on a single topic in a compact, visually appealing format.

**Variants:**

**Media Card (Vertical)**

_Node hierarchy (outermost to innermost):_
```
Card Frame (vertical auto-layout, border-radius, elevation shadow)
├── Image Container (frame, clipped, fixed height ~200px)
│   ├── Image Placeholder (rectangle with neutral fill, or photo icon)
│   ├── Badge Overlay (optional, top-left — Chip component instance, absolute positioned)
│   └── Action Icons Row (optional, top-right — horizontal auto-layout, absolute positioned)
│       ├── Heart Icon (Heroicons `heart`, 20x20, white stroke)
│       └── Share Icon (Heroicons `share`, 20x20, white stroke)
├── Pagination Dots (optional, horizontal auto-layout, centered)
│   ├── Dot Active (small filled Ellipse, primary color)
│   └── Dot Inactive (small filled Ellipse, neutral color) ×2-4
└── Content Area (vertical auto-layout, padding from spacing tokens)
    ├── Title (text node, Body Large Bold text style)
    ├── Metadata Line (text node, Body Small Regular text style, neutral color)
    └── Secondary Metadata (optional, text node, Body XSmall Regular)
```

**Compact Media Card**

_Node hierarchy:_
```
Card Frame (horizontal auto-layout, border-radius, elevation shadow)
├── Image Thumbnail (frame, clipped, fixed width ~80px, square)
│   └── Image Placeholder (rectangle with neutral fill)
└── Content Area (vertical auto-layout, padding)
    ├── Title (text node, Body Medium Bold)
    └── Detail (text node, Body Small Regular, neutral color)
```

**Action Card**

_Node hierarchy:_
```
Card Frame (vertical auto-layout, border-radius, elevation shadow, padding)
├── Header Area (vertical auto-layout)
│   ├── Headline (text node, Body Large Bold)
│   └── Supporting Text (text node, Body Medium Regular)
├── Controls Area (vertical auto-layout, gap from spacing tokens)
│   ├── Input instance (from Input component)
│   └── Input instance (optional second field)
├── CTA Button (Button component instance, Primary, Full-width)
└── Helper Text (text node, Body XSmall Regular, neutral color)
```

**Summary Card (Compact)**

_Node hierarchy:_
```
Card Frame (horizontal auto-layout, border-radius, elevation shadow, padding)
├── Thumbnail (frame, clipped, fixed size ~48x48, border-radius)
│   └── Image Placeholder (rectangle with neutral fill)
└── Content Area (vertical auto-layout, fill width)
    ├── Title (text node, Body Medium Bold)
    ├── Metadata (text node, Body Small Regular, neutral color)
    └── Status Line (text node, Body XSmall Regular, accent color)
```

**Required Icons:** `heart`, `share`, `ellipsis-horizontal`, `chevron-left`, `chevron-right`, `bookmark`, `photo` — see `heroicons-svg-reference.md`

**Composition rule:** Card variants compose existing base components (Button, Input, Chip). Use component instances, not rebuilt internals.

---

### 4.5 Dropdown
A dropdown displays a list of options as a menu when triggered, allowing the user to choose one option from a list.

**Variants:**
- **Without label** — Field with placeholder text and trailing chevron icon.
- **With label** — Floating label sits above the field value.
- **With leading icon** — A thumbnail or icon precedes the placeholder/value text.

**States:**
- Default
- Hover / Focused (border highlight)
- Selected (value displayed)

**Open state (menu panel):**
- List of selectable items
- Checkmark on the currently selected item
- Grouped sections separated by dividers, each with a group label

**Required Icons:** The trailing chevron MUST be a Heroicons `chevron-down` SVG. The selected-item checkmark MUST be a Heroicons `check` SVG. Both created via `figma.createNodeFromSvg()`. Never use Unicode characters (`▼`, `✓`) or text nodes.

---

### 4.6 Checkbox
A checkbox allows the user to select or deselect one or multiple options from a set of choices.

**Variants:**
- **Standalone** — Checkbox only, no label.
- **With label** — Checkbox + label text.
- **With label and description** — Checkbox + label text + supporting description below the label.

**States:**
- Unchecked
- Checked
- Indeterminate

**Sizes:**
- Default
- Large

**Visual Anatomy (per state):**

_Unchecked:_
- The checkbox box is a rounded rectangle frame with a neutral border stroke and no fill.
- The box is empty — no icon inside.

_Checked:_
- The checkbox box is filled with the primary color and has no visible border (or a primary-colored border).
- A white Heroicons `check` SVG icon is placed **inside** the box frame as a child node, centered both horizontally and vertically.
- The check icon must be sized proportionally to the box (e.g. 12x12 inside a 20x20 box for Default size, 16x16 inside a 24x24 box for Large size).
- Use `figma.createNodeFromSvg()` with the `check` SVG from `heroicons-svg-reference.md`, then recolor its strokes to white `{r:1, g:1, b:1}`.

_Indeterminate:_
- The checkbox box is filled with the primary color (same as Checked).
- A white Heroicons `minus` SVG icon is placed **inside** the box frame as a child node, centered both horizontally and vertically.
- Use `figma.createNodeFromSvg()` with the `minus` SVG from `heroicons-svg-reference.md`, then recolor its strokes to white.

**Required Icons:** `check` (Heroicons), `minus` (Heroicons) — see `heroicons-svg-reference.md`

---

### 4.7 Chip
A chip represents a small, manageable piece of information or data, such as a tag, keyword, or selected item.

**Variants:**

**Standard Chip**
- Pill-shaped, displays a short label or value.
- States: Default (no border), Hover (border appears), Focused (stronger border), Muted/Light, Selected (dark fill + light text).

**Compact Chip**
- Rounded rectangle, optimized for dense or space-constrained contexts (e.g. grids, toolbars, data tables).
- Displays a short label or value.
- States: Default, Hover, Selected (dark fill), Disabled (muted), Selected with trailing icon (e.g. close/remove).

**Required Icons:** The removable/close trailing icon MUST be a Heroicons `x-mark` SVG (created via `figma.createNodeFromSvg()`). Never use a Unicode `×` character or a text node.

---

### 4.8 Input
A text input allows users to enter and edit text or data.

**Variants:**
- **Without label** — Field with placeholder text only.
- **With label** — Static label sits above the field.
- **With floating label** — Label animates upward when the field is focused or filled.
- **With trailing icon** — Icon inside the field on the right (e.g. clear, calendar, show/hide).
- **Textarea** — Multi-line input for longer text entry.

**States** (required for all variants):
- Default (empty)
- Hover
- Focused
- Filled (value entered)
- Error (error border + error message below the field)
- Disabled

**Anatomy:**
- Label (static or floating)
- Input field (border, background)
- Placeholder text
- Entered value text
- Optional trailing icon
- Helper / error message below the field

**Required Icons:** All trailing icons MUST be Heroicons SVGs (created via `figma.createNodeFromSvg()`): `eye` (show password), `eye-slash` (hide password), `exclamation-circle` (error state), `magnifying-glass` (search), `x-circle` (clear field), `calendar` (date picker). Never use Unicode symbols or text nodes as icon substitutes. Never leave trailing icon slots as empty/blank frames.

---

### 4.9 Tabs
Tabs allow the user to switch between multiple related views or sections within the same context.

**Variants:**

**Underline Tabs**
- Active tab indicated by an underline and bolder text.
- Supports icon + label, or label only.
- States: Default, Hover, Active/Selected, Disabled.
- Two size options (standard and compact).

**Pill Tabs**
- Tabs rendered as rounded pill shapes in a row.
- Active tab has a visible border/outline; inactive tabs have no border.
- Label only (no icons).
- States: Default (no border), Selected (border).

**Required Icons:** For the icon+label Underline Tabs variant, use representative Heroicons SVGs such as `home`, `globe-alt`, `bookmark`, `squares-2x2` (created via `figma.createNodeFromSvg()`). Every tab that shows an icon must contain an actual Heroicons SVG — never a blank frame or Unicode character.

---

### 4.10 Navigation
Navigation helps users move between different pages or sections of an application.

**Variants:**

**Top Navbar (Header Bar)**
A persistent horizontal bar anchored to the top of the viewport.

- **Leading slot** — Brand logo or wordmark.
- **Center slot** — Primary navigation links (horizontal text items, optionally with icons), or a search input, or left empty.
- **Trailing slot** — Utility actions such as icon buttons (e.g. search, notifications, settings), a text/button CTA, user avatar or profile trigger, and/or an overflow/hamburger menu.
- **Optional secondary row** — Sub-navigation links, breadcrumbs, contextual filters, or a banner.

States: Default, Scrolled/Compact (collapses or reduces height on scroll), Mobile (collapses center slot into a hamburger/drawer).

**Bottom Navigation Bar**
A fixed horizontal bar at the bottom of the viewport, primarily for mobile and compact layouts.

- 3–5 equally spaced items, each consisting of an icon and a short label.
- Active item indicated by color, fill change, or a top/bottom indicator.
- Optional notification badge (dot or count) on individual items.

States per item: Default, Active/Selected, Badge visible.

**Sidebar Navigation (Vertical Nav / Drawer)**
A vertical panel, either permanently visible or toggled as a drawer overlay, used for apps with deep or hierarchical information architecture.

- Optional header area (brand logo, collapse/expand toggle, or user profile summary).
- Navigation items, each with an optional leading icon, a label, and an optional trailing element (badge count, expand/collapse chevron).
- Collapsible groups with section headers or dividers.
- Optional footer area (settings link, sign-out, version info).

States per item: Default, Hover, Active/Selected, Disabled, Expanded group, Collapsed group.
Panel states: Expanded (full-width labels visible), Collapsed/Icon-only (icons only, labels hidden), Drawer/Overlay (slides in over content on mobile).

**Breadcrumb Navigation**
A horizontal trail showing the user's location within a hierarchical structure.

- A sequence of text links separated by a delimiter (slash, chevron, or arrow).
- The final item represents the current page and is non-interactive (plain text, bolder weight or muted style).
- Optional truncation with an ellipsis or overflow menu when the path is deep.

States per link: Default (interactive), Hover, Current (non-interactive).

**Mega Menu / Expanded Dropdown Navigation**
A large, multi-column panel that opens from a top navbar item, used when the navigation structure has many categories or groupings.

- Multiple columns of grouped links, each column under a category heading.
- Optional featured or highlighted item (e.g. a card, image, or promoted link) in one column.
- Full-width container anchored below the navbar, overlaying page content.

States: Closed (hidden), Open (visible on hover or click of the parent nav item).

**Behavioral Notes:**
- Real products typically combine multiple variants (e.g. top navbar + sidebar, top navbar + bottom bar on mobile).
- The spec intentionally does not prescribe what goes in each slot — the product's domain and user needs determine the content.
- Responsive behavior should be defined per variant: which slots collapse, which variants swap (e.g. top navbar links become a hamburger drawer on mobile).

**Required Icons:** All navigation icons MUST be Heroicons SVGs (created via `figma.createNodeFromSvg()`): `bars-3` (hamburger/mobile toggle), `magnifying-glass` (search), `bell` (notifications), `cog-6-tooth` (settings), `user-circle` (profile/avatar), `home` (home link), `chevron-right` (breadcrumb separator), `squares-2x2` (dashboard). The hamburger icon must be a `bars-3` SVG, never three horizontal lines drawn manually or Unicode `☰`. Never leave icon slots as blank frames.

---

### 4.11 Radio Button
A radio button allows the user to select exactly one option from a list of mutually exclusive choices.

**Variants:**
- **Standalone** — Radio control only, no label.
- **With label** — Radio + label text.
- **With label and description** — Radio + label text + supporting description below the label.

**States:**
- Unselected
- Selected
- Disabled (unselected)
- Disabled (selected)

**Visual Anatomy (per state):**

_Unselected:_
- The outer circle is an Ellipse node (e.g. 20x20) with a neutral border stroke (stroke weight ~2) and no fill (or a white/transparent fill).
- The interior is empty — no inner dot.

_Selected:_
- The outer circle is an Ellipse node with a primary-colored border stroke.
- A filled inner dot Ellipse (e.g. 10x10) is placed **inside** the outer circle frame as a child node, centered both horizontally and vertically.
- The inner dot is filled with the primary color and has no stroke.
- Use `figma.createEllipse()` for the inner dot — do NOT use an SVG icon or text character.

_Disabled (unselected):_
- Same as Unselected but with reduced opacity (e.g. 0.4) on the outer circle stroke.

_Disabled (selected):_
- Same as Selected but with reduced opacity (e.g. 0.4) on both the outer circle stroke and the inner dot fill.

**Required Icons:** None — uses native Figma Ellipse nodes for the inner dot, not SVG icons.

---

### 4.12 Tile
A tile represents a single, rectangular item in a grid or matrix, used to display related information such as icons and labels in a compact, selectable format.

**Variants:**

**Standard Tile (Icon + Label)**
- Rounded rectangle containing a centered icon and label below it.
- States: Default (no border), Hover (border), Focused (stronger border), Selected (prominent border or fill).

**Icon-Only Tile (Compact)**
- Small tile showing only an icon, used in horizontal strips for category or type selection.
- Same interactive states as the standard tile.

**Detail Tile (Icon + Title + Description)**
- Larger tile with an icon, a title, and a supporting description line below.
- Used for displaying options that need more context.

**Required Icons:** All tile icons MUST be Heroicons SVGs (created via `figma.createNodeFromSvg()`). Use representative icons like `home`, `cog-6-tooth`, `chart-bar`, `document-text`, `star`, `globe-alt` from `heroicons-svg-reference.md`. Every tile that specifies an icon slot must contain an actual visible Heroicons SVG — never a blank frame, empty rectangle, or Unicode character.

---

### 4.13 Tooltip
A tooltip displays additional contextual information about an element when the user hovers over or clicks it.

**Themes:**
- **Light** — White background, dark text.
- **Dark** — Dark/black background, light text.

**Variants (per theme):**
- Without title: body text only + close icon.
- With title: bold title + body text + close icon.

**Arrow directions:**
- Top, bottom, left, right — tooltip can be anchored from any side of its trigger element.

**Required Icons:** The close icon MUST be a Heroicons `x-mark` SVG (created via `figma.createNodeFromSvg()`). Never use a Unicode `×` character or a text node.

---

### 4.14 Toggle
A toggle allows the user to switch between two mutually exclusive states (on/off).

**Variants:**
- **Standalone** — Toggle control only.
- **With label** — Label text on the left, toggle on the right.
- **With label and description** — Label + supporting description on the left, toggle on the right.

**States:**
- Off
- On (with checkmark indicator inside the thumb)
- Disabled Off
- Disabled On

**Visual Anatomy (On state):**
- The toggle track is filled with the primary color.
- The thumb (circle) is positioned to the right side of the track.
- A Heroicons `check` SVG (12x12, white strokes) is placed **inside** the thumb circle as a child node, centered.
- Use `figma.createNodeFromSvg()` with the `check` SVG from `heroicons-svg-reference.md`, then recolor to white and resize to 12x12.

**Visual Anatomy (Off state):**
- The toggle track has a neutral fill.
- The thumb is positioned to the left side. No icon inside the thumb.

**Required Icons:** `check` (Heroicons) for the On-state thumb indicator — see `heroicons-svg-reference.md`. Never use a Unicode checkmark or text node.

---

## 5. Domain-Specific Components

The 14 base components above form a universal toolkit. Every real product also requires components tailored to its specific domain and user workflows.

### 5.1 Identification

During discovery, analyze the user's stated product type, target audience, and core user flows to determine which additional components are needed. Ask the user about:
- The category of app or website (e.g. marketplace, dashboard, content platform, social app, learning tool, financial service).
- The 3–5 most important user tasks or workflows.
- Any components they already know they need that are not covered by the base 14.

Based on the answers, propose a concrete list of domain-specific components. Each must be justified by a real user flow — never add speculative components.

### 5.2 Spec Format

Every domain-specific component must follow the same structure as the base 14:
- **Description** — One-sentence purpose statement.
- **Variants** — Named layout or behavioral variants.
- **States** — All interactive states (default, hover, focused, active, disabled, etc.).
- **Anatomy** — Slot-by-slot breakdown of what the component contains.

### 5.3 Composition Rule

Domain-specific components must compose base components wherever possible rather than building from scratch. For example, a domain-specific list item might compose the base Checkbox, Chip, and Button. Internal spacing, color, and radius must reference the same foundation tokens as the base components.

### 5.4 Build Order

Domain-specific components are built after all 14 base components, in dependency order (components with fewer base-component dependencies first). Each follows the same per-component workflow: dedicated page, base component with auto-layout and variable bindings, variant combinations, component properties, and validation checkpoint.

---

## 6. Anti-Patterns (NEVER DO)

These are hard constraints. Never violate them when generating any design system asset, component, documentation page, or placeholder content.

- No emojis anywhere.
- No Inter font.
- No generic serif fonts (Times New Roman, Georgia, Garamond) — distinctive modern serifs only if needed.
- No pure black (#000000).
- No neon/outer glow shadows.
- No oversaturated accents.
- No excessive gradient text on large headers.
- No custom mouse cursors.
- No overlapping elements — clean spatial separation always.
- No 3-column equal card layouts.
- No generic names ("John Doe", "Acme", "Nexus").
- No fake round numbers (99.99%, 50%).
- No fabricated data or statistics — never generate metrics, performance numbers, uptime percentages, response times, or any data that the user did not explicitly provide. Use clear placeholder labels like [metric] instead of making up numbers.
- No fake system/metric sections — dashboard cards filled with invented data are banned.
- No LABEL // YEAR formatting (e.g. "SYSTEM // 2024") — this is a lazy AI convention, not real design typography.
- No AI copywriting cliches ("Elevate", "Seamless", "Unleash", "Next-Gen").
- No filler UI text: "Scroll to explore", "Swipe down", scroll arrows, bouncing chevrons.
- No broken Unsplash links — use picsum.photos or SVG avatars.
- Icons must use Heroicons only — no other icon libraries or custom icon SVGs.
- Never use Unicode symbols as icons (`✓`, `✗`, `▼`, `▲`, `→`, `←`, `×`, `•`, `—`, `☰`, `⋯`). Every icon must be a Heroicons SVG vector created via `figma.createNodeFromSvg()` with actual path data from `heroicons-svg-reference.md`.
- Never leave blank icon placeholders — empty frames, invisible rectangles, or zero-opacity nodes where an icon is expected. Every icon slot defined in a component's anatomy must contain a visible, relevant Heroicons SVG.
- Never use text nodes to simulate icons. A text node containing a symbol character (e.g. a Text node with "×" or "✓") is not an icon — it will render incorrectly at different sizes and cannot be recolored via fill properties. Icons must always be vector/SVG nodes.

---

## 7. Icon Requirements per Component

Every component that uses icons must source them from Heroicons via `figma.createNodeFromSvg()`. The full SVG markup for each icon is in [heroicons-svg-reference.md](heroicons-svg-reference.md).

| Component | Required Heroicons | Notes |
|---|---|---|
| Accordion | `chevron-down` | Rotate 180deg for open state |
| Button | `arrow-path` | Loading/spinner state only |
| Callout | `information-circle`, `exclamation-triangle`, `check-circle`, `tag`, `clock`, `currency-dollar` | One per callout context |
| Card | `heart`, `share`, `ellipsis-horizontal`, `chevron-left`, `chevron-right`, `bookmark`, `photo` | Action overlays + image pagination |
| Checkbox | `check`, `minus` | Checked and indeterminate states — icon must be inside the box |
| Chip | `x-mark` | Removable/close variant only |
| Dropdown | `chevron-down`, `check` | Trigger chevron + selected item mark |
| Input | `eye`, `eye-slash`, `exclamation-circle`, `magnifying-glass`, `x-circle`, `calendar` | Trailing icon variants |
| Navigation | `bars-3`, `magnifying-glass`, `bell`, `cog-6-tooth`, `user-circle`, `home`, `chevron-right`, `squares-2x2` | Across all nav variants |
| Radio Button | _(none)_ | Uses native Figma Ellipse for inner dot |
| Tabs | `home`, `globe-alt`, `bookmark`, `squares-2x2` | Icon+label underline variant |
| Tile | `home`, `cog-6-tooth`, `chart-bar`, `document-text`, `star`, `globe-alt` | Representative set per tile |
| Toggle | `check` | On-state thumb indicator (12x12, white) |
| Tooltip | `x-mark` | Close button |

---

## 8. Placeholder Images

Any component or documentation element that represents a media area (card image containers, portfolio project images, team member avatars, hero sections, thumbnails) MUST display a real placeholder image — never a blank solid-color rectangle.

### 8.1 How to Load Images

Use `figma.createImageAsync()` with a picsum.photos URL to create placeholder images:

```javascript
const imageData = await figma.createImageAsync(
  'https://picsum.photos/seed/project1/600/400'
);
const imageHash = imageData.hash;

// Apply to a frame or rectangle as an image fill
node.fills = [{
  type: 'IMAGE',
  scaleMode: 'FILL',
  imageHash: imageHash
}];
```

### 8.2 URL Patterns

Use seeded URLs so images are deterministic (same image every time for the same seed):
- Card hero images: `https://picsum.photos/seed/{cardName}/600/400`
- Thumbnails: `https://picsum.photos/seed/{thumbName}/200/200`
- Avatars: `https://picsum.photos/seed/{personName}/100/100`
- Portfolio images: `https://picsum.photos/seed/{projectName}/800/600`

Use different seed values for each image slot so they don't all show the same photo.

### 8.3 Where to Apply

| Component / Element | Image Size | Seed Pattern |
|---|---|---|
| Card — Media Card image area | 600x400 | `seed/card1`, `seed/card2` |
| Card — Compact thumbnail | 200x200 | `seed/thumb1` |
| Card — Summary Card thumbnail | 200x200 | `seed/summary1` |
| Portfolio Card — project image | 800x600 | `seed/project1`, `seed/project2` |
| Team Member Card — avatar | 100x100 | `seed/team1`, `seed/team2` |
| Testimonial Block — avatar | 100x100 | `seed/client1` |
| Navigation — user avatar | 100x100 | `seed/avatar1` |
| Foundations — image placeholder demos | 400x300 | `seed/demo1` |

### 8.4 Rules

- Never leave media containers as flat solid-color rectangles — they look broken and unfinished.
- Always use `picsum.photos` with the `/seed/` pattern for deterministic results.
- Never use Unsplash direct links (they break).
- Use `scaleMode: 'FILL'` so images cover the container without distortion.
- Avatars may alternatively use a `user-circle` Heroicons SVG if the component spec calls for an icon-based avatar, but card/portfolio media areas must always have images.
- Use the `await` keyword — `figma.createImageAsync()` returns a Promise.
