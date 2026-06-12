<!-- make-kit-guidelines -->
## Design System Setup — MANDATORY

This project depends on `@make-kits/varicent-kit-2`, `@varicent/varicent-ui-core`, `@varicent/varicent-ui-data-grid`, `@varicent/varicent-ui-date`, `@varicent/varicent-ui-filter-form`, `@varicent/varicent-ui-gen-ai`, `@varicent/varicent-ui-icons`, `@varicent/varicent-ui-illustrations`, `@varicent/varicent-ui-select`, `@varicent/varicent-ui-tokens`, `@varicent/varicent-ui-tree` packages. Before writing any code:

1. Read `guidelines/setup.md` and `guidelines/Guidelines.md` by their exact path (e.g. `node_modules/<scope>/<package>/guidelines/setup.md`). This project uses pnpm, which symlinks packages — do NOT use `find`, `glob`, or `file_search` to discover files as they silently fail on symlinks. Instead use: reading files by exact path, `ls` (follows symlinks), `find -L` (`-L` follows symlinks), or `cat`. Also read `guidelines/component-notes.md` — it contains known component usage rules and fixes from real Make sessions. Follow every rule in that file.
2. Execute all setup instructions (install dependencies, config changes) against THIS project — not the package itself.
3. Do not skip, modify, or improvise any setup steps.
4. Read ALL other required .md files specified in guidelines/Guidelines.md:
   - `guidelines/icon-registry.md` — semantic action → verified icon export (80+ mappings)
   - `guidelines/component-patterns.md` — per-component battle-tested usage rules
   - `guidelines/component-notes.md` — session-learned fixes with Don't/Do examples
5. Verify that all packages specified in setup.md appear in this project's package.json and that all required .md files have been read before proceeding.
<!-- /make-kit-guidelines -->

# Design System Reference

### B2B Sales Performance Management Platform

---

## :zap: PRIORITY INSTRUCTIONS (read first)

1. Use ONLY token names listed in Section 2. Never use raw hex values.
2. Do NOT modify navigation (`SideNavigation` and `TopNavigation`) or global layout unless explicitly asked.
3. **ENSURE ALL tokens from Section 2 are defined as CSS custom properties in `/src/styles/theme.css`**
4. **Use ONLY real Varicent DS component names and packages** (Section 5). The DS has 109 components across 16 packages — never invent aliases like `Modal`, `DataTable`, or `EmptyState`.
5. **Never use shadcn/ui, lucide-react, radix-ui, or @mui components.** Always prefer the DS equivalent from Section 5.

## :clipboard: TABLE OF CONTENTS

1. Product Context
2. Color Tokens
3. Typography Scale
4. Spacing System
5. Component Library
6. Component Quick Reference
7. Layout Patterns
8. Rules for AI Generation
9. Icon Reference
10. Component Notes

---

## 1. PRODUCT CONTEXT

**Product type:** B2B SaaS — Sales Performance Management and Sales Planning  
**User type:** Power users (compensation admins, sales reps, managers, revenue ops)  
**Usage pattern:** High-frequency, data-heavy  
**Design priority order:** Clarity → Density → Speed

**DS catalog:** 109 components in `varicent-shell-2/ds-index.json`. Section 5 documents the 30 highest-frequency components for ICM mockups. For anything else, look up the real `name` and `pkg` in `ds-index.json` before importing.

**Import rule:** Components are split across packages. Always import from the package listed in Section 5 — not everything is in `@varicent/varicent-ui-core`.

```tsx
// Correct
import { Dialog, Button, NonIdealState } from '@varicent/varicent-ui-core';
import { ObjectTable } from '@varicent/varicent-ui-data-grid';
import { Select } from '@varicent/varicent-ui-select';
import { DateInput } from '@varicent/varicent-ui-date';
import { Search } from '@varicent/varicent-ui-icons';

// Wrong — these names/packages do not exist in the DS
import { Modal } from '@varicent/varicent-ui-core';
import { DataTable } from '@varicent/varicent-ui-data-grid';
import { Search } from '@carbon/icons-react';
import { Button } from '@/components/ui/button';
```

---

## 2. COLOR TOKENS

**IMPORTANT:** All token names in this section are CSS custom properties and MUST be defined in `/src/styles/theme.css` within the `:root {}` selector. Every variable listed below should be added to the theme file.

### Brand / Primary

