---
name: AgentHub Modern Operations
colors:
  surface: '#faf8ff'
  surface-dim: '#d2d9f4'
  surface-bright: '#faf8ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f2f3ff'
  surface-container: '#eaedff'
  surface-container-high: '#e2e7ff'
  surface-container-highest: '#dae2fd'
  on-surface: '#131b2e'
  on-surface-variant: '#464555'
  inverse-surface: '#283044'
  inverse-on-surface: '#eef0ff'
  outline: '#777587'
  outline-variant: '#c7c4d8'
  surface-tint: '#4d44e3'
  primary: '#3525cd'
  on-primary: '#ffffff'
  primary-container: '#4f46e5'
  on-primary-container: '#dad7ff'
  inverse-primary: '#c3c0ff'
  secondary: '#505f76'
  on-secondary: '#ffffff'
  secondary-container: '#d0e1fb'
  on-secondary-container: '#54647a'
  tertiary: '#005338'
  on-tertiary: '#ffffff'
  tertiary-container: '#006e4b'
  on-tertiary-container: '#67f4b7'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#e2dfff'
  primary-fixed-dim: '#c3c0ff'
  on-primary-fixed: '#0f0069'
  on-primary-fixed-variant: '#3323cc'
  secondary-fixed: '#d3e4fe'
  secondary-fixed-dim: '#b7c8e1'
  on-secondary-fixed: '#0b1c30'
  on-secondary-fixed-variant: '#38485d'
  tertiary-fixed: '#6ffbbe'
  tertiary-fixed-dim: '#4edea3'
  on-tertiary-fixed: '#002113'
  on-tertiary-fixed-variant: '#005236'
  background: '#faf8ff'
  on-background: '#131b2e'
  surface-variant: '#dae2fd'
typography:
  headline-xl:
    fontFamily: Inter
    fontSize: 30px
    fontWeight: '700'
    lineHeight: 38px
    letterSpacing: -0.02em
  headline-xl-mobile:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: '700'
    lineHeight: 32px
    letterSpacing: -0.015em
  headline-lg:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: '600'
    lineHeight: 32px
    letterSpacing: -0.015em
  headline-md:
    fontFamily: Inter
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
    letterSpacing: -0.01em
  headline-sm:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
    letterSpacing: -0.005em
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
    letterSpacing: 0em
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 20px
    letterSpacing: 0em
  body-sm:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '400'
    lineHeight: 18px
    letterSpacing: 0em
  label-md:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '500'
    lineHeight: 18px
    letterSpacing: 0.01em
  label-sm:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.03em
  code-sm:
    fontFamily: JetBrains Mono
    fontSize: 12px
    fontWeight: '500'
    lineHeight: 16px
    letterSpacing: 0em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  gutter: 1.5rem
  gutter-mobile: 1rem
  margin: 2rem
  margin-mobile: 1rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2rem
---

## Brand & Style

This design system targets enterprise operators, technical project managers, and finance directors leasing and orchestrating autonomous digital workforces. The aesthetic avoids consumer-facing decorative flourishes, AI sci-fi tropes, glowing neon lines, and whimsical illustration. Instead, it prioritizes a surgical, calm, and high-legibility enterprise workspace built for sustained operational focus.

The visual style is rooted in **Modern Corporate Precision**:
- High data density calibrated for quick scanning without cognitive friction.
- Tactile, utilitarian controls grounded by balanced whitespace and structured dividers.
- Crisp neutral hierarchy utilizing slate tones to frame real-time analytical and tabular data.
- Strict functional color assignment: color is reserved exclusively for primary action triggers and mission-critical status indicators.
- All interface labeling, system messages, and operational terminology are localized in Spanish.

## Colors

The palette employs a cool, clinical slate neutral scale paired with an authoritative indigo primary accent and an unambiguous semantic traffic-light convention.

### Primary Accents
- **Indigo 600 (`#4f46e5`)**: Primary buttons, active navigation selections, focus rings, and primary interactive links.
- **Indigo 500 (`#6366f1`)**: Primary hover states, active toggle fills, focused border indicators.
- **Indigo 50 (`#eef2ff`)**: Primary subtle surface fills, selection highlights, active menu item backgrounds.

