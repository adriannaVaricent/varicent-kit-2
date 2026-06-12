# Components — `@varicent/varicent-ui-core`

All components are imported from `@varicent/varicent-ui-core`. They use Emotion for styles — most accept a `css?: Interpolation<Theme>` prop.

> **Before using any icon, read `icon-discovery.md`.** Do not guess icon names.

```tsx
import { Button, Card, Input, FormGroup, LayoutColumn, LayoutRow, Text } from "@varicent/varicent-ui-core";
```

---

## Layout

### `LayoutRow` / `LayoutColumn`

| Prop | Type | Notes |
|---|---|---|
| `gap` | `CSSProperties["gap"]` | Prefer spacing tokens |
| `padding` | `CSSProperties["padding"]` | |
| `alignItems` | `CSSProperties["alignItems"]` | Default `"stretch"` |
| `justifyContent` | `CSSProperties["justifyContent"]` | |
| `grow` | `boolean` | Fills parent flex |
| `fillHeight` | `boolean` | min-height 100% |
| `wrap` | `boolean` | Row only |

```tsx
<LayoutColumn gap="1rem" padding="1rem">
  <LayoutRow gap="0.5rem" alignItems="center">…</LayoutRow>
</LayoutColumn>
```

### `Card`
`elevation?: 0|1|2|3|4`, `padding?: "none"|"small"|"medium"|"large"` (default `"medium"`), `intent?: "default"|"alternate"`. Pass `interactive` + `onClick` to make a button-card.

### `Divider`
`vertical?: boolean` (default `false`).

---

## Navigation

| Component | Notes |
|---|---|
| `TopNavigation`, `TopNavigationDropdown` | App-shell header. Height constant `TOP_NAV_HEIGHT`. |
| `SideNavigation` | Vertical app rail. |
| `CollapsibleSidebar` | Width constants: `COLLAPSED_WIDTH`, `DEFAULT_WIDTH`, `MIN_WIDTH`, `MAX_WIDTH`. |
| `Tabs` + `Tab` | Controlled (`selectedTabId`, `onChange`). `Tab` requires `id: string`. |
| `Breadcrumbs` | |
| `Link` | Requires `href` OR `onClick`. Supports `icon`, `rightIcon`, `disabled`, `tooltip`. |
| `Header`, `HeaderGroup` | Section headers. |

```tsx
<Tabs id="settings" selectedTabId={tab} onChange={setTab}>
  <Tab id="general" header="General">…</Tab>
  <Tab id="billing" header="Billing">…</Tab>
</Tabs>
```

---

## Data Entry

### `Button`
| Prop | Values | Default |
|---|---|---|
| `priority` | `"tertiary" \| "secondary" \| "primary"` | `"secondary"` |
| `intent` | `"default" \| "danger" \| "ai"` | `"default"` |
| `small` | `boolean` | `false` |
| `loading` | `boolean` | `false` |
| `icon` | icon constructor or `<Icon/>` | — |
| `rightIcon` | icon constructor or `<Icon/>` | — |

Discriminated union: either `children`, or `icon` + `aria-label` (icon-only buttons).

```tsx
<Button priority="primary" icon={Add}>Create</Button>
<Button priority="tertiary" icon={Search} aria-label="Search" />
<Button priority="primary" intent="danger" loading loadingText="Deleting…">Delete</Button>
```

### `Input`
- `prefix?`, `suffix?`: ReactNode. `intent?: "default" | "danger"`.
- Inside `FormGroup`, label/aria are wired automatically.

### `NumberInput`
- `value`, `onValueChange(num, str)`, `min`, `max`, `stepSize`, `clampValueOnBlur`, `allowNumericCharactersOnly`.

### `TextArea`
- `prefix`, `suffix`, `intent`, `minHeight`, `maxHeight`, `showMaxLength`.

### `SearchInput`
- `showClearButton?: boolean` (default `true`), `loading?: boolean`.

### `Select`
- `items: SelectItem[]` — each: `{ text, value, disabled?, description?, iconLeft?, iconRight? }`.
- Single: `selectedItem` + `onItemSelect`. Multi: `selectedItems` + `onItemSelect`.
- `filterable?: boolean` — client-side search. `placeholder`, `loading`, `disabled`, `intent`.

