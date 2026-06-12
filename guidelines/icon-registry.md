# Icon registry — semantic action → exact export

Look icons up by **semantic action**, not by glyph. The export listed here is the
verified, correct import from `@varicent/varicent-ui-icons` for that action in
this product. Prefer this table over guessing or over any illustrative Carbon
list (which is not exhaustive).

> **Schema (append-only):** add a row, never silently rewrite one. Verify the
> export first (`ls node_modules/@varicent/varicent-ui-icons/dist | grep -i <keyword>`,
> then confirm the `.d.ts`). Record the exact export name, a `Verified` date, and
> a `NOT X` note when there's a tempting wrong pick. Entries may arrive from
> Figma Make sessions or Cursor sessions — keep the schema identical.

Import via `import { TrashCan } from '@varicent/varicent-ui-icons'` — never from
`@carbon/icons-react`, `lucide-react`, `react-icons`, or hand-drawn SVGs.

---

## Actions

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Delete permanently (destructive, irreversible) | `TrashCan` | `trashCan` / trash-can | Pair with danger intent and confirmation. Icon-only buttons need `aria-label="Delete …"`. **NOT `Close`, `SubtractAlt`, `ErrorFilled`.** | 2026-06-11 |
| Remove from list / detach / unassign (non-destructive) | `SubtractAlt` | `subtractAlt` / subtract--alt | User is removing an association, not deleting the underlying record. **NOT `TrashCan`, `Close`.** | 2026-06-11 |
| Dismiss chip, tag, banner, or inline item | `Close` or `CloseOutline` | `close` / close | Dismiss toasts, remove chips. `CloseOutline` in secondary/tertiary controls; `CloseFilled` only when matching a filled close-button pattern. **NOT `TrashCan`, `SubtractAlt`.** | 2026-06-11 |
| Close modal, drawer, or panel | `Close` | `close` / close | DS `Dialog` already uses `Close` — do not substitute. **NOT `TrashCan`, `ChevronDown`.** | 2026-06-11 |
| Add new item (secondary / inline affordance) | `AddAlt` | `addAlt` / add--alt | Toolbar rows, table header actions, "add row" links. **NOT `AddFilled`, `Copy`.** | 2026-06-11 |
| Add (primary CTA — create new object) | `AddFilled` | `addFilled` / add--filled | Primary buttons: `AddFilled` on `Button intent="primary"`. **NOT `AddAlt`, `Copy`.** | 2026-06-11 |
| Edit inline / open edit mode | `Edit` | `edit` / edit | Editing field values or row content. **NOT `Settings`, `DocumentView`.** | 2026-06-11 |
| Configure / open settings / admin preferences | `Settings` | `settings` / settings | Global or object-level configuration — not inline field edit. **NOT `Edit`, `ModelBuilder`.** | 2026-06-11 |
| Copy / duplicate | `Copy` | `copy` / copy | Duplicating an existing record, rule, or text value. **NOT `DocumentExport`, `AddAlt`.** | 2026-06-11 |
| Save / commit changes | `Save` | `save` / save | Persisting edits — not approval or submission for review. **NOT `CheckmarkFilled`, `SendAlt`.** | 2026-06-11 |
| Export / download data or document | `DocumentExport` | `documentExport` / document--export | Downloading CSV, PDF, or exported comp-plan output. **NOT `DocumentView`, `Copy`.** | 2026-06-11 |
| Move / relocate (folder, file, assignment) | `FolderMoveTo` | `folderMoveTo` / folder--move-to | Moving an object to a different container or location. **NOT `Draggable`, `Migrate`.** | 2026-06-11 |
| Undo last change | `Undo` | `undo` / undo | Editor or form undo — not browser/history back. **NOT `ArrowLeft`, `Redo`.** | 2026-06-11 |
| Redo last undone change | `Redo` | `redo` / redo | Paired with `Undo` in editor toolbars. **NOT `ArrowRight`, `Undo`.** | 2026-06-11 |
| Search / find in list or table | `Search` | `search` / search | `ExpandableSearch` / `SearchInput`, filter bars, lookup fields. **NOT `ZoomIn`, `ChevronDown`.** | 2026-06-11 |
| Clear / reset filters | `FilterReset` | `filterReset` / filter--reset | "Clear all" affordance. | 2026-06-11 |
| Send / submit message or form | `SendAltFilled` or `SendAlt` | `sendAltFilled` / send--alt | Chat send, notify action (e.g. notify access tree). Filled on primary send; outline on secondary. **NOT `Save`, `RequestQuote`.** See also `Send` if verified for a specific surface. | 2026-06-11 |
| Notify / send (access tree) | `Send` | `send` / send--alt | Notify access tree. | 2026-06-11 |
| View details / open read-only preview | `DocumentView` | `documentView` / document--view | Opening a document, plan detail, or read-only record pane. **NOT `View`, `Edit`.** | 2026-06-11 |
| View object / open externally | `Launch` | `launch` / launch | Deep-link out to an object. | 2026-06-11 |
| Show content (visibility on) | `View` or `ViewFilled` | `view` / view | Toggle revealing hidden values (e.g. masked pay data). Filled = currently visible/primary. **NOT `DocumentView`, `Search`.** | 2026-06-11 |
| Hide content (visibility off) | `ViewOff` or `ViewOffFilled` | `viewOff` / view--off | Toggle masking sensitive fields. Pair with `View`/`ViewFilled` as a set. **NOT `Close`, `TrashCan`.** | 2026-06-11 |
| Drag to reorder rows or list items | `Draggable` | `draggable` / draggable | Reorder handle only — must offer non-drag alternative for accessibility. **NOT `ChevronSort`, `OverflowMenuHorizontal`.** | 2026-06-11 |
| Validate (row-level) | `CheckmarkOutline` | `checkmarkOutline` / checkmark--outline | Row-level validate action. | 2026-06-11 |
| Overflow / more actions | `OverflowMenuHorizontal` | `overflowMenuHorizontal` / overflow-menu--horizontal | Row action menu trigger. **NOT `Settings`, `Draggable`.** | 2026-06-11 |

