# Varicent UI — design-system guidelines (kit 2)

This project uses the Varicent UI design system, shipped as four npm packages:

- `@varicent/varicent-ui-core` — React components (Emotion-based)
- `@varicent/varicent-ui-icons` — ~1650 named icon components
- `@varicent/varicent-ui-illustrations` — decorative SVG illustrations
- `@varicent/varicent-ui-tokens` — design tokens (CSS vars + JS constants)

All UI MUST be built from these packages. Do not introduce other UI kits (Material UI, AntD, shadcn, Tailwind utility classes) — they fight the token system and break theming.

---

<!-- make-kit-guidelines -->
## Design System Setup — MANDATORY

This project depends on `@make-kits/varicent-kit-2`, `@varicent/varicent-ui-core`, `@varicent/varicent-ui-icons`, `@varicent/varicent-ui-illustrations`, `@varicent/varicent-ui-tokens`. Before writing any code:

1. Read `guidelines/setup.md` and `guidelines/Guidelines.md` by exact path (e.g. `node_modules/@make-kits/varicent-kit-2/guidelines/setup.md`). This project uses pnpm, which symlinks packages — do NOT use `find`, `glob`, or `file_search`. Use: exact path reads, `ls`, `find -L`, or `cat`.
2. Execute all setup instructions against THIS project — not the package itself.
3. Do not skip, modify, or improvise any setup steps.
4. Read ALL other required `.md` files listed below.
5. Verify all packages appear in this project's `package.json` before proceeding.
<!-- /make-kit-guidelines -->

---

## Reading order

Always read first:
- `Guidelines.md` — this file; hub and entry point
- `setup.md` — providers, CSS imports, Vite config
- `tokens.md` — foundational design tokens

Read on-demand:
- `components.md` — BEFORE using any design-system component
- `icon-discovery.md` — BEFORE using any icons
- `styles.md` — when building layouts or applying custom spacing

---

## Product context

**Product type:** B2B SaaS — Sales Performance Management and Sales Planning
**User type:** Power users (compensation admins, sales reps, managers, revenue ops)
**Usage pattern:** High-frequency, data-heavy
**Design priority order:** Clarity → Density → Speed

---

## Priority instructions (read first)

1. Use ONLY color token names listed below. Never use raw hex values.
2. Do NOT modify navigation (`TopNavigation`, `SideNavigation`) or global layout unless explicitly asked.
3. ENSURE ALL tokens below are defined as CSS custom properties in `/src/styles/theme.css`.
4. Use ONLY `@carbon/icons-react` for icons — never `lucide-react`, `react-icons`, `@mui/icons-material`.
5. Use ONLY `<Text type="…">` from `@varicent/varicent-ui-core` for all typography — never raw `<h1>`, `<p>`, or Tailwind text classes.

---

## Color tokens

All tokens are CSS custom properties in `:root {}` in `/src/styles/theme.css`.

### Text
| Token | Value | Usage |
|---|---|---|
| `--text` | `rgb(77, 85, 128)` | Default body text |
| `--text-heading` | `rgb(48, 53, 80)` | All heading levels |
| `--text-placeholder` | `rgba(77, 85, 128, 0.83)` | Form input placeholders |
| `--text-disabled` | `rgb(168, 168, 168)` | Disabled text |
| `--text-inverse` | `#fff` | Text on dark backgrounds |

### Backgrounds
| Token | Value | Usage |
|---|---|---|
| `--background` | `#fff` | Main page and surface background |
| `--background-midtone` | `rgb(246, 246, 249)` | Secondary sections or card backgrounds |
| `--background-overlay` | `rgba(48, 53, 80, 0.4)` | Modal and dialog backdrops |
| `--background-disabled` | `rgb(232, 232, 232)` | Background for disabled elements |
| `--background-inverse` | `rgb(48, 53, 80)` | Dark mode or high-contrast surfaces |

### Borders
| Token | Value | Usage |
|---|---|---|
| `--border` | `rgb(221, 224, 238)` | Standard container and card borders |
| `--border-divider` | `rgba(170, 177, 213, 0.5)` | Thin line separators |
| `--border-input` | `rgb(128, 138, 186)` | Default form input border |
| `--border-input-hover` | `rgb(111, 121, 168)` | Hover state for inputs |
| `--border-input-active` | `rgb(77, 85, 128)` | Focused or active input |

### Neutrals
| Token | Value | Usage |
|---|---|---|
| `--neutral` | `rgb(111, 121, 168)` | Neutral UI elements |
| `--neutral-hover` | `rgb(77, 85, 128)` | Hover state |
| `--neutral-active` | `rgb(48, 53, 80)` | Active state |
| `--neutral-translucent-hover` | `rgba(111, 121, 168, 0.15)` | Subtle hover |
| `--neutral-translucent-active` | `rgba(111, 121, 168, 0.3)` | Translucent active |

### Accents
| Token | Value | Usage |
|---|---|---|
| `--accent` | `rgb(211, 23, 133)` | Secondary accents, highlights |
| `--accent-hover` | `rgb(178, 26, 127)` | Hover |
| `--accent-active` | `rgb(151, 17, 120)` | Active |
| `--accent-translucent-hover` | `rgba(220, 106, 173, 0.15)` | |
| `--accent-translucent-active` | `rgba(202, 114, 165, 0.3)` | |