| Token Name                | Value             | Usage                                 |
| ------------------------- | ----------------- | ------------------------------------- |
| `--intent-primary`        | rgb(43, 79, 244)  | Primary buttons, active states, links |
| `--intent-primary-hover`  | rgb(23, 55, 211)  | Hover on primary actions              |
| `--intent-primary-active` | rgb(8, 31, 145)   | Active on primary actions             |
| `--accent`                | rgb(211, 23, 133) | Secondary accents, highlights         |
| `--accent-hover`          | rgb(178, 26, 127) | Hover on secondary actions            |

### Text

Text variables are used to style various forms of text (default, headings, placeholders, etc.).

| Token Name | Value | Usage |
|---|---|---|
| `--text` | `rgb(77, 85, 128)` | Default body text color. |
| `--text-heading` | `rgb(48, 53, 80)` | Used for all heading levels. |
| `--text-placeholder` | `rgba(77, 85, 128, 0.83)` | Form input placeholders. |
| `--text-disabled` | `rgb(168, 168, 168)` | Disabled text states. |
| `--text-inverse` | `#fff` | Text on dark backgrounds. |

### Backgrounds

Background variables are used to style various backgrounds (default, midtone, overlay, etc.).

| Variable | Value | Usage |
|---|---|---|
| `--background` | `#fff` | Main page and surface background. |
| `--background-midtone` | `rgb(246, 246, 249)` | Secondary sections or card backgrounds. |
| `--background-overlay` | `rgba(48, 53, 80, 0.4)` | Modal and dialog backdrops. |
| `--background-disabled` | `rgb(232, 232, 232)` | Background for disabled elements. |
| `--background-inverse` | `rgb(48, 53, 80)` | Dark mode or high-contrast surfaces. |

### Borders

Border variables are used to style various borders and dividers.

| Variable | Value | Usage |
|---|---|---|
| `--border` | `rgb(221, 224, 238)` | Standard container and card borders. |
| `--border-divider` | `rgba(170, 177, 213, 0.5)` | Thin line separators. |
| `--border-input` | `rgb(128, 138, 186)` | Default form input border. |
| `--border-input-hover` | `rgb(111, 121, 168)` | Hover state for inputs. |
| `--border-input-active` | `rgb(77, 85, 128)` | Focused or active state for inputs. |

### Neutrals & Accents

Neutral variables are used for borders and backgrounds. Accents are used for primary elements like buttons.

| Variable | Value | Usage |
|---|---|---|
| `--neutral` | `rgb(111, 121, 168)` | Neutral UI elements |
| `--neutral-hover` | `rgb(77, 85, 128)` | Hover state for neutral elements |
| `--neutral-active` | `rgb(48, 53, 80)` | Active state for neutral elements |
| `--neutral-translucent-hover` | `rgba(111, 121, 168, 0.15)` | Subtle hover for very low-emphasis elements |
| `--neutral-translucent-active` | `rgba(111, 121, 168, 0.3)` | Active state translucent |
| `--accent` | `rgb(211, 23, 133)` | |
| `--accent-hover` | `rgb(178, 26, 127)` | |
| `--accent-active` | `rgb(151, 17, 120)` | |
| `--accent-translucent-hover` | `rgba(220, 106, 173, 0.15)` | |
| `--accent-translucent-active` | `rgba(202, 114, 165, 0.3)` | |

### Intents

Intent variables act as modifiers for primary, danger, success, and warning states.

| Variable | Value | Usage |
|---|---|---|
| `--intent-primary` | `rgb(43, 79, 244)` | Used in primary, default, and minimal buttons and their corresponding states; button-toggle, HTML links, menu items, certain icons, primary tags, file inputs, radio buttons (selected), checkboxes (checked and indeterminate), switches (default and hover) |
| `--intent-primary-hover` | `rgb(23, 55, 211)` | for cards, list items, table rows, buttons (secondary/ghost), and general interactive surfaces |
| `--intent-primary-active` | `rgb(8, 31, 145)` | |
| `--intent-primary-translucent-hover` | `rgba(163, 189, 255, 0.15)` | |
| `--intent-primary-translucent-active` | `rgba(157, 182, 251, 0.3)` | |
| `--intent-danger` | `rgb(196, 56, 49)` | For destructive actions (delete, remove, etc.) |
| `--intent-danger-hover` | `rgb(173, 38, 31)` | |
| `--intent-danger-active` | `rgb(134, 25, 19)` | |
| `--intent-danger-translucent-hover` | `rgba(255, 159, 154, 0.15)` | |
| `--intent-danger-translucent-active` | `rgba(255, 126, 119, 0.3)` | |
| `--intent-success` | `rgb(28, 125, 113)` | |
| `--intent-success-hover` | `rgb(24, 104, 104)` | |
| `--intent-success-active` | `rgb(19, 78, 83)` | |
| `--intent-success-translucent-hover` | `rgba(88, 187, 173, 0.149)` | |
| `--intent-success-translucent-active` | `rgba(123, 198, 189, 0.3)` | |
| `--intent-warning` | `rgb(236, 185, 34)` | |
| `--intent-warning-hover` | `rgb(208, 153, 27)` | |
| `--intent-warning-active` | `rgb(153, 102, 0)` | |
| `--intent-warning-translucent-hover` | `rgba(238, 208, 51, 0.15)` | |
| `--intent-warning-translucent-active` | `rgba(238, 208, 51, 0.3)` | |

