# Agent instructions for this repository

Personal [SKILL.md](https://github.com/anthropics/skills)-style skills distributed via the Vercel [`skills` CLI](https://github.com/vercel-labs/skills), **Claude Code** plugin marketplaces, and optional **Cursor** project rules. Treat this file as the checklist for any skill change.

## Repository layout

| Path | Purpose |
| --- | --- |
| `skills/<kebab-name>/` | Skill package. **`SKILL.md` is the canonical source** for behavior, triggers, and instructions. |
| `skills/<kebab-name>/.claude-plugin/plugin.json` | Claude Code plugin manifest (`name`, descriptions, `license`, `author`, `keywords`, `category`, `"skills": ["./"]` when `SKILL.md` sits at the plugin root). |
| `.claude-plugin/marketplace.json` | Marketplace catalog: lists plugins and `./skills/<kebab-name>` sources. Marketplace `name` is `curious-dolphin-skills` (used as `@curious-dolphin-skills` when installing plugins). |
| `.cursor/rules/<kebab-name>.mdc` | Optional Cursor rule. **Must stay aligned** with the same skill’s `SKILL.md` (frontmatter + body). Use when you want Cursor to apply the same guidance without relying on the skills CLI alone. |
| `README.md` | Skills table, install commands (`npx skills`, Claude marketplace). **Keep it current** when skills or install flows change. |

## Validation

From the repo root:

```bash
claude plugin validate .
```

Fix any reported issues before merging or publishing.

## Adding a new skill

1. Create `skills/<kebab-name>/` with `SKILL.md` (YAML frontmatter: `name`, `description` — match Anthropic-style triggers).
2. Add **`skills/<kebab-name>/.claude-plugin/plugin.json`** so Claude Code can install the skill as a plugin (see existing [`skills/zen-of-python/.claude-plugin/plugin.json`](skills/zen-of-python/.claude-plugin/plugin.json)).
3. Add a **plugin entry** to [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) (`name`, `source`, `description`, `license`, `author`, `category`, `keywords` as appropriate).
4. If this repo should mirror the skill in Cursor, add **`.cursor/rules/<kebab-name>.mdc`** with the same frontmatter and body as `SKILL.md` (or regenerate from `SKILL.md` so they cannot drift).
5. Add a row to the **Skills** table in [`README.md`](README.md) and extend installation notes only if something is skill-specific beyond the generic repo instructions.

Run `claude plugin validate .` when done.

## Editing an existing skill

1. Edit **`skills/<kebab-name>/SKILL.md`** first.
2. **Cross-agent sync:** update every mirror so behavior and triggers stay identical:
   - **Cursor:** `.cursor/rules/<kebab-name>.mdc` — same frontmatter and content as `SKILL.md` unless the format forces a tiny delta (then document why in a short comment at the top of the `.mdc` file only if unavoidable).
   - **Claude Code:** refresh **`plugin.json`** and the matching block in **`marketplace.json`** if the public `description`, keywords, category, or authorship changed.
3. **README:** if the skill’s purpose, name, or install story changed, update the Skills table and any affected install sections.

## Why alignment matters

Users may consume the same skill through **Cursor rules**, **`npx skills`**, or **Claude plugins**. Divergent descriptions or instructions cause inconsistent answers across tools. After every substantive `SKILL.md` edit, assume you must touch **Cursor (`.mdc`)**, **Claude (`plugin.json` / `marketplace.json`)**, and **README** until you confirm each is updated or intentionally N/A.

## License

Repository license is MIT — see [`LICENSE`](LICENSE). Keep `license` fields in plugin metadata consistent (`MIT`) unless the repo license changes.