## Status & feedback

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Error — blocking, validation failed, operation failed | `ErrorFilled` | `errorFilled` / error--filled | Primary error state in banners, toasts, field errors. `ErrorOutline` for secondary/low-emphasis errors. **NOT `Information`, `WarningAltFilled`.** | 2026-06-11 |
| Warning — caution, non-blocking risk, stale data; orphaned / error indicator | `WarningAltFilled` or `WarningAlt` | `warningAltFilled` / warning--alt--filled | Default caution glyph in ICM. Pair with a `Tooltip`; color via `--intent-danger`. Filled in tags/banners; outline in inline hints. **NOT `ErrorFilled`, `WarningFilled`.** See WarningAlt vs Warning below. | 2026-06-11 |
| Success / complete / approved / valid | `CheckmarkFilled` or `CheckmarkOutline` | `checkmarkFilled` / checkmark--filled | `CheckmarkFilled` for confirmed success; `ListChecked` when meaning "all items validated/checked". **NOT `InformationFilled`.** | 2026-06-11 |
| Info / neutral help / contextual tip | `InformationFilled` or `Information` | `informationFilled` / information--filled | Inline field help, info callouts. `Help` is for help-center/navigation, not inline info. **NOT `ErrorOutline`.** | 2026-06-11 |
| Help / open documentation or support article | `Help` | `help` / help | Links to docs, "?" help entry points. **NOT `Information`, `Headset`.** | 2026-06-11 |
| Loading / in-progress / pending async work | `CircleDash` | `circleDash` / circle-dash | Indeterminate progress on rows, badges, or inline status. Do not animate decoratively in dense tables. **NOT `Activity`, `PauseOutline`.** | 2026-06-11 |
| Disabled / unavailable (when icon is required) | *(no icon — use disabled control state)* | — | Prefer disabled button/input with visible reason text; do not imply error. **NOT `ErrorFilled`, `Close`.** | 2026-06-11 |
| AI-generated content indicator | `SparkleFilledAI` or `SparkleOutline` | `sparkleFilledAI` / sparkle--filled | Filled on primary AI surfaces; outline for secondary/metadata "AI-assisted" labels. **NOT `ChatBot`.** | 2026-06-11 |
| AI action / Copilot trigger / generate | `SparkleFilledAI` | `sparkleFilledAI` / sparkle--filled | All Gen AI primary actions and copilot entry points — mandatory. **NOT `ChatBot`, `SendAlt`, `Search`.** | 2026-06-11 |

