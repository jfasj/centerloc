---
name: CENTERLOC Fleet & Equipment Design System
colors:
  surface: '#f6faff'
  surface-dim: '#d6dae0'
  surface-bright: '#f6faff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f0f4fa'
  surface-container: '#eaeef4'
  surface-container-high: '#e4e8ee'
  surface-container-highest: '#dfe3e9'
  on-surface: '#171c20'
  on-surface-variant: '#44474f'
  inverse-surface: '#2c3136'
  inverse-on-surface: '#edf1f7'
  outline: '#747780'
  outline-variant: '#c4c6d1'
  surface-tint: '#415e94'
  primary: '#002453'
  on-primary: '#ffffff'
  primary-container: '#1a3a6e'
  on-primary-container: '#89a5e0'
  inverse-primary: '#adc7ff'
  secondary: '#994700'
  on-secondary: '#ffffff'
  secondary-container: '#ff8934'
  on-secondary-container: '#662d00'
  tertiary: '#3d1d00'
  on-tertiary: '#ffffff'
  tertiary-container: '#5d2f00'
  on-tertiary-container: '#da965f'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#d8e2ff'
  primary-fixed-dim: '#adc7ff'
  on-primary-fixed: '#001a41'
  on-primary-fixed-variant: '#28467b'
  secondary-fixed: '#ffdbc8'
  secondary-fixed-dim: '#ffb68b'
  on-secondary-fixed: '#321300'
  on-secondary-fixed-variant: '#743400'
  tertiary-fixed: '#ffdcc3'
  tertiary-fixed-dim: '#ffb77e'
  on-tertiary-fixed: '#2f1500'
  on-tertiary-fixed-variant: '#6b3a0a'
  background: '#f6faff'
  on-background: '#171c20'
  surface-variant: '#dfe3e9'
typography:
  display-lg:
    fontFamily: Barlow Condensed
    fontSize: 44px
    fontWeight: '900'
    lineHeight: 48px
    letterSpacing: 0.02em
  headline-xl:
    fontFamily: Barlow Condensed
    fontSize: 32px
    fontWeight: '800'
    lineHeight: 36px
    letterSpacing: 0.02em
  headline-lg:
    fontFamily: Barlow Condensed
    fontSize: 24px
    fontWeight: '700'
    lineHeight: 28px
    letterSpacing: 0.01em
  headline-md:
    fontFamily: Barlow Condensed
    fontSize: 20px
    fontWeight: '700'
    lineHeight: 24px
    letterSpacing: 0.01em
  headline-sm:
    fontFamily: Barlow Condensed
    fontSize: 16px
    fontWeight: '700'
    lineHeight: 20px
    letterSpacing: 0.02em
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 24px
    letterSpacing: -0.01em
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
    lineHeight: 16px
    letterSpacing: 0em
  label-md:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '600'
    lineHeight: 18px
    letterSpacing: 0.01em
  label-sm:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '700'
    lineHeight: 14px
    letterSpacing: 0.05em
  code-sm:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.02em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  space-2xs: 0.125rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 0.75rem
  space-lg: 1rem
  space-xl: 1.5rem
  space-2xl: 2rem
  space-3xl: 2.5rem
  gutter-dense: 0.75rem
  gutter-normal: 1rem
  sidebar-width: 260px
  topbar-height: 64px
---

## Brand & Style

This design system powers enterprise rental, telematics, and heavy equipment logistics in the industrial hub of Recife, Brazil. The personality blends maritime corporate authority with high-visibility civil engineering utility. It prioritizes operational speed, unambiguous fleet statuses, high data density, and resilient clarity under intense field and dispatch office conditions.

The design movement is **Industrial Corporate / Precision B2B**:
- Structural layouts relying on crisp grid scaffolding, high-legibility telemetry, and strict visual hierarchy.
- Punchy, condensed industrial headers pairing with neutral, machine-grade body typography.
- High-contrast visual accents to denote asset allocation, maintenance alerts, and contract lifecycles.
- A functional balance of restrained navy navigation envelopes and high-visibility alert states.