### Intents
| Token | Value | Usage |
|---|---|---|
| `--intent-primary` | `rgb(43, 79, 244)` | Primary buttons, active states, links |
| `--intent-primary-hover` | `rgb(23, 55, 211)` | Hover |
| `--intent-primary-active` | `rgb(8, 31, 145)` | Active |
| `--intent-primary-translucent-hover` | `rgba(163, 189, 255, 0.15)` | |
| `--intent-primary-translucent-active` | `rgba(157, 182, 251, 0.3)` | |
| `--intent-danger` | `rgb(196, 56, 49)` | Destructive actions |
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
| Token | Value |
|---|---|
| `--ai-primary` | `rgba(121, 56, 224, 1)` |
| `--ai-primary-hover` | `rgba(75, 24, 154, 1)` |
| `--ai-primary-active` | `rgba(53, 17, 110, 1)` |
| `--ai-translucent-hover` | `rgba(121, 56, 224, 0.15)` |
| `--ai-translucent-active` | `rgba(121, 56, 224, 0.3)` |
| `--ai-text` | `rgba(43, 14, 88, 1)` |
| `--ai-midtone` | `rgba(243, 237, 252, 1)` |
| `--ai-primary-gradient` | `linear-gradient(227.84deg, #9F2BD6 10%, #2B4FF4 100%)` |
| `--ai-primary-gradient-hover` | `linear-gradient(252.35deg, #6F1D96 7.83%, #0A2CC7 92.17%)` |
| `--ai-primary-gradient-active` | `linear-gradient(252.54deg, #6F1D96 7.81%, #071C7E 92.19%)` |
| `--ai-gradient-light` | `linear-gradient(90deg, #FFFFFF -40.96%, #D2BDF5 152.41%)` |

### Highlights (categorical — tags, avatars)
| Token | Value |
|---|---|
| `--highlight-01` | `rgb(74, 40, 113)` |
| `--highlight-01-translucent` | `rgba(71, 0, 153, 0.1)` |
| `--highlight-02` | `rgb(15, 67, 127)` |
| `--highlight-02-translucent` | `rgba(0, 71, 153, 0.1)` |
| `--highlight-03` | `rgb(2, 117, 88)` |
| `--highlight-03-translucent` | `rgba(0, 204, 152, 0.1)` |
| `--highlight-04` | `rgb(124, 103, 18)` |
| `--highlight-04-translucent` | `rgba(204, 164, 0, 0.1)` |
| `--highlight-05` | `rgb(150, 44, 141)` |
| `--highlight-05-translucent` | `rgba(204, 0, 188, 0.1)` |
| `--highlight-06` | `rgb(4, 109, 144)` |
| `--highlight-06-translucent` | `rgba(0, 154, 204, 0.1)` |
| `--highlight-07` | `rgb(66, 94, 46)` |
| `--highlight-07-translucent` | `rgba(64, 153, 0, 0.1)` |
| `--highlight-08` | `rgb(144, 81, 50)` |
| `--highlight-08-translucent` | `rgba(204, 68, 0, 0.1)` |
| `--highlight-09` | `rgb(122, 31, 88)` |
| `--highlight-09-translucent` | `rgba(153, 0, 95, 0.1)` |
| `--highlight-10` | `rgb(100, 82, 161)` |
| `--highlight-10-translucent` | `rgba(48, 0, 204, 0.1)` |
| `--highlight-11` | `rgb(3, 87, 94)` |
| `--highlight-11-translucent` | `rgba(0, 141, 153, 0.1)` |
| `--highlight-12` | `rgb(117, 72, 19)` |
| `--highlight-12-translucent` | `rgba(153, 83, 0, 0.1)` |

---

## Typography scale

**Font family:** Inter — `"Inter", sans-serif`
**Base size:** 14px | **Line-height base:** 1.25

Always use `<Text type="…">` from `@varicent/varicent-ui-core`. Never use raw `<h1>`, `<p>`, or Tailwind text utilities.

| `type` value | Size/LH | Weight | Usage |
|---|---|---|---|
| `h1` | 24/30 | Semibold | Page title |
| `h2` | 20/25 | Semibold | Column titles |
| `h3` | 16/20 | Semibold | Section titles |
| `h4` | 16/20 | Semibold | Card title |
| `h5` | 14/17.5 | Semibold | Paragraph title |
| `p` | 14/17.5 | Regular | Paragraph |
| `pbold` | 14/17.5 | Semibold | Bold paragraph |
| `label` | 12/14.4 | Semibold | Field labels |
| `desc` | 14/17.5 | Italic | Descriptions / secondary copy |
| `placeholder` | 14/17.5 | Italic | Input placeholder text |
| `code` | 14/17.5 | Regular | System messages, code, errors |
| `error` | 14/17.5 | Regular | Validation messages |
| `dh1` | Display | Semibold | Hero headings |
| `dh2` | Display | Semibold | Hero subheadings |