### `DateInput`
- Single: `value: Date | null`, `onChange`. Range: `startDate`, `endDate`, `onStartDateChange`, `onEndDateChange`.

### `FormGroup`
| Prop | Notes |
|---|---|
| `label` | Required `ReactNode` |
| `error` | Replaces `description` when set |
| `description`, `hint` | Helper text below label |
| `required` | Adds asterisk |
| `labelFor` | `id` of the input |
| `hideLabel` | Visually hidden label |

### `Checkbox` / `Switch` / `RadioGroup`
- `Checkbox`: `checked`, `onChange`, `indeterminate`, `disabled`, `label`.
- `Switch`: `checked`, `onChange`, `disabled`, `label`.
- `RadioGroup`: `selectedValue`, `onChange`. Children: `<Radio value="…" label="…">`.

### `ButtonGroup`, `ButtonToggle`, `ControlGroup`
- `ButtonGroup` — joins multiple Buttons.
- `ButtonToggle` — segmented toggle.
- `ControlGroup` — joins input with adjacent buttons.

---

## Data Display

| Component | Key props / notes |
|---|---|
| `Text` | `type: TextType` required. Values: `"dh1"\|"dh2"\|"dh3"\|"h1"\|"h2"\|"h3"\|"h4"\|"h5"\|"p"\|"pbold"\|"label"\|"desc"\|"code"\|"placeholder"\|"error"`. `multiline?: boolean` (default `true`; `false` truncates). |
| `Icon` | `size?: number` (default 16). `IconSize`: SMALL=12, DEFAULT=16, MEDIUM=20, LARGE=24, XLARGE=32. |
| `Tag` | `intent`: default/neutral/warning/success/danger/ai/highlight-01..12. `onRemove` adds ×. |
| `Avatar`, `AvatarStack` | `name` required. `photo?`, `size?: AvatarSize\|number` (SMALL=24, MEDIUM=40, LARGE=64). |
| `MetricCard` | KPI card. Pass a `MetricValue`. |
| `ProgressBar` | `animate?`, `description?`. |
| `ElementItemDefault` / `ElementItemLarge` / `ElementItemSmall` | Selectable element items. See gotchas below. |
| `ListItem`, `SortableList`, `VirtualList` | List primitives. |
| `Toolbar`, `ToolbarGroup` | Toolbar surfaces. |

```tsx
<Text type="h2">Reports</Text>
<Text type="p" multiline={false}>Truncated single-line copy…</Text>
<Tag intent="success" icon={CheckmarkFilled}>Active</Tag>
```

### Icon usage in components
Pass the icon **constructor** (not an instance) — the component wraps it in `<Icon>` internally:

```tsx
<Button icon={Add}>New</Button>          // ✅ constructor
<Button icon={<Add />}>New</Button>     // ❌ instance
```

### `ElementItemLarge` — gotchas (verified 2026-06-12)
Default flex styling collapses when children have explicit widths. Fix via `css` prop:

```tsx
// ✅ correct
<ElementItemLarge
  css={{ width: "100%", minWidth: 0 }}
  primaryContent={<Text type="h5">{name}</Text>}
  secondaryContent={<Text type="desc">{subtitle}</Text>}
  actions={<Button icon={Edit} aria-label="Edit" />}
/>

// ❌ explicit widths on children cause layout collapse
<ElementItemLarge>
  <div style={{ width: "200px" }}>…</div>
</ElementItemLarge>
```

---

## Feedback

### `Callout`
| Prop | Notes |
|---|---|
| `title` or `children` | `title` is the heading |
| `intent` | `"default" \| "success" \| "warning" \| "danger"` |
| `icon` | omit → intent picks default; `null` → no icon |
| `inPage` | Remove border/radius for edge-to-edge banners |
| `action` | ReactNode top-right |

### `Toast`
Same API as `Callout` minus `inPage`, `loading`, `css`. Show via your toast manager.

### `Spinner`
`size?: SpinnerSize` (SMALL=16, MEDIUM=24, LARGE=40). `intent?: "light" | "default" | "ai"`.

### `NonIdealState`
Empty/error states. Combine with `@varicent/varicent-ui-illustrations`.

