# Tokens — `@varicent/varicent-ui-tokens`

Two parallel APIs for the same design system:

1. **CSS custom properties** in `@varicent/varicent-ui-tokens/dist/variables.css` — `var(--token-name)` (kebab-case).
2. **JS / TS constants** from `@varicent/varicent-ui-tokens` — camelCase strings for Emotion.

```tsx
import {
  text, textHeading, background, accent,
  spacingMedium, borderRadiusMedium, elevation1,
} from "@varicent/varicent-ui-tokens";
```

Always reference tokens — never raw hex/rgb.

---

## Color tokens

### Semantic colors (preferred)

| Category | Tokens |
|---|---|
| Text | `text`, `textHeading`, `textPlaceholder`, `textDisabled`, `textDisabledAccessible`, `textInverse` |
| Background | `background`, `backgroundMidtone`, `backgroundOverlay`, `backgroundDisabled`, `backgroundInverse` |
| Border | `border`, `borderDivider`, `borderTable`, `borderDisabled`, `borderInput`, `borderInputHover`, `borderInputActive` |
| Neutral | `neutral`, `neutralHover`, `neutralActive`, `neutralTranslucentHover`, `neutralTranslucentActive` |
| Accent | `accent`, `accentHover`, `accentActive`, `accentTranslucentHover`, `accentTranslucentActive` |
| AI | `aiPrimary`, `aiPrimaryHover`, `aiPrimaryActive`, `aiTranslucentHover`, `aiTranslucentActive`, `aiPrimaryGradient`, `aiGradientLight`, `aiText`, `aiMidtone` |
| Intent: primary | `intentPrimary`, `intentPrimaryHover`, `intentPrimaryActive`, `intentPrimaryTranslucentHover`, `intentPrimaryTranslucentActive` |
| Intent: success | `intentSuccess`, `intentSuccessHover`, `intentSuccessActive`, `intentSuccessTranslucentHover`, `intentSuccessTranslucentActive` |
| Intent: warning | `intentWarning`, `intentWarningHover`, `intentWarningActive`, `intentWarningTranslucentHover`, `intentWarningTranslucentActive` |
| Intent: danger | `intentDanger`, `intentDangerHover`, `intentDangerActive`, `intentDangerTranslucentHover`, `intentDangerTranslucentActive` |
| Highlight (1–12) | `highlight01`..`highlight12` + each `*Translucent`. Arrays: `highlights`, `highlightTranslucents`. |
| Focus ring | `focus` |
| Graph nodes | `graphNodeGreen`, `graphNodeLime`, `graphNodeYellow`, `graphNodeOrange`, `graphNodeRed`, `graphNodeMagenta`, `graphNodePurple` (+ `*Translucent`). Stored as `"r, g, b"` triplets — wrap with `rgba(…, a)`. |

### Palette scales (use sparingly)

```tsx
import { colorVaricentBlue3 } from "@varicent/varicent-ui-tokens";
const bg = `rgba(${colorVaricentBlue3}, 0.12)`;
```

Scales: `MidnightBlue`, `EveningRed`, `VaricentBlue`, `MorningRed`, `FieldGreen`, `SunlightYellow`, `DuskPurple`, `DarkGray`, `Gray`, `LightGray`, `Red`, `Vermilion`, `Orange`, `Gold`, `Sepia`, `Lime`, `Green`, `Turquoise`, `Blue`, `Cobalt`, `Indigo`, `Rose`.

---

## Typography

Use `<Text type="…">` — never recompute sizes manually. See `components.md`.

```tsx
<Text type="h1">Page title</Text>
<Text type="p">Body copy.</Text>
<Text type="label">Field label</Text>
<Text type="desc">Helper description</Text>
<Text type="error">Validation message</Text>
```

`textTypeValues = ["dh1","dh2","dh3","h1","h2","h3","h4","h5","p","pbold","label","desc","code","placeholder","error"]`

---

## Spacing

Responsive — tokens re-bind per viewport via CSS `var()`:

| Token | Desktop | Tablet (≤960px) | Mobile (≤600px) |
|---|---|---|---|
| `spacingXSmall` | `0.25rem` | inherits | inherits |
| `spacingSmall` | `0.5rem` | `0.25rem` | inherits |
| `spacingMedium` | `1rem` | `0.5rem` | `0.25rem` |
| `spacingLarge` | `2rem` | `0.75rem` | `0.5rem` |
| `spacingXxLarge` | `4rem` | `1rem` | inherits |

```tsx
import { spacingMedium } from "@varicent/varicent-ui-tokens";
<LayoutColumn gap={spacingMedium} padding={spacingLarge}>…</LayoutColumn>
```

---

## Border radius

| Token | Value |
|---|---|
| `borderRadiusExtraSmall` | `0.125rem` |
| `borderRadiusSmall` | `0.25rem` |
| `borderRadiusMedium` | `0.5rem` |
| `borderRadiusLarge` | `0.75rem` |
| `borderRadiusFull` | `100px` (pill) |

---

## Elevation (shadows)

| Token | Use |
|---|---|
| `elevation0` | Hairline border |
| `elevation1` | Resting cards |
| `elevation2` | Hover / raised cards |
| `elevation3` | Popovers, dropdowns |
| `elevation4` | Modals |

Prefer `Card elevation={n}` prop over hand-rolled `box-shadow`.

---

## Breakpoints

| Token | Value |
|---|---|
| `breakpointMaxExtraSmall` | `"600px"` |
| `breakpointMaxSmall` | `"768px"` |
| `breakpointMaxMedium` | `"960px"` |
| `breakpointMinSmall` | `"601px"` |
| `breakpointMinMedium` | `"769px"` |
| `breakpointMinLarge` | `"961px"` |

---

## Layers (z-index)

```ts
dialogLayer  = 100
popoverLayer = 200
tooltipLayer = 300
```

Helpers: `aboveLayer(layer, offset?)`, `belowLayer(layer, offset?)`.

---

## Theming

The package ships **only a light theme**. Import once at app boot:

```ts
import "@varicent/varicent-ui-tokens/dist/variables.css";
```

---

## DO / DON'T

```tsx
// DO
color: text;
background: backgroundMidtone;
gap: spacingMedium;
boxShadow: elevation1;

// DON'T
color: "#4d5580";               // ❌ raw hex
background: "rgb(246,246,249)"; // ❌ raw rgb
padding: "16px";                // ❌ use spacingMedium
```
