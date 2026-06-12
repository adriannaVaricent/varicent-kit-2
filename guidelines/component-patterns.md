# Component patterns — verified usage notes

Project-specific, battle-tested rules that supplement `Guidelines.md` Section 5–6. One entry
per component, fixed sub-headings. **Append-only**; stamp each entry with a
`Verified` date. Entries may arrive from Figma Make or Cursor sessions — keep the
schema identical so they stay consistent.

> Schema for each entry:
> **Use for** · **Don't** · **Instead / Pattern** · **Gotchas** · **Verified**

---

### ElementItemLarge / ElementItemDefault
- **Use for:** simple rows — a title, optional icon, and a single overflow menu
  or one small control.
- **Don't:** place a title plus multiple inline controls into the `metadata`
  slot. Internally the title wrapper and `metadata` are both `flex: 1; min-width: 0`;
  in the Make/Vite runtime the title loses the flex tug-of-war and renders
  **one character per line** (vertical text). Same collapse applies to
  `columnHeaders` in `ElementItemsContainer` when metadata holds multiple controls.
- **Instead / Pattern:** compose dense rows yourself from DS primitives:
  `Card` (padding="small", radius={4}, css width:100%) → `LayoutRow` (gap, alignItems center, **wrap**) containing:
  - a title `LayoutColumn` with `grow` **and** a `minWidth`/`flexBasis` floor (≥ ~200px) so it can never collapse;
  - one **fixed-width** cell per control (`LayoutColumn` with `css={{ flex: "0 0 200px" }}`) — never `flex: 1` or `width: 100%`;
  - a trailing `LayoutRow` of action buttons.
  If using `ElementItemsContainer` `columnHeaders`, every row's metadata must be a
  matching fixed-width track; otherwise omit `columnHeaders` and label each control
  with `<Text type="label">`.
- **Gotchas:** `LayoutRow wrap` gives content-driven responsive stacking, so no
  hard-coded breakpoints are needed. No `width: 100%` on metadata wrappers.
- **Verified:** 2026-06-11.

### Styling — `css` prop vs `style`
- **Use for:** the Emotion `css` prop ONLY on design-system components
  (`Button`, `Card`, `LayoutRow/Column`, `Icon`, `Section`, `Text`, …) — they consume it.
- **Don't:** put a `css` prop on a raw DOM element (`<div>`, `<span>`, `<p>`).
  The per-file `@jsxImportSource @emotion/react` pragma is NOT honored in the
  Make runtime, so `css` leaks to the DOM
  (`React does not recognize the css prop … __EMOTION_TYPE_PLEASE_DO_NOT_USE__`).
- **Instead / Pattern:** use the native `style` prop with tokens
  (`style={{ color: "var(--intent-danger)" }}`) or wrap content in a DS
  component such as `<Icon>` or `<Text type="p" css={{ … }}>`.
- **Gotchas:** still reference design tokens in `style` — use `var(--intent-danger)`, not hardcoded hex.
- **Verified:** 2026-06-11.

### Toast
- **Use for:** structured success/error feedback (title + short description).
- **Don't:** assume a toast manager ships — only the presentational `Toast`
  (Callout-style) exists; there is no `onClose` prop.
- **Instead / Pattern:** render `Toast` in a fixed-position stack you own; add a
  close affordance via the `action` prop (a tertiary icon `Button` with `Close`).
- **Verified:** 2026-06-11.

### Select (there is no `Select` component in core)
- **Use for:** single-select inline controls (e.g. assigning a tree/workflow).
- **Don't:** import `Select` from `@varicent/varicent-ui-core` — it is not exported there.
- **Instead / Pattern:** compose `DropdownMenu.Menu` + `DropdownMenu.RadioGroup`/
  `RadioItem` with a `Button` trigger (`rightIcon={ChevronDown}`).
- **Verified:** 2026-06-11.

### Icon-only Button
- **Use for:** recurring controls that have a dedicated icon (see `icon-registry.md`).
- **Don't:** render an icon-only `Button` without `aria-label`
  (the `ChildrenAndIconProps` union requires label-or-children); don't fall back
  to a text button when a dedicated glyph exists.
- **Instead / Pattern:** `<Button priority="tertiary" small icon={UpToTop} aria-label="Expand all sections" tooltip="Expand all sections" onClick={…} />`.
- **Verified:** 2026-06-11.