### `Tooltip` / `InfoTooltip`
- `content?: ReactNode`, `placement?` (default `"top"`), `delay?` (default 700), `showArrow?` (default `true`).
- `InfoTooltip` is the ⓘ + tooltip pattern.

---

## Overlay

### `Dialog` (compound)
```tsx
<Dialog aria-label="Edit user" trigger={<Button>Edit</Button>}>
  <Dialog.Header>Edit user</Dialog.Header>
  <Dialog.Body>…</Dialog.Body>
  <Dialog.Footer>
    <Dialog.Actions>
      <Button>Cancel</Button>
      <Button priority="primary">Save</Button>
    </Dialog.Actions>
  </Dialog.Footer>
</Dialog>
```
- `aria-label` is **required**.
- Controlled: `open` + `onOpenChange`. Uncontrolled: `defaultOpen` or `trigger`.

### `Popover`
- `content?`, `placement?` (default `"bottom-start"`), `open?`, `onOpenChange?`.
- `aria-label` required. `modal?: boolean` (default `true`). `initialFocus?: number | RefObject` (`-1` suppresses autofocus).

### `DropdownMenu` (compound)
```tsx
<DropdownMenu.Menu content={
  <>
    <DropdownMenu.Item text="Profile" iconLeft={Account} />
    <DropdownMenu.Item text="Sign out" intent="danger" iconLeft={Logout} />
    <DropdownMenu.Divider />
  </>
}>
  <Button rightIcon={ChevronDown}>Menu</Button>
</DropdownMenu.Menu>
```
Subcomponents: `Menu`, `SubMenu`, `Item`, `Divider`, `Heading`, `RadioGroup`, `RadioItem`, `CheckboxItem`, `MenuWithSearch`.

`Item` props: `text`, `description`, `iconLeft`, `iconRight`, `labelRight`, `intent`, `selected`, `onSelect`, `href`, `disabled` + `tooltip`.

---

## Illustrations — `@varicent/varicent-ui-illustrations`

```tsx
import { Search } from "@varicent/varicent-ui-illustrations";
<NonIdealState>
  <Search size={240} />
  <Text type="h3">No results</Text>
</NonIdealState>
```

Available: `AiEmptyState`, `Charts`, `Countdown`, `Database`, `FileBox`, `InputNeeded`, `Search`, `SymonFeedback`, `SymonPipes`, `Tasks`, `Workflows`.

---

## Controlled vs. uncontrolled

| Component | Controlled | Uncontrolled |
|---|---|---|
| `Dialog`, `Drawer`, `Popover`, `DropdownMenu` | `open` + `onOpenChange` | `defaultOpen` or `trigger` |
| `Tabs` | `selectedTabId` + `onChange` | omit both |
| `Input`, `TextArea`, `NumberInput` | `value` + handler | `defaultValue` |
| `Checkbox`, `Switch` | `checked` + `onChange` | `defaultChecked` |

---

## Known gotchas (verified in project)

### `IntlProvider` is required (verified 2026-06-12)
All Varicent UI components call `useIntl()`. Missing provider = everything throws:

```tsx
import { IntlProvider } from "react-intl";
import { intlMessages } from "@varicent/varicent-ui-core";
<IntlProvider locale="en" defaultLocale="en" messages={intlMessages}>
  {children}
</IntlProvider>
```

### Emotion `css` prop on raw DOM (verified 2026-06-12)

```tsx
// ❌ throws — missing pragma
<div css={myStyle}>…</div>

// ✅ add at top of file
/** @jsxImportSource @emotion/react */
<div css={myStyle}>…</div>

// ✅ use a design-system component (pragma built in)
<LayoutColumn css={myStyle}>…</LayoutColumn>
```

---

## DO / DON'T

```tsx
// DO
<Button priority="primary" intent="danger" icon={TrashCan}>Delete</Button>
<FormGroup label="Name" required error={errors.name}>
  <Input value={name} onChange={(e) => setName(e.target.value)} />
</FormGroup>

// DON'T
<button className="bg-blue-500">Save</button>  // ❌ raw element
<Button icon={Search} />                        // ❌ missing aria-label
<Button priority="ghost">…</Button>             // ❌ invalid variant
```