### Gen AI

Tokens used for AI-specific components and branding.

| Variable | Value | Usage |
|---|---|---|
| `--ai-primary` | `rgba(121, 56, 224, 1)` | |
| `--ai-primary-hover` | `rgba(75, 24, 154, 1)` | |
| `--ai-primary-active` | `rgba(53, 17, 110, 1)` | |
| `--ai-translucent-hover` | `rgba(121, 56, 224, 0.15)` | |
| `--ai-translucent-active` | `rgba(121, 56, 224, 0.3)` | |
| `--ai-text` | `rgba(43, 14, 88, 1)` | |
| `--ai-midtone` | `rgba(243, 237, 252, 1)` | |
| `--ai-primary-gradient` | `linear-gradient(227.84deg, #9F2BD6 10%, #2B4FF4 100%)` | |
| `--ai-primary-gradient-hover` | `linear-gradient(252.35deg, #6F1D96 7.83%, #0A2CC7 92.17%)` | |
| `--ai-primary-gradient-active` | `linear-gradient(252.54deg, #6F1D96 7.81%, #071C7E 92.19%)` | |
| `--ai-gradient-light` | `linear-gradient(90deg, #FFFFFF -40.96%, #D2BDF5 152.41%)` | |

### Highlights

Used for group differentiation (tags, avatars, etc.).

| Variable | Value | Usage |
|---|---|---|
| `--highlight-01` | `rgb(74, 40, 113)` | |
| `--highlight-01-translucent` | `rgba(71, 0, 153, 0.1)` | |
| `--highlight-02` | `rgb(15, 67, 127)` | |
| `--highlight-02-translucent` | `rgba(0, 71, 153, 0.1)` | |
| `--highlight-03` | `rgb(2, 117, 88)` | |
| `--highlight-03-translucent` | `rgba(0, 204, 152, 0.1)` | |
| `--highlight-04` | `rgb(124, 103, 18)` | |
| `--highlight-04-translucent` | `rgba(204, 164, 0, 0.1)` | |
| `--highlight-05` | `rgb(150, 44, 141)` | |
| `--highlight-05-translucent` | `rgba(204, 0, 188, 0.1)` | |
| `--highlight-06` | `rgb(4, 109, 144)` | |
| `--highlight-06-translucent` | `rgba(0, 154, 204, 0.1)` | |
| `--highlight-07` | `rgb(66, 94, 46)` | |
| `--highlight-07-translucent` | `rgba(64, 153, 0, 0.1)` | |
| `--highlight-08` | `rgb(144, 81, 50)` | |
| `--highlight-08-translucent` | `rgba(204, 68, 0, 0.1)` | |
| `--highlight-09` | `rgb(122, 31, 88)` | |
| `--highlight-09-translucent` | `rgba(153, 0, 95, 0.1)` | |
| `--highlight-10` | `rgb(100, 82, 161)` | |
| `--highlight-10-translucent` | `rgba(48, 0, 204, 0.1)` | |
| `--highlight-11` | `rgb(3, 87, 94)` | |
| `--highlight-11-translucent` | `rgba(0, 141, 153, 0.1)` | |
| `--highlight-12` | `rgb(117, 72, 19)` | |
| `--highlight-12-translucent` | `rgba(153, 83, 0, 0.1)` | |

### 2.1. THEME FILE REQUIREMENTS

All color tokens listed in Section 2 must be added to `/src/styles/theme.css` as CSS custom properties in the `:root` selector.

**Required structure:**
- All variables use the exact token names shown (e.g., `--text`, `--intent-primary`, `--highlight-05`)
- No prefixes or suffixes added to token names
- Include all states: base, hover, active, and translucent variants
- Include all highlight colors (01-12)
- Include all intent colors (primary, success, warning, danger) with all variants

**Missing any of these variables will cause UI elements to lose their colors.**

### 2.2. TOKEN PACKAGE BRIDGE

The CSS custom properties in `/src/styles/theme.css` mirror the semantic values from `@varicent/varicent-ui-tokens`. In Make prototypes:

- **For custom layout CSS:** use `var(--token-name)` from `theme.css` (Section 2).
- **For Emotion/JS styling inside DS components:** prefer token exports from `@varicent/varicent-ui-tokens` when the component API accepts `css` props.
- **Do not invent parallel token names.** If a value is not in Section 2, look it up in `@varicent/varicent-ui-tokens` before adding it.

---

## 3. TYPOGRAPHY SCALE

**IMPORTANT:**
1. **Font family:** Inter
2. **Base size:** 14px
3. **Line height base:** 1.25

Use the `Text` component from `@varicent/varicent-ui-core` with `type` prop — do not hand-roll heading tags.

| Token Name    | Size              | Weight   | Usage                                                             |
| ------------- | ----------------- | -------- | ----------------------------------------------------------------- |
| `placeholder` | 14/17.5           | Italic   | For use in input fields only                                      |
| `label`       | 12/14.4           | Semibold | Label                                                             |
| `code`        | 14/17.5           | Regular  | Use display system message such as code or errors.                |
| `h1`          | 24/30             | Semibold | Page Title                                                        |
| `h2`          | 20/25             | Semibold | Column Titles                                                     |
| `h3`          | 16/20             | Semibold | Section titles                                                    |
| `h4`          | 16/20             | Semibold | Card title                                                        |
| `h5`          | 14/17.5           | Semibold | Paragraph Title                                                   |
| `p`           | 14/17.5           | Regular  | Paragraph                                                         |
| `dh1`         | Display Heading 1 | Semibold | High-impact headings for hero and marketing-like contexts         |
| `dh2`         | Display Heading 2 | Semibold | High-impact headings for hero and marketing-like contexts         |
| `pbold`       | Paragraph Bold    | Semibold | `type="p"` with bold styling (e.g., via `styleOverride` or `css`) |
| `desc`        | Description       | Italic   | `type="p"` with body/secondary style                              |

---

## 4. SPACING SYSTEM

Use these semantic tokens for internal component spacing, margins, and smaller layout gaps.

**Base unit:** 4px  
**Consistency:** Always prioritize these tokens over "hard-coded" pixel values.

| Token Name | Value | Usage                                   |
| ---------- | ----- | --------------------------------------- |
| `X-Small`  | 4px   | Tight groupings, icon/text alignment    |
| `Small`    | 8px   | Internal component padding, small gaps  |
| `Medium`   | 16px  | Section padding, card padding           |
| `Large`    | 32px  | Section grouping, larger component gaps |
| `XX-Large` | 64px  | Major vertical section breaks           |

### Layout Padding

These tokens define the structural breathing room for major containers and page sections.

| Token Name        | Value | Value              |
| ----------------- | ----- | ------------------ |
| `Wrapper`         | 16px  | Horizontal Padding |
| `Content-Section` | 16px  | Horizontal Padding |
| `Content-Section` | 24px  | Gap                |
| `Main-Content`    | 48px  | Vertical Padding   |

---

## 5. COMPONENT LIBRARY

**Source of truth:** `varicent-shell-2/ds-index.json` (109 components). This section covers the 30 highest-frequency ICM components. Use exact export names and packages below.

### Navigation & Shell

| Component | Package | Import |
|---|---|---|
| `SideNavigation` | `@varicent/varicent-ui-core` | `import { SideNavigation } from '@varicent/varicent-ui-core'` |
| `TopNavigation` | `@varicent/varicent-ui-core` | `import { TopNavigation } from '@varicent/varicent-ui-core'` |
| `CollapsibleSidebar` | `@varicent/varicent-ui-core` | `import { CollapsibleSidebar } from '@varicent/varicent-ui-core'` |
| `Breadcrumbs` | `@varicent/varicent-ui-core` | `import { Breadcrumbs } from '@varicent/varicent-ui-core'` |
| `Tabs` | `@varicent/varicent-ui-core` | `import { Tabs, Tab } from '@varicent/varicent-ui-core'` |

- `SideNavigation` — Primary app navigation with expandable sub-menus
- `TopNavigation` — Horizontal app bar at the top of the application
- `CollapsibleSidebar` — Secondary in-page navigation; resizable panel for filters or supplementary content
- `Breadcrumbs` — Location trail within a workflow
- `Tabs` — Section or view switcher within a page (use `Tab` children)

### Data Display