### Neutrals (Light Mode Default)
- **Canvas Background**: Slate 50 (`#f8fafc`).
- **Surface / Card Background**: Pure White (`#ffffff`).
- **Surface Hover**: Slate 100 (`#f1f5f9`).
- **Borders & Dividers**: Slate 200 (`#e2e8f0`).
- **Muted Text / Secondary Labels**: Slate 500 (`#64748b`).
- **Primary Body & Headings**: Slate 900 (`#0f172a`).

### Neutrals (Dark Mode Override)
- **Canvas Background**: Slate 950 (`#020617`).
- **Surface / Card Background**: Slate 900 (`#0f172a`).
- **Surface Hover**: Slate 800 (`#1e293b`).
- **Borders & Dividers**: Slate 800 (`#1e293b`).
- **Muted Text / Secondary Labels**: Slate 400 (`#94a3b8`).
- **Primary Body & Headings**: Slate 50 (`#f8fafc`).

### Semantic Status Scale
- **Activo (Operational / Running)**: Emerald 500 (`#10b981`) text/dot with Emerald 50 (`#ecfdf5`) background (Dark: Emerald 950/30).
- **Pendiente (Provisioning / Queued)**: Amber 500 (`#f59e0b`) text/dot with Amber 50 (`#fffbeb`) background (Dark: Amber 950/30).
- **Suspendido / Error (Critical / Terminated)**: Red 500 (`#ef4444`) text/dot with Red 50 (`#fef2f2`) background (Dark: Red 950/30).
- **Inactivo (Dormant / Archived)**: Slate 400 (`#94a3b8`) text/dot with Slate 100 (`#f1f5f9`) background (Dark: Slate 800).

## Typography

The type system relies exclusively on **Inter** to ensure micro-legibility in complex tabular layouts, metadata grids, and dense metric arrays. Monospaced elements (such as agent session keys, model identifiers, and token rates) utilize **JetBrains Mono**.

### Typographic Hierarchy Rules
- **KPI Figures & Metrics**: Rendered using `headline-xl` (desktop) and `headline-xl-mobile` (mobile) with semi-bold to bold weight and tight negative tracking (`-0.02em`).
- **Section & Table Headers**: Set to `headline-sm` with Slate 900 (`#0f172a`) in light mode and Slate 50 (`#f8fafc`) in dark mode.
- **Data Table Cells**: Base body cells utilize `body-md` (14px/20px) with tabular figures enabled via OpenType feature flags (`tnum`).
- **Pill Badges & Table Column Caps**: Set in `label-sm` with uppercase transformation and a subtle positive letter-spacing (`0.03em`) for instant categoric identification.

## Layout & Spacing

The layout is built on a responsive 12-column fluid grid governed by strict desktop, tablet, and mobile structural scaffolding.

### Structural Shell Specifications
- **Left Navigation Sidebar**:
  - *Desktop (≥1024px)*: Fixed width at 256px.
  - *Tablet (768px – 1023px)*: Collapses into a 64px compact icon rail with contextual tooltips on hover.
  - *Mobile (<768px)*: Completely off-canvas drawer controlled by a topbar hamburger trigger.
- **Top Navigation Bar**:
  - Sticky positioning with a strict height of 64px.
  - Horizontal padding aligns with the inner canvas margin: 32px (`space-xl`) on desktop, 16px (`space-md`) on mobile.
  - Backing surface uses Slate 50 / Slate 950 at 80% opacity with a hardware-accelerated 8px backdrop blur (`backdrop-blur-md`).
- **Main Canvas Grid**:
  - Desktop content uses a 12-column grid with 24px gutters (`gutter`).
  - Metric cards distribute across 4 columns (span 3), transitioning to 2 columns on tablet (span 6), and 1 column on mobile (span 12).
  - Main operational tables span the full 12 columns.

## Elevation & Depth

This design system minimizes dramatic dropshadows and eliminates high-spread ambient diffusion. Hierarchy is maintained through structural 1px borders, subtle tint differentials, and tight, low-altitude surface separation.

