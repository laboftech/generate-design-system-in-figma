# Generate Design System in Figma

Agent skill that builds a production-grade design system in Figma — Color Styles, Text Styles, Effect Styles, Number Variables, and 14 components with full variant/state coverage — using the remote Figma MCP server.

## Requirements

- Cursor with the [Figma plugin](https://cursor.com/marketplace/figma) installed

## Install

```bash
npx skills add laboftech/generate-design-system-in-figma
# or globally:
npx skills add laboftech/generate-design-system-in-figma -g
```

## Usage

> "Create a design system in Figma"

## How it works

4-phase workflow: Discovery, Foundations, File Structure, Components.

Components are built in **parallel tiers** using sub-agents — atoms with no dependencies run simultaneously (3 batches), then molecules that compose them (2 batches). Each component goes through **CoVE (Chain-of-Verification)** — the agent generates component-specific verification questions, answers them independently from screenshots with cited visual evidence, and applies counter-factual checks before proceeding.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill entry point — build order, CoVE process, parallel architecture, sub-agent template |
| `references/design-system-spec.md` | Full spec — anatomy, states, variants for every token and component |
| `references/heroicons-svg-reference.md` | SVG markup for all Heroicons used across components |