## Navigation

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Collapse section / panel | `ChevronUp` | `chevronUp` / chevron--up | Section accordion header — chevron reflects expand/collapse, not direction of travel. Disclosure (closed). **NOT `ArrowLeft`, `Close`.** | 2026-06-11 |
| Expand section / panel | `ChevronDown` | `chevronDown` / chevron--down | Accordion/dropdown open affordance. Disclosure (open). Paired with `ChevronUp` on the same control. **NOT `ArrowRight`, `AddAlt`.** | 2026-06-11 |
| Expand all (sections) | `UpToTop` | `upToTop` / up-to-top | Section expand-all control. Scroll/jump to top of list or page. **NOT `ExpandAll`, `ChevronUp`, `ZoomIn`.** | 2026-06-11 |
| Collapse all (sections) | `DownToBottom` | `downToBottom` / down-to-bottom | Section collapse-all control. Scroll/jump to bottom of list or page. **NOT `CollapseAll`, `ChevronDown`, `ZoomOut`.** | 2026-06-11 |
| Next item / forward in queue or wizard | `ArrowRight` | `arrowRight` / arrow--right | Step forward, next record in review queue. **NOT `ChevronDown`, `ChevronUp`.** | 2026-06-11 |
| Previous item / back in queue or wizard | `ArrowLeft` | `arrowLeft` / arrow--left | Step back, previous record — not undo. **NOT `ChevronDown`, `ChevronUp`, `Undo`.** | 2026-06-11 |
| Back to parent page / exit detail view | `ArrowLeft` | `arrowLeft` / arrow--left | Breadcrumb back, "return to list" — navigation, not dismiss. **NOT `ChevronDown`, `Close`.** | 2026-06-11 |
| Sort column (neutral / bi-directional) | `ChevronSort` | `chevronSort` / chevron--sort | Table header sort affordance before direction is known. **NOT `ChevronDown`, `Draggable`.** | 2026-06-11 |
| Expand dropdown / select menu | `ChevronDown` | `chevronDown` / chevron--down | Trailing icon on `Select`, combobox, or menu trigger. `ChevronUp` when open. **NOT `ArrowLeft`, `ListDropdown`.** | 2026-06-11 |