| Component | Package | Import |
|---|---|---|
| `ObjectTable` | `@varicent/varicent-ui-data-grid` | `import { ObjectTable } from '@varicent/varicent-ui-data-grid'` |
| `MetricCard` | `@varicent/varicent-ui-core` | `import { MetricCard } from '@varicent/varicent-ui-core'` |
| `Tag` | `@varicent/varicent-ui-core` | `import { Tag } from '@varicent/varicent-ui-core'` |
| `Text` | `@varicent/varicent-ui-core` | `import { Text } from '@varicent/varicent-ui-core'` |
| `Card` | `@varicent/varicent-ui-core` | `import { Card } from '@varicent/varicent-ui-core'` |
| `ObjectRow` | `@varicent/varicent-ui-core` | `import { ObjectRow } from '@varicent/varicent-ui-core'` |

- `ObjectTable` — **Primary data table pattern.** Sortable, filterable, paginated AG Grid wrapper. Use for all dense list views. **Not** `DataTable`.
- `MetricCard` — KPI and statistic cards for dashboards
- `Tag` — Status, stage, and category labels
- `Text` — All headings, labels, and body copy via `type` prop
- `Card` — Grouped content surface
- `ObjectRow` — Single object summary row outside a full table

### Forms & Inputs

| Component | Package | Import |
|---|---|---|
| `FormGroup` | `@varicent/varicent-ui-core` | `import { FormGroup } from '@varicent/varicent-ui-core'` |
| `Input` | `@varicent/varicent-ui-core` | `import { Input } from '@varicent/varicent-ui-core'` |
| `Select` | `@varicent/varicent-ui-select` | `import { Select } from '@varicent/varicent-ui-select'` |
| `DateInput` | `@varicent/varicent-ui-date` | `import { DateInput } from '@varicent/varicent-ui-date'` |
| `SearchInput` | `@varicent/varicent-ui-core` | `import { SearchInput } from '@varicent/varicent-ui-core'` |
| `Checkbox` | `@varicent/varicent-ui-core` | `import { Checkbox } from '@varicent/varicent-ui-core'` |
| `Switch` | `@varicent/varicent-ui-core` | `import { Switch } from '@varicent/varicent-ui-core'` |
| `FilterForm` | `@varicent/varicent-ui-filter-form` | `import { FilterForm } from '@varicent/varicent-ui-filter-form'` |
| `AddInput` | `@varicent/varicent-ui-tree` | `import { AddInput } from '@varicent/varicent-ui-tree'` |

- `FormGroup` — Wraps label, control, helper text, and validation
- `Input` — Single-line text entry
- `Select` — Single- and multi-select with optional search (`@varicent/varicent-ui-select`, not core)
- `DateInput` — Single date picker input (`@varicent/varicent-ui-date`). For ranges use `DateRangeInput` from the same package.
- `SearchInput` — Search field with clear affordance
- `Checkbox` / `Switch` — Boolean and toggle inputs
- `FilterForm` — Multi-criteria filter panel with apply/clear
- `AddInput` — Multi-select assignment from a searchable tree/list

### Feedback & Overlays

| Component | Package | Import |
|---|---|---|
| `Toast` | `@varicent/varicent-ui-core` | `import { Toast } from '@varicent/varicent-ui-core'` |
| `Dialog` | `@varicent/varicent-ui-core` | `import { Dialog } from '@varicent/varicent-ui-core'` |
| `Drawer` | `@varicent/varicent-ui-core` | `import { Drawer } from '@varicent/varicent-ui-core'` |
| `Tooltip` | `@varicent/varicent-ui-core` | `import { Tooltip } from '@varicent/varicent-ui-core'` |
| `NonIdealState` | `@varicent/varicent-ui-core` | `import { NonIdealState } from '@varicent/varicent-ui-core'` |
| `Spinner` | `@varicent/varicent-ui-core` | `import { Spinner } from '@varicent/varicent-ui-core'` |

- `Toast` — Non-blocking success, error, and info notifications
- `Dialog` — **Blocking confirmation and modal overlays.** **Not** `Modal`.
- `Drawer` — Side panel for contextual detail without leaving the page
- `Tooltip` — Supplementary label for icon buttons and truncated text
- `NonIdealState` — **Empty, error, and no-results states with CTA.** **Not** `EmptyState`.
- `Spinner` — Loading indicator for async regions

### Actions & Flow

| Component | Package | Import |
|---|---|---|
| `Button` | `@varicent/varicent-ui-core` | `import { Button } from '@varicent/varicent-ui-core'` |
| `DropdownMenu` | `@varicent/varicent-ui-core` | `import { DropdownMenu } from '@varicent/varicent-ui-core'` |
| `Stepper` | `@varicent/varicent-ui-core` | `import { Stepper } from '@varicent/varicent-ui-core'` |

