# Style system — spacing, layout, responsive

This kit uses **Emotion** (`@emotion/react`) and tokens from `@varicent/varicent-ui-tokens`. There is **no Tailwind**, no CSS modules, no global utility classes. Style with: (1) component props, (2) the `css` prop on design-system components, (3) tokens.

```tsx
/** @jsxImportSource @emotion/react */
import { css } from "@emotion/react";
import { spacingMedium, borderRadiusMedium, border } from "@varicent/varicent-ui-tokens";
```

---

## Spacing scale

| Token | Desktop | Use for |
|---|---|---|
| `spacingXSmall` | `0.25rem` | Tight inline gaps (icon ↔ text) |
| `spacingSmall` | `0.5rem` | Field-internal padding, small gaps |
| `spacingMedium` | `1rem` | Default gap between siblings, card padding |
| `spacingLarge` | `2rem` | Section spacing, page padding |
| `spacingXxLarge` | `4rem` | Hero / chrome-level spacing |

These re-bind across viewports — **do not** replace with literal `16px`.

---

## Layout primitives

| Primitive | When |
|---|---|
| `LayoutColumn` | Vertical stack |
| `LayoutRow` | Horizontal arrangement; supports `wrap` |
| `Divider` | Visual separator (`vertical` for row dividers) |
| `Card` | Surface with elevation/padding/radius |
| `Section` | Titled content section |
| `Panel`, `Drawer` | Side panels |

```tsx
<LayoutColumn gap={spacingMedium} padding={spacingLarge}>
  <Text type="h2">Settings</Text>
  <LayoutRow gap={spacingSmall} alignItems="center" wrap>
    <Tag intent="default">All</Tag>
    <Tag intent="default">Active</Tag>
  </LayoutRow>
</LayoutColumn>
```

There is **no** `Grid` / `Box` / `Stack` / `Inline` — use `LayoutColumn` / `LayoutRow` plus CSS grid via `css` when needed.

---

## Spacing conventions

| Pattern | Token |
|---|---|
| Gap between form fields | `spacingMedium` |
| Gap between buttons in Dialog.Actions | `spacingSmall` |
| Padding inside a Card | `Card padding="medium"` (default) |
| Page-level outer padding | `spacingLarge` desktop |
| Section ↔ section | `spacingLarge` |
| Icon ↔ text inside Button/Tag | handled by component — don't add manual margin |

---

## Responsive patterns

```tsx
import { breakpointMaxMedium, spacingMedium } from "@varicent/varicent-ui-tokens";

const gridStyles = css`
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: ${spacingMedium};

  @media (max-width: ${breakpointMaxMedium}) {
    grid-template-columns: 1fr;
  }
`;
```

---

## CSS methodology

- **Emotion `css` prop** on design-system components is the primary tool for one-off overrides.
- **Emotion `css` template literal** for shared styles within a component file.
- **`styled` API** from `@emotion/styled` for repeated wrappers.
- Import `@varicent/varicent-ui-tokens/dist/variables.css` once at app boot.

**Do not:**
- Add Tailwind / utility class frameworks.
- Apply `css` prop to raw DOM elements without `/** @jsxImportSource @emotion/react */` pragma.
- Author global stylesheets that override design-system internals.

---

## Custom elements alongside the design system

Build from tokens, not literals — keeps custom surfaces visually consistent with `Card`, `Panel`, etc.

```tsx
const cardLike = css`
  padding: ${spacingMedium};
  border: 1px solid ${border};
  border-radius: ${borderRadiusMedium};
  background: ${background};
  color: ${text};
`;

<section css={cardLike}>…</section>
```

---

## DO / DON'T

```tsx
// DO
<LayoutColumn gap={spacingMedium} padding={spacingLarge}>…</LayoutColumn>
border-radius: ${borderRadiusMedium};
@media (max-width: ${breakpointMaxMedium}) { … }

// DON'T
<div style={{ padding: 16, gap: 24 }} />    // ❌ literals
<div className="p-4 gap-2 rounded-md" />    // ❌ Tailwind
<div css={style}>…</div>                    // ❌ missing pragma
<Button><Icon><Add/></Icon><span style={{marginLeft:8}}>New</span></Button>  // ❌
<Button icon={Add}>New</Button>             // ✅
```