### Surface Tiers
- **Canvas (Level 0)**: Flat base layer. Slate 50 (`#f8fafc`) in light mode, Slate 950 (`#020617`) in dark mode.
- **Container / Card (Level 1)**: Pure White (`#ffffff`) or Slate 900 (`#0f172a`), bordered by a continuous 1px solid Slate 200 (`#e2e8f0`) / Slate 800 (`#1e293b`). Features a crisp micro-shadow: `0 1px 2px 0 rgba(15, 23, 42, 0.05)`.
- **Dropdown & Flyout Panels (Level 2)**: Elevated contextual surfaces with 1px solid Slate 200 / Slate 800 border and medium structural shadow: `0 4px 6px -1px rgba(15, 23, 42, 0.08), 0 2px 4px -2px rgba(15, 23, 42, 0.04)`.
- **Modal Dialogs (Level 3)**: High priority overlay surfaces with `0 20px 25px -5px rgba(15, 23, 42, 0.12), 0 8px 10px -6px rgba(15, 23, 42, 0.06)`. Surrounded by an assertive backdrop overlay of Slate 950 at 40% opacity with a 4px blur.

## Shapes

The geometric framework is calibrated to roundedness token `2` (Rounded):
- **Base Components (Inputs, Buttons, Dropdown items)**: 0.5rem (8px) corner radius.
- **Structural Containers & Metrics Cards**: 1rem (16px) corner radius (`rounded-xl`), creating distinct physical containers for grouped telemetry and tabular sections.
- **Pill Badges & Status Indicators**: Full pill radius (`9999px`) to immediately differentiate taxonomy and state flags from interactive rectangular inputs.
- **Checkboxes**: 0.25rem (4px) rounded corners for sharp, unambiguous selection states.

## Components

### Buttons
- **Primario**: Solid Indigo 600 (`#4f46e5`) background, White text, 8px radius, 10px 16px padding. Hover: Indigo 500 (`#6366f1`). Focus: 2px offset with Indigo 600 ring.
- **Secundario**: White surface, 1px Slate 200 border, Slate 700 text. Hover: Slate 50 background, Slate 900 text.
- **Peligro (Destructive)**: Red 50 background, 1px Red 200 border, Red 600 text. Hover: Red 600 background, White text.
- **Acción Rápida (⋮)**: 32x32px square hit area, 8px radius, transparent background. Hover: Slate 100 surface, Slate 600 icon color.

### Status Pill Badges
- **Dimensions & Layout**: Inline-flex, horizontal padding 8px, vertical padding 2px, 9999px radius.
- **Internal Indicator**: 6px solid circular dot preceding the label with a 6px margin-right.
- **State Specifications (Spanish)**:
  - *Activo*: Background Emerald 50, Text Emerald 700, Dot Emerald 500.
  - *Pendiente*: Background Amber 50, Text Amber 700, Dot Amber 500.
  - *Suspendido / Error*: Background Red 50, Text Red 700, Dot Red 500.
  - *Inactivo*: Background Slate 100, Text Slate 600, Dot Slate 400.

### Data Tables
- **Container**: Card enclosure with 16px radius, 1px Slate 200 border, zero internal padding around the table element.
- **Header Row**: 40px height, Slate 50 background, Slate 500 uppercase 11px font (`label-sm`), bottom border 1px Slate 200.
- **Data Rows**: 52px fixed height, regular tabular lining numbers, bottom border 1px Slate 100. Hover: Slate 50 surface transition.
- **Numeric & Metric Columns**: Right-aligned headers and cell contents with tabular zero-padded typography.

### Metrics Cards
- **Structure**: Surface White, 16px radius, 1px Slate 200 border, 20px uniform internal padding (`space-lg`).
- **Content Hierarchy**:
  1. Upper row: Muted label in `label-md` (Slate 500) paired with an optional 16px category icon.
  2. Value: `headline-xl` (Slate 900) displayed prominently.
  3. Delta badge: Inline pill showing percentage change (+12.4% vs. período anterior) colored according to positive (Emerald) or negative (Red) performance.

### Inputs & Filters
- **Form Controls**: 38px height, 8px radius, 1px Slate 200 border, Slate 900 text, 14px font size.
- **Placeholder**: Slate 400. Focus: Border switches to Indigo 600 with an ambient 3px Indigo 50 halo ring.
- **Checkbox & Radio**: 16x16px dimension. Checked state filled with Indigo 600 and a pure white glyph.

### Confirmation & Detail Modals
- **Placement**: Centered vertically and horizontally over viewport.
- **Backdrop**: Slate 950 at 45% opacity with 4px backdrop blur.
- **Surface**: White surface, 16px radius, max-width 540px, 24px padding (`space-xl`), 1px Slate 200 border.
- **Footer**: Right-aligned flex layout with 12px gap between "Cancelar" (Secundario) and "Confirmar Acción" (Primario or Peligro).