---

## Spacing system

**Base unit:** 4px. Always use tokens — never hard-coded pixel values.

| Token | Value | Usage |
|---|---|---|
| `spacingXSmall` | 4px | Tight groupings, icon/text alignment |
| `spacingSmall` | 8px | Internal component padding, small gaps |
| `spacingMedium` | 16px | Section padding, card padding |
| `spacingLarge` | 32px | Section grouping, larger component gaps |
| `spacingXxLarge` | 64px | Major vertical section breaks |

---

## Component library

### Navigation
- `SideNavigation` — Primary app navigation with sub-menus
- `TopNavigation` — Horizontal bar at top of application
- `CollapsibleSidebar` — Secondary in-page navigation, toggleable and resizable

### Data display
- `DataTable` — Primary list view. Sortable, filterable, paginated
- `MetricCard` — KPI/key metric cards
- `Tag` — Stage labels, status indicators
- `ElementItemDefault` / `ElementItemLarge` / `ElementItemSmall` — Selectable item rows

### Forms & inputs
- `FormGroup` — Groups label + input + description/error
- `Input` / `NumberInput` / `TextArea` — Text data entry
- `Select` — Single and multi-select with optional search
- `DateInput` — Single date and range
- `SearchInput` — Search input with clear button

### Feedback
- `Toast` / `Callout` — Non-blocking notifications (success, error, info, warning)
- `Dialog` — Blocking confirmation dialogs
- `Tooltip` / `InfoTooltip` — Label support for icon buttons and truncated text
- `NonIdealState` — Empty / error states, always with a CTA

---

## Layout patterns

### Dashboard
```
[TopNavigation — full width]
[SideNavigation][CollapsibleSidebar] | [MetricCard row]
                                     | [DataTable — full width]
```

### Detail
```
[TopNavigation]
[SideNavigation][CollapsibleSidebar] | [Detail header + stage + actions]
                                     | [TabBar]
                                     | [2-col: Form fields LEFT | ActivityFeed RIGHT]
```

---

## Rules for AI generation

1. Use only components listed in the component library above.
2. Use only color tokens listed above — no raw hex values.
3. All spacing must use token values — no arbitrary padding.
4. Maximum information density — reduce whitespace, not data.
5. One primary action per view. All others are secondary or minimal.
6. Status always uses semantic color tokens — never brand colors for status.
7. No decorative elements — no illustrations, gradients, or custom iconography outside library.
8. Always include empty and loading states for data components.
9. Left-align all text and labels. Right-align numbers only.
10. Use ONLY Carbon Design System icons from `@carbon/icons-react`.
11. Apply `/** @jsxImportSource @emotion/react */` pragma at top of any file using `css` prop on raw DOM elements.
12. Wrap entire app in `<IntlProvider locale="en" messages={intlMessages}>` from `react-intl`.

---

## Icon reference

**DO:** Import from `@carbon/icons-react`
**DON'T:** Use `lucide-react`, `react-icons`, `@mui/icons-material`, or any other library.

```tsx
import { Home, Settings, User, Add } from "@carbon/icons-react";
```

### Verified icon list
```
CircleDash          UserMultiple        OperationsRecord    ListChecked
Home                Devices             ContainerSoftware   ChevronSort
Enterprise          Cube                ModelAlt            Activity
DataBase            EventSchedule       Forum               Group
ModelBuilder        ReportData          RequestQuote        LicenseThirdParty
User                ScalesTipped        Settings            UserAvatar
Chat                Notification        Help                Headset
ChatBot             InformationFilled   Information
CheckmarkFilled     CheckmarkOutline    WarningAltFilled    WarningAlt
WarningFilled       Warning             PlayOutlineFilled   PlayOutline
PauseOutlineFilled  PauseOutline        StopOutlineFilled   StopOutline
SendAltFilled       SendAlt             ErrorFilled         ErrorOutline
ViewFilled          ViewOffFilled       View                ViewOff
Edit                Save                DocumentTasks       DocumentExport
TrashCan            AddFilled           AddAlt              SubtractAlt
CloseFilled         CloseOutline        Close               Copy
DocumentView        FolderMoveTo        OverflowMenuHorizontal
ChevronDown         ChevronUp           DownToBottom        UpToTop
Draggable           Redo                Undo                Search
SendToBack          ShapeUnite          Migrate             ArrowLeft
ArrowRight          Table               ZoomIn              Apps
ZoomOut             StringText          StringInteger       ListDropdown
Calendar            Link                AiSparkleOutline    AiSparkleFilled
```

> If an icon you need is not in this list, use the closest available one. Never import from another library.

---

<!-- HOUSE-RULES:PENDING -->
## Pending kit rules (project-local)

Designers and developers add rows here via `/add-rule` in Make chat, or by opening a PR directly on this repo. Append-only + dated. Remove a row once it ships upstream.

### Icons
| Semantic action | Export | Library | Use when / notes | Verified |
|---|---|---|---|---|

### Components
<!-- ### ComponentName — Use for / Don't / Instead / Gotchas / Verified: YYYY-MM-DD -->
<!-- /HOUSE-RULES:PENDING -->