- `Button` — Primary, secondary, tertiary, danger, and AI intents via `intent` + `priority`
- `DropdownMenu` — Overflow and contextual action menus
- `Stepper` — Multi-step flow progress (pair with `DialogWizard` for wizards)

### Common name corrections (do not use left column)

| Wrong (hallucinated) | Correct DS name | Package |
|---|---|---|
| `Modal` | `Dialog` | `@varicent/varicent-ui-core` |
| `DataTable` | `ObjectTable` | `@varicent/varicent-ui-data-grid` |
| `EmptyState` | `NonIdealState` | `@varicent/varicent-ui-core` |
| `TabBar` | `Tabs` | `@varicent/varicent-ui-core` |

---

## 6. COMPONENT QUICK REFERENCE

| Component | Package | Import | Key Props | When to use |
|---|---|---|---|---|
| `Button` | `@varicent/varicent-ui-core` | `import { Button } from '@varicent/varicent-ui-core'` | `intent`, `priority`, `loading`, `rightIcon`, `small` | Primary, secondary, danger, or AI actions |
| `SideNavigation` | `@varicent/varicent-ui-core` | `import { SideNavigation } from '@varicent/varicent-ui-core'` | `items`, `activeItemId`, `onItemSelect` | Persistent left app navigation |
| `TopNavigation` | `@varicent/varicent-ui-core` | `import { TopNavigation } from '@varicent/varicent-ui-core'` | `title`, `leftElement`, `rightElement` | Top app bar across the shell |
| `CollapsibleSidebar` | `@varicent/varicent-ui-core` | `import { CollapsibleSidebar } from '@varicent/varicent-ui-core'` | `isOpen`, `onResize`, `children` | In-page secondary nav, filters, or detail pane |
| `Breadcrumbs` | `@varicent/varicent-ui-core` | `import { Breadcrumbs } from '@varicent/varicent-ui-core'` | `items`, `onItemClick` | Wayfinding within a nested workflow |
| `Tabs` | `@varicent/varicent-ui-core` | `import { Tabs, Tab } from '@varicent/varicent-ui-core'` | `selectedTabId`, `onChange`, `vertical` | Switch views or sections on one page |
| `ObjectTable` | `@varicent/varicent-ui-data-grid` | `import { ObjectTable } from '@varicent/varicent-ui-data-grid'` | `columnDefs`, `rowData`, `height`, `footerPagination`, `draggableRows` | Dense sortable/filterable data lists |
| `MetricCard` | `@varicent/varicent-ui-core` | `import { MetricCard } from '@varicent/varicent-ui-core'` | `title`, `metrics`, `icon`, `tooltip`, `footer` | Dashboard KPIs and summary stats |
| `Tag` | `@varicent/varicent-ui-core` | `import { Tag } from '@varicent/varicent-ui-core'` | `intent`, `onRemove`, `children` | Status, stage, and filter chips |
| `Text` | `@varicent/varicent-ui-core` | `import { Text } from '@varicent/varicent-ui-core'` | `type`, `styleOverride`, `multiline` | All typography (headings, labels, body) |
| `Card` | `@varicent/varicent-ui-core` | `import { Card } from '@varicent/varicent-ui-core'` | `children`, `interactive`, `selected` | Grouped content blocks |
| `ObjectRow` | `@varicent/varicent-ui-core` | `import { ObjectRow } from '@varicent/varicent-ui-core'` | `title`, `description`, `rightElement` | Single object summary outside a grid |
| `FormGroup` | `@varicent/varicent-ui-core` | `import { FormGroup } from '@varicent/varicent-ui-core'` | `label`, `helperText`, `intent`, `children` | Label + input + validation grouping |
| `Input` | `@varicent/varicent-ui-core` | `import { Input } from '@varicent/varicent-ui-core'` | `value`, `onChange`, `intent`, `disabled`, `placeholder` | Single-line text fields |
| `Select` | `@varicent/varicent-ui-select` | `import { Select } from '@varicent/varicent-ui-select'` | `items`, `getItemId`, `getItemText`, `value`, `onChange`, `multiple` | Dropdown and searchable pick lists |
| `DateInput` | `@varicent/varicent-ui-date` | `import { DateInput } from '@varicent/varicent-ui-date'` | `value`, `onChange`, `shortcuts`, `disabledDates` | Single date entry |
| `SearchInput` | `@varicent/varicent-ui-core` | `import { SearchInput } from '@varicent/varicent-ui-core'` | `value`, `onChange`, `placeholder`, `onClear` | Toolbar and list search |
| `Checkbox` | `@varicent/varicent-ui-core` | `import { Checkbox } from '@varicent/varicent-ui-core'` | `checked`, `onChange`, `indeterminate`, `children` | Independent multi-select options |
| `Switch` | `@varicent/varicent-ui-core` | `import { Switch } from '@varicent/varicent-ui-core'` | `checked`, `onChange`, `intent`, `children` | On/off settings and toggles |
| `FilterForm` | `@varicent/varicent-ui-filter-form` | `import { FilterForm } from '@varicent/varicent-ui-filter-form'` | `selectValues`, `getItemId`, `getItemLabel`, `onApply`, `onClear` | Multi-filter panels with apply/clear |
| `AddInput` | `@varicent/varicent-ui-tree` | `import { AddInput } from '@varicent/varicent-ui-tree'` | `items`, `selectedItems`, `onItemSelect`, `multiple`, `search` | Assign multiple items from a tree/list |
| `Toast` | `@varicent/varicent-ui-core` | `import { Toast } from '@varicent/varicent-ui-core'` | `intent`, `message`, `onDismiss` | Transient action feedback |
| `Dialog` | `@varicent/varicent-ui-core` | `import { Dialog } from '@varicent/varicent-ui-core'` | `open`, `onOpenChange`, `title`, `children`, `footer`, `closable` | Blocking confirmations and forms |
| `Drawer` | `@varicent/varicent-ui-core` | `import { Drawer } from '@varicent/varicent-ui-core'` | `isOpen`, `onClose`, `title`, `size`, `children` | Side detail without leaving context |
| `Tooltip` | `@varicent/varicent-ui-core` | `import { Tooltip } from '@varicent/varicent-ui-core'` | `content`, `children`, `placement` | Icon-only control labels |
| `NonIdealState` | `@varicent/varicent-ui-core` | `import { NonIdealState } from '@varicent/varicent-ui-core'` | `icon`, `title`, `description`, `action` | Empty, filtered-empty, and error states |
| `Spinner` | `@varicent/varicent-ui-core` | `import { Spinner } from '@varicent/varicent-ui-core'` | `size` | Loading regions and button pending state |
| `DropdownMenu` | `@varicent/varicent-ui-core` | `import { DropdownMenu } from '@varicent/varicent-ui-core'` | `items`, `onItemSelect`, `maxHeight`, `closeOnSelect` | Row actions and overflow menus |
| `Stepper` | `@varicent/varicent-ui-core` | `import { Stepper } from '@varicent/varicent-ui-core'` | `steps`, `activeStepId`, `onStepClick` | Wizard and multi-step progress |

