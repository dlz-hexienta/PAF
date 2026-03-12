---
title: "{Product} UI Design"
domain: "{domain}"
category: ui-design
status: draft
version: "0.1"
date: {date}
parent: "{root-architecture-filename}"
depends_on: ["{root-architecture-filename}"]
tags: []
---

# {Product} UI Design

## 1. Design Principles

<!-- What visual philosophy governs this product? (e.g., "dark-first precision",
     "clean minimalism", "data-dense operational") -->

- {principle}: {explanation}

---

## 2. Color System

### Surface Hierarchy
<!-- Background layers from deepest to most prominent -->

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-0` | {hex} | Page background |
| `--bg-1` | {hex} | Cards, panels |
| `--bg-2` | {hex} | Nested surfaces |
| `--bg-3` | {hex} | Hover states |
| `--bg-4` | {hex} | Active elements |

### Semantic Colors
<!-- Colors with meaning (accent, status, action) -->

| Token | Hex | Meaning |
|-------|-----|---------|
| `--accent-primary` | {hex} | {meaning} |
| `--accent-secondary` | {hex} | {meaning} |

### Status Colors
| Status | Hex | Usage |
|--------|-----|-------|
| Success/Green | {hex} | {when used} |
| Warning/Amber | {hex} | {when used} |
| Error/Red | {hex} | {when used} |

### Text Colors
| Token | Hex | Usage |
|-------|-----|-------|
| `--text-primary` | {hex} | Headings, important text |
| `--text-secondary` | {hex} | Body text |
| `--text-tertiary` | {hex} | Muted, labels |

### Border Colors
| Token | Hex | Usage |
|-------|-----|-------|
| `--border-primary` | {hex} | Dividers, card edges |
| `--border-subtle` | {hex} | Nested borders |

---

## 3. Typography

| Role | Font | Weight | Size |
|------|------|--------|------|
| Headings | {font} | {weight} | {sizes} |
| Body | {font} | {weight} | {size} |
| Code/Data | {font} | {weight} | {size} |

---

## 4. Spacing & Layout

### Spacing Scale
<!-- Base unit and scale (e.g., 4px base: 4, 8, 12, 16, 24, 32, 48) -->

### Grid System
<!-- Layout grid (e.g., sidebar + main content, responsive breakpoints) -->

### Responsive Breakpoints
| Breakpoint | Width | Layout Change |
|------------|-------|---------------|
| {name} | {px} | {what changes} |

---

## 5. Component Specifications

<!-- Each component the product uses. Include: visual spec, states, interaction. -->

### 5.1 Buttons
<!-- Variants (primary, secondary, ghost, danger), sizes, states (hover, active, disabled) -->

### 5.2 Forms & Inputs
<!-- Text inputs, selects, checkboxes, toggles, validation states -->

### 5.3 Cards & Panels
<!-- Surface components, padding, border radius -->

### 5.4 Tables
<!-- Data tables, sorting, pagination, row actions -->

### 5.5 Navigation
<!-- Sidebar, breadcrumbs, tabs -->

### 5.6 Modals & Dialogs
<!-- Overlay components, confirmation dialogs -->

### 5.7 Status Indicators
<!-- Badges, pills, progress bars, health indicators -->

---

## 6. Icons

**Library:** {icon library}
**Default weight:** {weight}
**Sizes:** {sidebar: Npx, inline: Npx}

---

## 7. Motion & Animation

| Property | Value | Usage |
|----------|-------|-------|
| Duration | {ms} | {standard transitions} |
| Easing | {curve} | {all transitions} |

---

## 8. Screen Layouts

<!-- [T2+] Each primary screen. Include: purpose, layout wireframe, key components. -->

### Screen: {Name}
**Purpose:** {what the user does here}
**Layout:** {description or wireframe reference}
**Key Components:** {which components from §5 appear here}

---

## 9. Design Anti-Patterns

<!-- What to explicitly avoid -->

- **NO:** {anti-pattern with rationale}

---

<!-- TIER GUIDANCE:
T2: Sections 1-7 required. §8 for primary screens only. §5 can be abbreviated.
T3: All sections required. §5 should be comprehensive. §8 for every screen.
-->
