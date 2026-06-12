# Component Notes

A growing log of component usage rules learned from real Make sessions.

---

## ElementItemLarge — title collapses to one character per line

**Problem:** ElementItemLarge's title and metadata slot are both flex:1 min-width:0 internally, so when metadata holds multiple controls the title shrinks to ~0px width and text renders one character per line; columnHeaders in ElementItemsContainer collapse the same way.

**Root cause:** Both title and metadata compete as flex:1 with min-width:0, so metadata with multiple controls wins the flex tug-of-war and collapses the title track.

**Rule:**
- Use ElementItem only for simple rows: title + optional icon + one small control or overflow menu
- For rows with title + multiple inline controls + actions, compose with Card + LayoutRow + LayoutColumn instead
- Title LayoutColumn MUST have grow AND a minWidth/flexBasis floor (≥ 200px)
- Control cells MUST be fixed width (flex: 0 0 <px>) — never flex:1 or width:100%
- Use LayoutRow wrap for responsive stacking instead of hard-coded breakpoints
- If using ElementItemsContainer columnHeaders, every row's metadata must be a matching fixed-width track; otherwise omit columnHeaders and label each control with `<Text type="label">`

**Don't:**
```tsx
// Stuffing multiple controls into ElementItemLarge metadata slot
<ElementItemLarge
  title="Document name"
  metadata={
    <div style={{ width: '100%' }}>
      <Select label="Access" /> // These collapse the title
      <Select label="Sign-off" />
      <Select label="Inquiry" />
    </div>
  }
/>
```

**Do:**
```tsx
import { Card, LayoutRow, LayoutColumn, Text } from "@varicent/varicent-ui-core";
import { spacingMedium, spacingXSmall } from "@varicent/varicent-ui-tokens";

<Card padding="small" radius={4} css={{ width: "100%" }}>
  <LayoutRow gap={spacingMedium} alignItems="center" wrap>
    {/* Title: grows but never collapses below 220px */}
    <LayoutColumn grow minWidth={220} css={{ flexBasis: 220 }}>
      <Text type="h5" multiline={false}>{title}</Text>
    </LayoutColumn>
    {/* Fixed-width control cells */}
    <LayoutColumn minWidth={180} css={{ flex: "0 0 200px" }}>
      <Text type="label">Access</Text>
      <Select ... />
    </LayoutColumn>
    {/* Actions */}
    <LayoutRow gap={spacingXSmall} alignItems="center">
      <Button ... />
    </LayoutRow>
  </LayoutRow>
</Card>
```

**Checklist:** title has grow + minWidth floor · control cells flex:0 0 <px> · no width:100% on metadata · LayoutRow wrap · no hard-coded breakpoints

---

## Emotion css prop — __EMOTION_TYPE_PLEASE_DO_NOT_USE__ warning in Make runtime

**Problem:** In the Figma Make / Vite runtime the per-file `@jsxImportSource @emotion/react` pragma is NOT honored, so using the `css` prop on a raw DOM element (`<div>`, `<span>`, `<p>`) leaks it to the DOM as an unknown attribute and triggers the `__EMOTION_TYPE_PLEASE_DO_NOT_USE__` React warning.

**Root cause:** The Emotion JSX transform pragma can't be relied on in Make's runtime — only Varicent DS components consume `css` as a real prop.

**Rule:**
- Use the `css` prop ONLY on Varicent design-system components (they accept it as a real prop)
- For raw DOM elements, use the native `style` prop with CSS custom property tokens
- Still reference design tokens — use `var(--intent-danger)` in style, not hardcoded hex

**Don't:**
```tsx
// css prop on raw DOM element — leaks to DOM, triggers warning
<div css={{ color: "var(--intent-danger)", padding: spacingSmall }}>
  Warning text
</div>
```

**Do:**
```tsx
// style prop on raw DOM element — safe in Make runtime
<div style={{ color: "var(--intent-danger)", padding: spacingSmall }}>
  Warning text
</div>

// css prop on DS component — fine, consumed as real prop
<Text type="p" css={{ color: "var(--intent-danger)" }}>Warning text</Text>
```

**Checklist:** css prop only on DS components · raw div/span/p use style prop · token values still used (var(--intent-danger), not hex)

---

## Icons — Intent Map