---

## 7. LAYOUT PATTERNS

### Dashboard Layout

```
[TopNavigation - full width]
[SideNavigation][CollapsibleSidebar] | [MetricCard row]
                                     | [ObjectTable - full width]
```

### Detail Layout

```
[TopNavigation]
[SideNavigation][CollapsibleSidebar] | [Detail header + Tag + actions]
                                     | [Tabs]
                                     | [2-col: Form fields LEFT | activity RIGHT]
```

### Settings Layout

```
[TopNavigation]
[SideNavigation][CollapsibleSidebar] | [Settings category nav LEFT]
                                     | [Form sections with Divider RIGHT]
```

---

## 8. RULES FOR AI GENERATION

When generating any screen for this design system, follow these rules without exception:

1. **Use only components listed in Section 5 and Section 6.** Do not create bespoke components when a DS equivalent exists.
2. **Use only color tokens from Section 2.** No raw hex values in component or layout CSS.
3. **All spacing must match Section 4.** No arbitrary padding values.
4. **Maximum information density** — reduce whitespace, not data.
5. **One primary action per view.** All others are secondary or minimal.
6. **Status always uses semantic color tokens** — never brand colors for status.
7. **No decorative elements** — no illustrations, gradients, or custom iconography outside the DS icon library.
8. **Always include empty and loading states** for data components (`NonIdealState`, `Spinner`).
9. **Left-align all text and labels. Right-align numbers only.**
10. **Use ONLY icons from `@varicent/varicent-ui-icons`** (PascalCase React components). Do **not** import from `@carbon/icons-react`, `lucide-react`, `react-icons`, or `@mui/icons-material`. DS components already consume `@varicent/varicent-ui-icons` internally (e.g. `Dialog` uses `Close`).
11. **Never use shadcn/ui, lucide-react, radix-ui, or @mui components.** Always prefer the DS equivalent from Section 5. Do not create `components/ui/` scaffold folders.

---

## 9. ICON REFERENCE