## Colors

The system uses a high-contrast industrial utility palette tailored for enterprise operations:

- **Primary Navy (`#1A3A6E`)**: Anchors persistent structures—primary side navigation, module titles, data table column heads, and foundational brand marks.
- **Accent Orange (`#E87722`)**: The high-visibility machinery accent reserved for primary calls to action, selected row indicators, active tab bars, critical operational triggers, and focus halos.
- **Canvas Neutral (`#EEF2F8`)**: A cool, technical blue-gray base that minimizes glare during full-shift desk use while establishing contrast against white cards.
- **Surface & Borders (`#FFFFFF` / `#D4DFEE`)**: Data surfaces use pure white with a 1px solid structural border (`#D4DFEE`) to isolate equipment telemetry panels without visual heaviness.
- **Text Tiers**: `#1A273A` handles primary values, identifiers, and table data; `#64748B` covers secondary metadata, machine specs, timestamps, and column labels.
- **Operational Statuses**:
  - `Success (#16A34A)`: Available, returned, inspection passed.
  - `Warning (#F59E0B)`: Maintenance pending, reservation hold, overdue review.
  - `Danger (#DC2626)`: Vehicle grounded, contract breach, critical engine fault.
  - `Info (#2563EB)`: In transit, assigned, scheduled dispatch.

## Typography

The type scale combines **Barlow Condensed** (all-caps or title-case display for unit numbers, KPI metrics, fleet categories, and operational headers) with **Inter** for dense transactional tables, spec sheets, and form controls.

- Headings utilize Barlow Condensed's heavy weights (`700`, `800`, `900`) with uppercase transformations on major metric readouts and module labels to echo physical fleet stamping and logistics signage.
- Body copy relies on Inter at `14px` default for high-density enterprise efficiency, dropping to `12px` for auxiliary telemetry tags (chassis number, plate numbers, engine hours).
- Numerical values in tables and KPI modules should enable tabular figures (`font-variant-numeric: tabular-nums`) to ensure strict vertical alignment across multi-row data tables.

## Layout & Spacing

The layout is built for high-density dispatch and fleet management operations:

- **Structure**: A fixed-width left navigation rail (`260px`) clad in Primary Navy (`#1A3A6E`), combined with an adaptive main dashboard canvas (`#EEF2F8`) that stretches fluidly across standard enterprise displays (`1366px`, `1920px`, and ultrawide dispatch monitors).
- **Grid Architecture**: A 12-column grid utilizing tight `16px` gutters for dashboards and `12px` gutters inside modular side-drawers and inspection detail modals.
- **Data Density**: Table rows maintain a compact vertical rhythm of `40px` (dense) to `48px` (default), reserving generous padding exclusively for top-level summary KPI metrics.
- **Breakpoints**:
  - `Desktop Wide (>= 1440px)`: 4-column metric cards, persistent split-view panels for asset master-detail views.
  - `Desktop Standard (1024px - 1439px)`: 3 or 4-column metric cards, collapsable filter drawers.
  - `Tablet (768px - 1023px)`: Side nav collapses to an icon rail (`72px`), table horizontal scrolling with frozen ID columns.
  - `Mobile (< 768px)`: Bottom navigation or off-canvas sheet; data cards replace wide telemetry tables.

## Elevation & Depth

Visual hierarchy relies on structural containment and crisp borders rather than floating shadows:

- **Surface Separation**: Flat panels on canvas rely on a 1px border (`#D4DFEE`). Shadows are used sparingly to prevent visual fatigue on dense data interfaces.
- **Resting Cards**: Pure white (`#FFFFFF`) with a 1px solid border (`#D4DFEE`) and a minimal ambient drop: `0 1px 2px 0 rgba(26, 39, 58, 0.04)`.
- **Top Accent Stripe**: Key operational cards and KPI tiles feature a `4px` structural top accent border (Accent Orange `#E87722`, Primary Navy `#1A3A6E`, or the relevant semantic status color).
- **Floating Overlays & Menus**: Dropdowns, filter menus, and context sheets utilize `0 8px 16px -2px rgba(26, 39, 58, 0.12)`, anchored by a 1px border (`#D4DFEE`).
- **Active Modals**: Centered overlays with a heavy `#1A273A` backdrop at `50%` opacity and an elevated card shadow: `0 20px 25px -5px rgba(26, 39, 58, 0.20)`.