Reference for mapping UI jobs to approved `@varicent/varicent-ui-icons` exports. Use only PascalCase names from the approved list in `Guidelines.md` §9 and the full semantic map in `icon-registry.md`. Import via `import { TrashCan } from '@varicent/varicent-ui-icons'` — never from `@carbon/icons-react`, `lucide-react`, or hand-drawn SVGs.

### Intent → Icon map

#### Actions

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Delete permanently (destructive, irreversible) | `TrashCan` | `Close`, `SubtractAlt`, `ErrorFilled` | Pair with danger intent and confirmation. Icon-only buttons need `aria-label="Delete …"`. |
| Remove from list / detach / unassign (non-destructive) | `SubtractAlt` | `TrashCan`, `Close` | User is removing an association, not deleting the underlying record. |
| Dismiss chip, tag, banner, or inline item | `Close` or `CloseOutline` | `TrashCan`, `SubtractAlt` | `CloseOutline` in secondary/tertiary controls; `CloseFilled` only when matching a filled close-button pattern. |
| Close modal, drawer, or panel | `Close` | `TrashCan`, `ChevronDown` | DS `Dialog` already uses `Close` — do not substitute. |
| Add new item (secondary / inline affordance) | `AddAlt` | `AddFilled`, `Copy` | Toolbar rows, table header actions, "add row" links. |
| Add (primary CTA — create new object) | `AddFilled` | `AddAlt`, `Copy` | Primary buttons: `AddFilled` on `Button intent="primary"`. |
| Edit inline / open edit mode | `Edit` | `Settings`, `DocumentView` | Editing field values or row content. |
| Configure / open settings / admin preferences | `Settings` | `Edit`, `ModelBuilder` | Global or object-level configuration — not inline field edit. |
| Copy / duplicate | `Copy` | `DocumentExport`, `AddAlt` | Duplicating an existing record, rule, or text value. |
| Save / commit changes | `Save` | `CheckmarkFilled`, `SendAlt` | Persisting edits — not approval or submission for review. |
| Export / download data or document | `DocumentExport` | `DocumentView`, `Copy` | Downloading CSV, PDF, or exported comp-plan output. |
| Move / relocate (folder, file, assignment) | `FolderMoveTo` | `Draggable`, `Migrate` | Moving an object to a different container or location. |
| Undo last change | `Undo` | `ArrowLeft`, `Redo` | Editor or form undo — not browser/history back. |
| Redo last undone change | `Redo` | `ArrowRight`, `Undo` | Paired with `Undo` in editor toolbars. |
| Search / find in list or table | `Search` | `ZoomIn`, `ChevronDown` | Filter bars, expandable search, lookup fields. |
| Send / submit message or form | `SendAltFilled` or `SendAlt` | `Save`, `RequestQuote` | Chat send, notify action. Filled on primary send; outline on secondary. |
| View details / open read-only preview | `DocumentView` | `View`, `Edit` | Opening a document, plan detail, or read-only record pane. |
| Show content (visibility on) | `View` or `ViewFilled` | `DocumentView`, `Search` | Toggle revealing hidden values (e.g. masked pay data). Filled = currently visible/primary. |
| Hide content (visibility off) | `ViewOff` or `ViewOffFilled` | `Close`, `TrashCan` | Toggle masking sensitive fields. Pair with `View`/`ViewFilled` as a set. |
| Drag to reorder rows or list items | `Draggable` | `ChevronSort`, `OverflowMenuHorizontal` | Reorder handle only — must offer non-drag alternative for accessibility. |

