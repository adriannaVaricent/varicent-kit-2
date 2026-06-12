# Contributing to varicent-kit-2

This repo is the source of truth for the Varicent UI design-system guidelines used by **Figma Make** and **Cursor**. AI reads the `guidelines/*.md` files at generation time — editing them directly changes AI behaviour for everyone using the kit.

---

## Who contributes

| Role | What they do |
|---|---|
| **Designers** | Add icon/component rules to the `HOUSE-RULES:PENDING` section in `guidelines/Guidelines.md` |
| **Developers** | Update component gotchas, token tables, or setup instructions in the relevant `.md` file |
| **Make chat** | Use `/add-rule` to append a verified rule without opening GitHub directly |

---

## Contribution workflow

```
1. Clone this repo
   git clone https://github.com/adriannaVaricent/varicent-kit-2.git

2. Create a branch
   git checkout -b rule/confirm-icon

3. Edit the relevant guidelines/*.md file

4. Open a PR → get one approval → merge to main

5. Bump the version in package.json
   (patch for rule additions, minor for new sections)

6. Create a git tag and push it
   git tag v0.1.1
   git push origin v0.1.1

7. GitHub Actions publishes to npm automatically

8. Update the kit version in your Make project's package.json
   "@make-kits/varicent-kit-2": "0.1.1"
```

---

## File map

| File | Edit when… |
|---|---|
| `guidelines/Guidelines.md` | Adding house rules, updating token tables, changing AI generation rules |
| `guidelines/components.md` | Adding component gotchas, new component API docs |
| `guidelines/icon-discovery.md` | Updating the verified icon list, adding icon library rules |
| `guidelines/tokens.md` | Updating token values or adding new token categories |
| `guidelines/setup.md` | Changing required packages, providers, or Vite config |
| `guidelines/styles.md` | Updating spacing conventions or layout patterns |

---

## Adding a house rule

Append a row to the `### Icons` or `### Components` table in the `HOUSE-RULES:PENDING` section of `guidelines/Guidelines.md`:

**Icon rule:**
```markdown
| confirm | CheckmarkFilled | @carbon/icons-react | For approval/confirm actions in dialogs | 2026-06-12 |
```

**Component rule:**
```markdown
### Dialog — always set aria-label / Don't omit it / Runtime throws without it / Verified: 2026-06-12
```

Once a rule is verified and merged, remove the row from PENDING — it's now part of the main tables above.

---

## Versioning

| Change type | Bump |
|---|---|
| Add/fix a house rule | patch (0.1.x) |
| New component documented | patch (0.1.x) |
| New guideline section | minor (0.x.0) |
| Breaking change to setup | major (x.0.0) |

---

## npm publishing (first time)

You need an npm account with access to publish under `@make-kits` (or your own scope).

```bash
npm login
npm publish --access public
```

After that, tagging handles it automatically via GitHub Actions (see `.github/workflows/publish.yml`). Add your `NPM_TOKEN` as a GitHub repository secret under **Settings → Secrets → Actions**.
