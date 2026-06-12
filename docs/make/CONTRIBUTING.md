# Contributing guidelines to the Varicent Make Kit

This kit is a **living knowledge system**. Rules learned in Make sessions flow back here so every connected project benefits.

## Who contributes

- **Designers** — via Make chat ("add rule:") or by flagging issues to the kit owner
- **Design systems / pioneers** — via Cursor sessions, direct PRs, or review of designer-submitted rules

## How designers add a rule in Make

In any Make session with the kit attached, tell the agent:

```
add rule: [what you learned]
```

**Example:**

```
add rule: Never use ElementItemLarge when a row has more than one inline control — compose with Card + LayoutRow + LayoutColumn instead.
```

The Make agent should:

1. Verify the rule against the actual component API or icon export
2. Append a dated entry to the correct file using the schema below
3. Open a PR to `varicent-kit-2` (or `varicent-ui/guidelines/` for DS-wide rules)

Designers do not need Git or a terminal.

## Which file to update

| Rule type | File | Schema |
|-----------|------|--------|
| Icon semantic mapping | `guidelines/icon-registry.md` | Table row: Semantic action · Export · Folder/Carbon name · Notes (include `NOT X`) · Verified date |
| Per-component usage | `guidelines/component-patterns.md` | **Use for** · **Don't** · **Instead / Pattern** · **Gotchas** · **Verified: YYYY-MM-DD** |
| Session fix with code examples | `guidelines/component-notes.md` | Problem · Root cause · Rule · Don't/Do code · Checklist |
| Tokens, component list, global bans | `guidelines/Guidelines.md` | Section update (requires DS review) |

**Append-only:** add a new row or entry — never silently rewrite existing verified content. If a rule was wrong, add a corrected entry with a new date.

## Verification before merging

1. **Icons** — confirm export exists: `ls node_modules/@varicent/varicent-ui-icons/dist | grep -i <keyword>`, then check `.d.ts`
2. **Components** — confirm name and package in `varicent-ui/ds-index.json`
3. **Patterns** — reproduce the failure in Make if possible; document the fix that worked

## PR → kit publish flow

```
Designer says "add rule:" in Make
        ↓
Agent appends entry + opens PR to varicent-kit-2
        ↓
DS engineering / kit owner reviews
        ↓
Merge to main
        ↓
Kit owner publishes new @make-kits/varicent-kit-2 version in Figma Make Kit UI
        ↓
All connected Make projects pick up changes on next pnpm install
```

## Review checklist

- [ ] Entry follows the schema for its file
- [ ] `Verified: YYYY-MM-DD` date is present
- [ ] Icon exports are confirmed in `@varicent/varicent-ui-icons`
- [ ] Component names match `ds-index.json` (not hallucinated aliases)
- [ ] `NOT X` notes included when a tempting wrong choice exists
- [ ] No silent rewrites of prior verified entries

## Cadence

| When | Action | Who |
|------|--------|-----|
| Per PR | Review for accuracy | DS engineering |
| On merge | Publish new kit version in Figma Make Kit UI | Kit owner |
| Per initiative | Pin kit version in Make file description | Designer |
| Monthly | Review conformance scores; top violations → kit patches | Pioneers + DS |