## Shapes

The design system maintains a disciplined, architectural geometric treatment:

- Standard structural elements (cards, containers, text fields, operational action buttons) use an exact `8px` (`0.5rem`) corner radius.
- Inner elements (table search inputs, micro-buttons, segmented switches, checkbox containers) scale down to a precise `4px` corner radius.
- Status badges and category chips diverge into complete pills (`rounded-full` / `9999px`) to create immediate tactile and visual distinction from rectangular action buttons and data cards.

## Components

### Buttons
- **Primary Action**: Background `#E87722`, text `#FFFFFF`, font Barlow Condensed `700`, uppercase, letter-spacing `0.03em`. Hover: `#D26717`. Active focus ring: `2px` solid `#E87722` with a `2px` offset (`#EEF2F8`).
- **Secondary Action**: Solid `#1A3A6E`, text `#FFFFFF`. Hover: `#12294E`.
- **Outline / Operational Button**: Background `#FFFFFF`, border `1px` solid `#D4DFEE`, text `#1A273A`. Hover: background `#EEF2F8`, border `#B9CBE3`.
- **Destructive**: Background `#DC2626`, text `#FFFFFF`. Hover: `#B91C1C`.

### Cards & Metric Panels
- Base container: Background `#FFFFFF`, border `1px` solid `#D4DFEE`, border radius `8px`.
- **KPI Metric Strip Cards**: Features a prominent `4px` top border stripe. Top stripe maps to:
  - Orange (`#E87722`): Active Fleet / Fleet Utilization
  - Navy (`#1A3A6E`): Total Asset Valuation
  - Red (`#DC2626`): Grounded / Maintenance Urgent
  - Green (`#16A34A`): In-yard Available
- Inside: Barlow Condensed `800` for the metric number (`32px` to `40px`), Inter `12px` uppercase medium for labels.

### Data Tables
- Header row: Background `#F8FAFC`, border bottom `2px` solid `#D4DFEE`, typography Barlow Condensed `700` uppercase in `#1A3A6E` with sort indicators.
- Row structure: Background `#FFFFFF`, alternating hover state `#F1F5F9`, border bottom `1px` solid `#EEF2F8`.
- Cell typography: Inter `13px`/`14px` `#1A273A`, numbers in monospace or tabular alignment.
- Row selection: Left `4px` vertical border in `#E87722` with an ambient tint background `#FFF7ED`.

### Badges & Status Pills
- Pill shape (`radius: 9999px`), padding `2px 10px`, typography Inter `11px` bold uppercase.
- **Available**: Background `#DCFCE7`, text `#15803D`, border `1px` solid `#86EFAC`.
- **Rented / Deployed**: Background `#DBEAFE`, text `#1D4ED8`, border `1px` solid `#93C5FD`.
- **Maintenance**: Background `#FEF3C7`, text `#B45309`, border `1px` solid `#FCD34D`.
- **Critical / Grounded**: Background `#FEE2E2`, text `#B91C1C`, border `1px` solid `#FCA5A5`.

### Input Fields & Controls
- Height `38px` (dense) to `42px` (standard).
- Background `#FFFFFF`, border `1px` solid `#D4DFEE`, border radius `8px`, text `#1A273A`, placeholder `#94A3B8`.
- Focus state: Border color `#E87722`, outline `2px` solid `#E87722`, outline offset `2px`.
- Checkboxes: `18px x 18px`, border `1.5px` solid `#64748B`, checked fill `#E87722` with white check glyph.

### Fleet Unit Tags
- Monospaced badge for machine serials and vehicle license plates (`e.g., PE-REC-4921`): Background `#1A3A6E`, text `#FFFFFF`, border radius `4px`, padding `2px 6px`, font size `11px`, font weight `700`.