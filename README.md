# CuriousDolphin Skills

[SKILL.md](https://github.com/anthropics/skills)-compatible skills for coding agents. Repository: **`CuriousDolphin/skills`**.


## Skills in this repo

| Skill | Description |
| --- | --- |
| [`zen-of-python`](./skills/zen-of-python) | Apply PEP 20 when writing or reviewing any Python — snippets, fixes, refactors, reviews. |
### Zen of python

The flagship skill encodes **PEP 20 — Zen of Python** (Tim Peters, 1999) so models apply the same vocabulary when they write or review Python: readability, explicitness, simplicity, and practical judgment.

#### Why this exists

Without shared criteria, agents drift toward noisy APIs, clever one-liners, and drive-by “cleanup.” The Zen is **principles, not laws** — a compact checklist that keeps Python changes readable, minimal, and aligned with community norms.

#### The Zen of Python (PEP 20)

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

#### How the skill maps those ideas

| Focus | What to optimize for |
| --- | --- |
| **Readability** | Names, structure, comments only where they earn their keep |
| **Explicitness** | Clear APIs, types where they help, no hidden magic |
| **Simplicity** | Fewer layers; don’t abstract until you need it twice |
| **Robustness** | Errors surfaced or deliberately silenced — never accidental silence |
| **Practicality** | When purity fights clarity, choose clarity and say why |

Full operational detail lives in [`skills/zen-of-python/SKILL.md`](./skills/zen-of-python/SKILL.md).


## Install

### Claude Code (plugin marketplace)

Recommended if you want the skill as a **plugin** across projects. See [plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces). Marketplace id: **`curious-dolphin-skills`**.

```bash
claude plugin marketplace add CuriousDolphin/skills
claude plugin install zen-of-python@curious-dolphin-skills
```

In chat: `/plugin marketplace add CuriousDolphin/skills`, then `/plugin install zen-of-python@curious-dolphin-skills`. Invoke with **`/zen-of-python`**.



### Cursor

Install into Cursor’s skill paths with the [Vercel `skills` CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add CuriousDolphin/skills -a cursor
```

Single skill:

```bash
npx skills add CuriousDolphin/skills --skill zen-of-python -a cursor
```

This repo also ships a committed rule at [`.cursor/rules/zen-of-python.mdc`](./.cursor/rules/zen-of-python.mdc) — open the repo in Cursor (or copy the rule into another project) to mirror the same guidance.

### Option C — [skills.sh](https://skills.sh) / default CLI target

When this collection appears on [skills.sh](https://skills.sh), use the install snippet from the listing, or:

```bash
npx skills add CuriousDolphin/skills
```

Use `-a <agent>` to pick agents explicitly (`claude-code`, `cursor`, etc.); omit `-a` to follow your CLI defaults.

## License

MIT — see [LICENSE](./LICENSE).
