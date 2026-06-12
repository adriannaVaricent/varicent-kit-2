# Icon discovery

## Rule: UI chrome icons come from `@carbon/icons-react`

```tsx
import { Home, Settings, Add, TrashCan } from "@carbon/icons-react";
```

Do NOT use `@varicent/varicent-ui-icons` for UI chrome. Use Varicent icons **only** when a design-system component prop requires one (e.g. `Button.icon`, `Tag.icon`, `DropdownMenu.Item.iconLeft`).

```tsx
// UI chrome — always @carbon/icons-react
import { Home, Settings } from "@carbon/icons-react";
<Home size={20} />

// Component icon prop — Varicent icons only
import { CheckmarkFilled } from "@varicent/varicent-ui-icons";
<Tag icon={CheckmarkFilled} intent="success">Done</Tag>
```

---

## Carbon icons (`@carbon/icons-react`)

Size via the `size` prop (default 16):

```tsx
<Home size={20} />
<TrashCan size={16} />
```

### Verified icon list

Use only icons from this list. If absent, use the closest available — never guess.

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

---

## Varicent icons (`@varicent/varicent-ui-icons`)

Use **only** for component icon props.

### Export naming
camelCase folder → PascalCase export:

| Folder | Export |
|---|---|
| `add/` | `Add` |
| `addFilled/` | `AddFilled` |
| `chevronDown/` | `ChevronDown` |
| `aiStatusComplete/` | `AiStatusComplete` |

### Sizing
Icons render at 32×32 viewBox. To resize, wrap in `Icon` from `@varicent/varicent-ui-core`:

```tsx
import { Icon, IconSize } from "@varicent/varicent-ui-core";
import { CheckmarkFilled } from "@varicent/varicent-ui-icons";

<Icon size={IconSize.LARGE}><CheckmarkFilled /></Icon>
```

`IconSize`: SMALL=12, DEFAULT=16, MEDIUM=20, LARGE=24, XLARGE=32.

When passing to a component prop, always pass the **constructor** — not an instance:

```tsx
<Button icon={Add}>New</Button>          // ✅
<Button icon={<Add />}>New</Button>     // ❌
```

### Finding Varicent icons at code-gen time

```bash
# Grep dist folder by keyword (camelCase names):
ls node_modules/@varicent/varicent-ui-icons/dist | grep -i chevron | head -20

# Verify export name from .d.ts:
cat node_modules/@varicent/varicent-ui-icons/dist/chevronDown/chevronDown.d.ts
# → export declare const ChevronDown: React.FC<SvgIconProps>;
```

Never list the full dist directory (~1650 entries). Always narrow with grep first.