## Data & content types (ICM)

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Text / string field type | `StringText` | `stringText` / string--text | Comp-plan field palette, column type indicator, schema labels. **NOT `DocumentView`, `Forum`.** | 2026-06-11 |
| Number / integer field type | `StringInteger` | `stringInteger` / string--integer | Currency-adjacent fields may still use `StringInteger` for type; format with locale separately. **NOT `StringText`, `Table`.** | 2026-06-11 |
| Date / calendar field type | `Calendar` | `calendar` / calendar | Field-type icon. `EventSchedule` is for scheduled events/workflows, not field typing. **NOT `StringText`.** | 2026-06-11 |
| Dropdown / list / picklist field type | `ListDropdown` | `listDropdown` / list--dropdown | Field-type indicator in builders — not the chevron on the live control. **NOT `ChevronDown`, `ListChecked`.** | 2026-06-11 |
| Formula / calculation / computed field | `ModelAlt` | `modelAlt` / model--alt | Single formula, expression, or derived metric node. **NOT `ModelBuilder`, `ShapeUnite`.** | 2026-06-11 |
| Comp plan / model builder (surface or section) | `ModelBuilder` | `modelBuilder` / model--builder | Whole plan-designer destination — not a single formula field. **NOT `ModelAlt`, `Settings`.** | 2026-06-11 |
| Database / data source / connection | `DataBase` | `dataBase` / data-base | Ingestion sources, JDBC connections, data-store picker. **NOT `Table`, `ContainerSoftware`.** | 2026-06-11 |
| Report / analytics output | `ReportData` | `reportData` / report--data | Published reports, statement outputs — not raw grid data. **NOT `Table`, `DocumentView`.** | 2026-06-11 |
| Document / file / attachment | `DocumentView` or `DocumentTasks` | `documentTasks` / document--tasks | `DocumentTasks` when task/checklist semantics apply (sign-off docs, workflow attachments). **NOT `DocumentExport`, `FolderMoveTo`.** | 2026-06-11 |
| Quota / target / quote object | `RequestQuote` | `requestQuote` / request-quote | Quota plans, quote lines, target assignments in SPM context. **NOT `ScalesTipped`, `Table`.** | 2026-06-11 |
| Territory / geographic or account segment | `Enterprise` | `enterprise` / enterprise | Territory hierarchy, org segmentation. `Cube` for multi-dimensional model blocks. **NOT `Group`.** | 2026-06-11 |
| Multi-dimensional model block / cube | `Cube` | `cube` / cube | OLAP-style dimensions in advanced modeling UIs. **NOT `Enterprise`, `DataBase`.** | 2026-06-11 |
| Team / group / participant set | `Group` or `UserMultiple` | `userMultiple` / user--multiple | `UserMultiple` for headcount/participant lists; `Group` for named team entity. **NOT `User`, `Forum`.** | 2026-06-11 |
| User / person / payee | `User` or `UserAvatar` | `userAvatar` / user--avatar | `UserAvatar` in profile chips and assignee pickers; `User` for generic person column. **NOT `UserMultiple`.** | 2026-06-11 |
| Role / license / third-party entitlement | `LicenseThirdParty` | `licenseThirdParty` / license--third-party | License seats, external system entitlements. **NOT `User`, `Settings`.** | 2026-06-11 |
| Compensation / balance / weigh outcomes | `ScalesTipped` | `scalesTipped` / scales--tipped | Fairness checks, weighting, balance metaphors — not generic quota rows. **NOT `RequestQuote`, `ReportData`.** | 2026-06-11 |
| Table / grid data view | `Table` | `table` / table | Raw tabular data surface — assignments grid, transaction table. **NOT `ReportData`, `Apps`.** | 2026-06-11 |
| Link / URL / external reference | `Link` | `link` / link | Hyperlinks, cross-object references. **NOT `DocumentView`, `ArrowRight`.** | 2026-06-11 |
| Activity / audit event stream | `Activity` | `activity` / activity | Live feed of recent events — not a single audit record. **NOT `OperationsRecord`, `CircleDash`.** | 2026-06-11 |
| Audit record / operation log entry | `OperationsRecord` | `operationsRecord` / operations--record | Single traceable change record in compliance/audit UIs. **NOT `Activity`, `DocumentTasks`.** | 2026-06-11 |
| Discussion / comments thread | `Forum` | `forum` / forum | Threaded plan discussion — async, attached to an object. **NOT `Chat`, `Notification`.** | 2026-06-11 |
| Chat / messaging | `Chat` | `chat` / chat | Direct messaging — not AI copilot. **NOT `Forum`, `ChatBot`.** | 2026-06-11 |
| Notification / alert bell | `Notification` | `notification` / notification | System notifications inbox — not inline warning status. **NOT `WarningAltFilled`, `Chat`.** | 2026-06-11 |
| Home / dashboard landing | `Home` | `home` / home | Top-level home nav item. **NOT `Apps`, `Table`.** | 2026-06-11 |
| App launcher / module switcher | `Apps` | `apps` / apps | Switching between ICM modules or workspaces. **NOT `Home`, `Table`.** | 2026-06-11 |
| Device / channel (mobile, portal) | `Devices` | `devices` / devices | Portal or device-specific configuration surfaces. **NOT `Apps`, `User`.** | 2026-06-11 |
| Software container / deployment unit | `ContainerSoftware` | `containerSoftware` / container--software | Technical packaging — rare in admin UIs; use only when literal. **NOT `DataBase`, `Cube`.** | 2026-06-11 |