#### Status & Feedback

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Error — blocking, validation failed, operation failed | `ErrorFilled` | `Information`, `InformationFilled`, `WarningAltFilled` | Primary error state in banners, toasts, field errors. `ErrorOutline` for secondary/low-emphasis errors. |
| Warning — caution, non-blocking risk, stale data | `WarningAltFilled` or `WarningAlt` | `ErrorFilled`, `Information`, `WarningFilled` | Default caution glyph in ICM. Filled in tags/banners; outline in inline hints. See WarningAlt vs Warning below. |
| Success / complete / approved / valid | `CheckmarkFilled` or `CheckmarkOutline` | `InformationFilled`, `ListChecked` | `CheckmarkFilled` for confirmed success; `ListChecked` when meaning "all items validated/checked". |
| Info / neutral help / contextual tip | `InformationFilled` or `Information` | `Help`, `ErrorOutline` | Inline field help, info callouts. `Help` is for help-center/navigation, not inline info. |
| Help / open documentation or support article | `Help` | `Information`, `Headset` | Links to docs, "?" help entry points. |
| Loading / in-progress / pending async work | `CircleDash` | `Activity`, `PauseOutline` | Indeterminate progress on rows, badges, or inline status. Do not animate decoratively in dense tables. |
| Disabled / unavailable (when icon is required) | *(no icon — use disabled control state)* | `ErrorFilled`, `Close` | Prefer disabled button/input with visible reason text; do not imply error. |
| AI-generated content indicator | `SparkleFilledAI` or `SparkleOutline` | `ChatBot`, `SparkleOutline` alone on primary AI CTA | Filled on primary AI surfaces; outline for secondary/metadata "AI-assisted" labels. |
| AI action / Copilot trigger / generate | `SparkleFilledAI` | `ChatBot`, `SendAlt`, `Search` | All Gen AI primary actions and copilot entry points — mandatory. |

#### Navigation

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Collapse section / panel | `ChevronUp` | `ArrowLeft`, `Close` | Section accordion header — chevron reflects expand/collapse, not direction of travel. |
| Expand section / panel | `ChevronDown` | `ArrowRight`, `AddAlt` | Paired with `ChevronUp` on the same control. |
| Next item / forward in queue or wizard | `ArrowRight` | `ChevronDown`, `ChevronUp` | Step forward, next record in review queue. |
| Previous item / back in queue or wizard | `ArrowLeft` | `ChevronDown`, `ChevronUp`, `Undo` | Step back, previous record — not undo. |
| Back to parent page / exit detail view | `ArrowLeft` | `ChevronDown`, `Close` | Breadcrumb back, "return to list" — navigation, not dismiss. |
| Sort column (neutral / bi-directional) | `ChevronSort` | `ChevronDown`, `ChevronUp`, `Draggable` | Table header sort affordance before direction is known. |
| Scroll / jump to top of list or page | `UpToTop` | `ChevronUp`, `ZoomIn` | Long tables, audit logs, activity feeds. |
| Scroll / jump to bottom of list or page | `DownToBottom` | `ChevronDown`, `ZoomOut` | Jump-to-latest in feeds or paginated results. |
| Open overflow / more actions menu | `OverflowMenuHorizontal` | `Settings`, `Draggable` | Row-level or toolbar "more" — not configuration. |
| Expand dropdown / select menu | `ChevronDown` | `ArrowLeft`, `ListDropdown` | Trailing icon on `Select`, combobox, or menu trigger. `ChevronUp` when open. |