**Package:** `@varicent/varicent-ui-icons`  
**Import pattern:** PascalCase named exports — each icon is a React SVG component.

```tsx
import { ChevronDown, Search, SparkleFilledAI, TrashCan } from '@varicent/varicent-ui-icons';
import { Button, Icon, IconSize } from '@varicent/varicent-ui-core';

// Pass component reference to Button
<Button rightIcon={Search}>Find</Button>

// Or wrap for explicit sizing
<Icon size={IconSize.DEFAULT}><ChevronDown /></Icon>
```

**DO:** Import from `@varicent/varicent-ui-icons`  
**DON'T:** Import from `@carbon/icons-react` directly — always use `@varicent/varicent-ui-icons`. Also avoid `lucide-react`, `react-icons`, `@mui/icons-material`, or hand-drawn SVGs.  
**Why:** Icons are Carbon-derived glyphs (plus custom Varicent icons) packaged as React components with consistent `SvgIconProps`. AI frequently hallucinates icon names — use only PascalCase export names from the list below, not Carbon `Icon/foo--bar` paths.

**Naming rule:** Most icons follow Carbon `Icon/foo--bar` → PascalCase `FooBar` (drop `Icon/` prefix, remove `--`, capitalize each segment). Examples: `Icon/chevron--down` → `ChevronDown`, `Icon/user--avatar` → `UserAvatar`, `Icon/trash-can` → `TrashCan`.

**AI sparkle icons (custom Varicent — not from Carbon):** There is no export named `aiSparkle`. Use `SparkleFilledAI` for the filled Gen AI sparkle (gradient) and `SparkleOutline` for the outline variant. Both are custom Varicent icons and must be imported from `@varicent/varicent-ui-icons` — do not import from `@carbon/icons-react`.

### Approved icons (PascalCase export names)

```
CircleDash
UserMultiple
OperationsRecord
ListChecked
Home
Devices
ContainerSoftware
ChevronSort
Enterprise
Cube
ModelAlt
Activity
DataBase
EventSchedule
Forum
Group
ModelBuilder
ReportData
RequestQuote
LicenseThirdParty
User
ScalesTipped
Settings
UserAvatar
Chat
Notification
Help
Headset
ChatBot
InformationFilled
Information
CheckmarkFilled
CheckmarkOutline
WarningAltFilled
WarningAlt
WarningFilled
Warning
PlayOutlineFilled
PlayOutline
PauseOutlineFilled
PauseOutline
StopOutlineFilled
StopOutline
SendAltFilled
SendAlt
ErrorFilled
ErrorOutline
ViewFilled
ViewOffFilled
View
ViewOff
Edit
Save
DocumentTasks
DocumentExport
TrashCan
AddFilled
AddAlt
SubtractAlt
CloseFilled
CloseOutline
Close
Copy
DocumentView
FolderMoveTo
OverflowMenuHorizontal
ChevronDown
ChevronUp
DownToBottom
UpToTop
Draggable
Redo
Undo
Search
SendToBack
ShapeUnite
Migrate
ArrowLeft
ArrowRight
Table
ZoomIn
Apps
ZoomOut
StringText
StringInteger
ListDropdown
Calendar
Link
SparkleOutline
SparkleFilledAI
```

> :warning: If an icon you need is not in this list, use the closest available one from `@varicent/varicent-ui-icons`. **DO NOT import from `@carbon/icons-react` directly** — always use `@varicent/varicent-ui-icons`. Never add any third-party icon library to the project.

---

## 10. COMPONENT NOTES & LIVING KNOWLEDGE

Real-world fixes and usage rules from Make sessions. This list grows over time.

| File | Purpose |
|------|---------|
| `guidelines/component-notes.md` | Session-learned fixes with Problem / Rule / Don't / Do examples |
| `guidelines/component-patterns.md` | Per-component schema: Use for · Don't · Instead / Pattern · Gotchas · Verified |
| `guidelines/icon-registry.md` | Semantic action → verified `@varicent/varicent-ui-icons` export (append-only) |

### Quick rules (most recent)
- **ElementItemLarge with multiple controls** → Do not use. Compose with `Card + LayoutRow + LayoutColumn` instead. Title column needs `grow + minWidth ≥ 200px`. Control cells must be `flex: 0 0 200px` (never `flex:1`). See `component-patterns.md`.
- **`css` prop in Make runtime** → Only safe on DS components. Use `style={{}}` with `var(--token)` on raw `<div>/<span>/<p>`. See `component-patterns.md`.
- **Icon intent** → Look up semantic action in `icon-registry.md` before importing. Never guess icon names.