## Workflow & process (ICM)

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Start / run process or calculation job | `PlayOutlineFilled` or `PlayOutline` | `playOutlineFilled` / play--outline--filled | Run comp calc, start batch job, execute workflow step. Filled on primary run CTA. **NOT `SendAlt`, `ArrowRight`.** | 2026-06-11 |
| Pause running process | `PauseOutlineFilled` or `PauseOutline` | `pauseOutlineFilled` / pause--outline--filled | Suspend without terminating — user can resume. **NOT `StopOutline`, `CircleDash`.** | 2026-06-11 |
| Stop / cancel running process | `StopOutlineFilled` or `StopOutline` | `stopOutlineFilled` / stop--outline--filled | Hard stop or cancel job — distinct from delete record. **NOT `TrashCan`, `Close`.** | 2026-06-11 |
| Approve / sign-off / mark complete | `ListChecked` or `CheckmarkFilled` | `listChecked` / list--checked | Approval queues, manager sign-off, validation pass. **NOT `Save`, `SendAlt`.** | 2026-06-11 |
| Request / submit for review | `RequestQuote` or `SendAltFilled` | `requestQuote` / request-quote | Submitting for approval — not saving a draft. **NOT `Save`, `AddFilled`.** | 2026-06-11 |
| Audit / track changes (workflow action) | `OperationsRecord` | `operationsRecord` / operations--record | Open change history or audit trail for an object. **NOT `Activity`, `Undo`.** | 2026-06-11 |
| Schedule / calendar event / planned run | `EventSchedule` | `eventSchedule` / event--schedule | Scheduled calc runs, period close dates, review deadlines. **NOT `Calendar`, `PlayOutline`.** | 2026-06-11 |
| Migrate / transfer data between environments | `Migrate` | `migrate` / migrate | Environment promotion, data migration wizards — not moving one file. **NOT `FolderMoveTo`, `Copy`.** | 2026-06-11 |
| Merge / unite shapes (builder operation) | `ShapeUnite` | `shapeUnite` / shape--unite | Visual builder merge — comp-plan canvas only. **NOT `Copy`, `AddAlt`.** | 2026-06-11 |
| Send to back / layer order (builder) | `SendToBack` | `sendToBack` / send-to-back | Z-order in visual builders — not list navigation. **NOT `Draggable`, `DownToBottom`.** | 2026-06-11 |
| Support / contact agent | `Headset` | `headset` / headset | Live support entry — not AI copilot. **NOT `Help`, `ChatBot`.** | 2026-06-11 |
| AI assistant persona (non-action, decorative) | `ChatBot` | `chatBot` / chat-bot | Avatar or label for bot identity only — never as the primary AI action icon. **NOT `SparkleFilledAI`.** | 2026-06-11 |

## Zoom & magnification (narrow use)

| Semantic action | Export | Folder / Carbon name | Use when / notes | Verified |
|---|---|---|---|---|
| Zoom in on chart, map, or canvas | `ZoomIn` | `zoomIn` / zoom-in | Only on genuine zoom controls — never as "view more" or "search". **NOT `Search`, `AddAlt`.** | 2026-06-11 |
| Zoom out on chart, map, or canvas | `ZoomOut` | `zoomOut` / zoom-out | Paired with `ZoomIn` on the same surface. **NOT `SubtractAlt`, `Search`.** | 2026-06-11 |

---

## Anti-patterns — never do this

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

## Icon usage rules

1. **Approved list only** — Import PascalCase exports from `@varicent/varicent-ui-icons`. Never import from `@carbon/icons-react`, `lucide-react`, `react-icons`, or custom SVGs.
2. **Filled vs outline** — Filled variants (`*Filled`, `PlayOutlineFilled`, etc.) for primary actions, active/selected state, and high-emphasis status. Outline variants for secondary actions, inactive toggles, and low-emphasis inline hints.
3. **SparkleFilledAI is mandatory for AI** — All Gen AI / Copilot primary actions, AI buttons, and AI callout headers use `SparkleFilledAI`. `SparkleOutline` only for secondary AI labels. Never `ChatBot` as a substitute.
4. **Pair toggles consistently** — Visibility (`View`/`ViewOff`), playback (`PlayOutline`/`PauseOutline`/`StopOutline`), and expand/collapse (`ChevronDown`/`ChevronUp`) must use matching variant weight (both filled or both outline) on the same control.
5. **Size via `Icon` wrapper** — Wrap icons in `<Icon size={IconSize.DEFAULT}>` (16px) for inline controls and table cells; `<Icon size={48}>` for `NonIdealState` empty/error illustrations. Pass the component reference (`icon={Search}`) to `Button` — do not render ad-hoc sizes.
6. **Icon-only buttons require labels** — Every icon-only `Button` needs `aria-label` describing the outcome ("Delete assignment", "Export report") — never rely on the icon alone for accessibility.
7. **One icon, one job** — Do not repurpose an icon because it "looks close". If the approved list has no match, pick the closest row in this table and document the gap — do not import a new library.
8. **Status icons need text** — Error, warning, and success icons must appear with plain-language text (WCAG — do not communicate status by color/icon alone).

**Checklist:** PascalCase from approved list · filled = primary/active · `SparkleFilledAI` for AI actions · `TrashCan` only for permanent delete · `ArrowLeft` for nav back · `WarningAltFilled` for caution · `Icon`/`IconSize` for sizing · `aria-label` on icon-only buttons · status icon + text