#### Data & Content Types (ICM)

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Text / string field type | `StringText` | `DocumentView`, `Forum` | Comp-plan field palette, column type indicator, schema labels. |
| Number / integer field type | `StringInteger` | `StringText`, `Table` | Currency-adjacent fields may still use `StringInteger` for type; format with locale separately. |
| Date / calendar field type | `Calendar` | `EventSchedule`, `StringText` | Field-type icon. `EventSchedule` is for scheduled events/workflows, not field typing. |
| Dropdown / list / picklist field type | `ListDropdown` | `ChevronDown`, `ListChecked` | Field-type indicator in builders — not the chevron on the live control. |
| Formula / calculation / computed field | `ModelAlt` | `ModelBuilder`, `ShapeUnite` | Single formula, expression, or derived metric node. |
| Comp plan / model builder (surface or section) | `ModelBuilder` | `ModelAlt`, `Settings` | Whole plan-designer destination — not a single formula field. |
| Database / data source / connection | `DataBase` | `Table`, `ContainerSoftware` | Ingestion sources, JDBC connections, data-store picker. |
| Report / analytics output | `ReportData` | `Table`, `DocumentView` | Published reports, statement outputs — not raw grid data. |
| Document / file / attachment | `DocumentView` or `DocumentTasks` | `DocumentExport`, `FolderMoveTo` | `DocumentTasks` when task/checklist semantics apply (sign-off docs, workflow attachments). |
| Quota / target / quote object | `RequestQuote` | `ScalesTipped`, `Table` | Quota plans, quote lines, target assignments in SPM context. |
| Territory / geographic or account segment | `Enterprise` | `Cube`, `Group` | Territory hierarchy, org segmentation. `Cube` for multi-dimensional model blocks. |
| Multi-dimensional model block / cube | `Cube` | `Enterprise`, `DataBase` | OLAP-style dimensions in advanced modeling UIs. |
| Team / group / participant set | `Group` or `UserMultiple` | `User`, `Forum` | `UserMultiple` for headcount/participant lists; `Group` for named team entity. |
| User / person / payee | `User` or `UserAvatar` | `UserMultiple`, `LicenseThirdParty` | `UserAvatar` in profile chips and assignee pickers; `User` for generic person column. |
| Role / license / third-party entitlement | `LicenseThirdParty` | `User`, `Settings` | License seats, external system entitlements. |
| Compensation / balance / weigh outcomes | `ScalesTipped` | `RequestQuote`, `ReportData` | Fairness checks, weighting, balance metaphors — not generic quota rows. |
| Table / grid data view | `Table` | `ReportData`, `Apps` | Raw tabular data surface — assignments grid, transaction table. |
| Link / URL / external reference | `Link` | `DocumentView`, `ArrowRight` | Hyperlinks, cross-object references. |
| Activity / audit event stream | `Activity` | `OperationsRecord`, `CircleDash` | Live feed of recent events — not a single audit record. |
| Audit record / operation log entry | `OperationsRecord` | `Activity`, `DocumentTasks` | Single traceable change record in compliance/audit UIs. |
| Discussion / comments thread | `Forum` | `Chat`, `Notification` | Threaded plan discussion — async, attached to an object. |
| Chat / messaging | `Chat` | `Forum`, `ChatBot` | Direct messaging — not AI copilot. |
| Notification / alert bell | `Notification` | `WarningAltFilled`, `Chat` | System notifications inbox — not inline warning status. |
| Home / dashboard landing | `Home` | `Apps`, `Table` | Top-level home nav item. |
| App launcher / module switcher | `Apps` | `Home`, `Table` | Switching between ICM modules or workspaces. |
| Device / channel (mobile, portal) | `Devices` | `Apps`, `User` | Portal or device-specific configuration surfaces. |
| Software container / deployment unit | `ContainerSoftware` | `DataBase`, `Cube` | Technical packaging — rare in admin UIs; use only when literal. |

#### Workflow & Process (ICM)

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Start / run process or calculation job | `PlayOutlineFilled` or `PlayOutline` | `SendAlt`, `ArrowRight` | Run comp calc, start batch job, execute workflow step. Filled on primary run CTA. |
| Pause running process | `PauseOutlineFilled` or `PauseOutline` | `StopOutline`, `CircleDash` | Suspend without terminating — user can resume. |
| Stop / cancel running process | `StopOutlineFilled` or `StopOutline` | `TrashCan`, `Close` | Hard stop or cancel job — distinct from delete record. |
| Approve / sign-off / mark complete | `ListChecked` or `CheckmarkFilled` | `Save`, `SendAlt` | Approval queues, manager sign-off, validation pass. |
| Request / submit for review | `RequestQuote` or `SendAltFilled` | `Save`, `AddFilled` | Submitting for approval — not saving a draft. |
| Audit / track changes (workflow action) | `OperationsRecord` | `Activity`, `Undo` | Open change history or audit trail for an object. |
| Schedule / calendar event / planned run | `EventSchedule` | `Calendar`, `PlayOutline` | Scheduled calc runs, period close dates, review deadlines. |
| Migrate / transfer data between environments | `Migrate` | `FolderMoveTo`, `Copy` | Environment promotion, data migration wizards — not moving one file. |
| Merge / unite shapes (builder operation) | `ShapeUnite` | `Copy`, `AddAlt` | Visual builder merge — comp-plan canvas only. |
| Send to back / layer order (builder) | `SendToBack` | `Draggable`, `DownToBottom` | Z-order in visual builders — not list navigation. |
| Support / contact agent | `Headset` | `Help`, `ChatBot` | Live support entry — not AI copilot. |
| AI assistant persona (non-action, decorative) | `ChatBot` | `SparkleFilledAI` | Avatar or label for bot identity only — never as the primary AI action icon. |

#### Zoom & Magnification (narrow use)

| Job / Intent | Use | Never use instead | Notes |
|---|---|---|---|
| Zoom in on chart, map, or canvas | `ZoomIn` | `Search`, `AddAlt` | Only on genuine zoom controls — never as "view more" or "search". |
| Zoom out on chart, map, or canvas | `ZoomOut` | `SubtractAlt`, `Search` | Paired with `ZoomIn` on the same surface. |

### Anti-patterns — Never do this

1. **Delete vs dismiss** — Do not use `TrashCan` to close a modal, banner, or chip; do not use `Close` to delete a compensation plan, payee assignment, or quota record.
2. **Zoom as action** — Do not use `ZoomIn`/`ZoomOut` for search, "view details", expand row, or add item. Reserve zoom icons for chart/canvas magnification only.
3. **Information for errors** — Do not use `Information` or `InformationFilled` for validation errors or failed saves. Use `ErrorFilled` (blocking) or `WarningAltFilled` (caution).
4. **Settings for edit** — Do not use `Settings` on inline row edit or field-level pencil actions. Use `Edit` for edit; `Settings` for configuration surfaces.
5. **ChatBot for AI features** — Do not use `ChatBot` on Gen AI generate, copilot, or "draft with AI" buttons. Use `SparkleFilledAI` (primary) or `SparkleOutline` (secondary).
6. **ChevronDown for back navigation** — Do not use `ChevronDown`/`ChevronUp` as "go back" or "return to list". Use `ArrowLeft` for navigation back; chevrons are for expand/collapse and dropdown triggers only.
7. **WarningFilled vs WarningAltFilled** — `WarningAltFilled`/`WarningAlt` (triangle) is the standard caution icon for ICM warnings, stale data, and non-blocking risk. `WarningFilled`/`Warning` (diamond) is an alternate Carbon glyph — do not substitute for `WarningAlt` in banners, tags, or form warnings unless matching an existing production screen that already uses the diamond variant.
8. **Checkmark for save** — Do not use `CheckmarkFilled` on Save buttons. Use `Save` to commit; reserve checkmarks for success confirmation or approval status.
9. **Add vs copy** — Do not use `AddAlt`/`AddFilled` for duplicate-row actions. Use `Copy` when cloning an existing rule, tier, or assignment.
10. **Draggable as sort or menu** — Do not use `Draggable` for column sort (`ChevronSort`) or more-actions (`OverflowMenuHorizontal`). Drag handle is reorder-only.

### Icon usage rules

1. **Approved list only** — Import PascalCase exports from `@varicent/varicent-ui-icons`. Never import from `@carbon/icons-react`, `lucide-react`, `react-icons`, or custom SVGs.
2. **Filled vs outline** — Filled variants (`*Filled`, `PlayOutlineFilled`, etc.) for primary actions, active/selected state, and high-emphasis status. Outline variants for secondary actions, inactive toggles, and low-emphasis inline hints.
3. **SparkleFilledAI is mandatory for AI** — All Gen AI / Copilot primary actions, AI buttons, and AI callout headers use `SparkleFilledAI`. `SparkleOutline` only for secondary AI labels. Never `ChatBot` as a substitute.
4. **Pair toggles consistently** — Visibility (`View`/`ViewOff`), playback (`PlayOutline`/`PauseOutline`/`StopOutline`), and expand/collapse (`ChevronDown`/`ChevronUp`) must use matching variant weight (both filled or both outline) on the same control.
5. **Size via `Icon` wrapper** — Wrap icons in `<Icon size={IconSize.DEFAULT}>` (16px) for inline controls and table cells; `<Icon size={48}>` for `NonIdealState` empty/error illustrations. Pass the component reference (`icon={Search}`) to `Button` — do not render ad-hoc sizes.
6. **Icon-only buttons require labels** — Every icon-only `Button` needs `aria-label` describing the outcome ("Delete assignment", "Export report") — never rely on the icon alone for accessibility.
7. **One icon, one job** — Do not repurpose an icon because it "looks close". If the approved list has no match, pick the closest row in this table and document the gap — do not import a new library.
8. **Status icons need text** — Error, warning, and success icons must appear with plain-language text (WCAG — do not communicate status by color/icon alone).

**Checklist:** PascalCase from approved list · filled = primary/active · `SparkleFilledAI` for AI actions · `TrashCan` only for permanent delete · `ArrowLeft` for nav back · `WarningAltFilled` for caution · `Icon`/`IconSize` for sizing · `aria-label` on icon-only buttons · status icon + text
