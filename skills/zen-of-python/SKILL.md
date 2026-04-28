---
name: zen-of-python
description: Apply the Zen of Python (PEP 20) when writing or reviewing ANY Python code — full programs, scripts, functions, even one-liners. Use this skill every time the user asks for Python, whether or not they say "pythonic" or "clean code". Includes snippets, bug fixes, refactors, and code reviews. If it's Python, use this skill."
---

# Zen of Python

Operational guide to PEP 20 for writing and reviewing Python. Principles, not rules — apply with judgment.

## How to use

- **Writing code:** internalize the principles, then run the [checklist](#checklist) on the draft. Don't over-correct tiny snippets.
- **Reviewing code:** scan against the [checklist](#checklist), flag issues, propose a rewrite.
- **Inline comments:** when a non-obvious choice traces back to a principle, add a *short* comment referencing it (e.g. `# flat > nested`). 0–3 per function. Often zero for snippets ≤10 lines.
- **Don't lecture** the user about PEP 20 in prose. Apply silently. Mention an aphorism only when explaining a concrete change.

## Principles in practice

**Beautiful > ugly.** Code is read more than written. Rewrite gnarly chains, cryptic one-liners, names like `x2`/`tmp`/`data2`.

**Explicit > implicit.** Type hints on public functions. Named args when there are 3+. No `import *`. Function names should reveal side effects (`get_user` shouldn't write to a DB). But: truthiness checks and duck typing are fine when types are unambiguous — explicit ≠ verbose.

**Simple > complex.** Prefer fewer moving parts. No class hierarchy where a function works. No async for sync calls.

**Complex > complicated.** When the problem is genuinely complex (parser, scheduler), structure it into small components. Complex = many understandable parts. Complicated = no one can follow it.

**Flat > nested.** Use early returns / guard clauses to flatten:
```python
# Bad: nested
def f(user):
    if user is not None:
        if user.is_active:
            return do(user)
# Good: flat
def f(user):
    if user is None: return None
    if not user.is_active: return None
    return do(user)
```

**Sparse > dense.** Break long expressions across lines. Blank lines between logical sections. Don't chain `.filter().map().reduce()` into unreadable single expressions — name intermediate results.

**Readability counts.** The tiebreaker. Names describe intent (`active_users` not `user_list`). A loop with an obvious name beats a comprehension that needs a comment.

**Special cases aren't special enough...** Consistency compounds. Don't break codebase conventions for one "special" module.

**...although practicality beats purity.** When following a rule produces clearly worse code, break it — and leave a comment explaining why.

**Errors should never pass silently.** Bare `except:` and `except Exception: pass` are almost always wrong. Catch specific exceptions; log, re-raise, or return a meaningful default.

**Unless explicitly silenced.** When silencing is correct, make it obvious:
```python
try:
    tmp_path.unlink()
except FileNotFoundError:
    pass  # Best-effort cleanup; missing file is fine.
```

**In the face of ambiguity, refuse to guess.** Surface unclear inputs as errors or ask the user. Don't paper over with a default that *might* be right.

**One obvious way to do it.** Don't offer five ways to do the same thing in an API. Pick one.

**...although that way may not be obvious unless you're Dutch.** Use idiomatic Python: `with`, `enumerate`, `zip`, comprehensions, `dataclasses`, `pathlib`, f-strings.

**Now > never. Although never > *right* now.** Ship working code, but don't ship broken code under pressure.

**Hard to explain → bad idea. Easy to explain → maybe good.** If you can't write a 2-sentence docstring, the function is doing too much.

**Namespaces are great.** Group code in modules/classes. Avoid global state — pass dependencies in. No star imports.

## Anti-patterns to fix on sight

| Bad | Good |
|---|---|
| `if x == True:` | `if x:` |
| `if len(items) > 0:` | `if items:` |
| `for i in range(len(items)):` | `for i, item in enumerate(items):` |
| `try: ... except: pass` | catch specific exception; comment if silencing |
| `def f(x=[]):` | `def f(x=None): x = x or []` |
| String `+=` in loop | `"".join(parts)` |
| `d[k] if k in d else default` | `d.get(k, default)` |
| Manual file open/close | `with open(...) as f:` |
| `type(x) == C` | `isinstance(x, C)` |
| Long `if/elif` on a value | dict dispatch or `match` (3.10+) |
| `os.path.join` everywhere | `pathlib.Path` |
| `"%s" % x` / `.format()` for simple cases | f-strings |

## Checklist

After drafting, scan these. Revise what clearly fails — don't over-engineer small snippets.

1. **Names** describe purpose (no `data`/`tmp`/`x` outside ≤3-line scopes).
2. **Indentation** not noticeably deep — guard clauses where it is.
3. **Function size** — one thing per function.
4. **Errors** — every `except` does something or is commented as silenced.
5. **Type hints** on public functions when non-trivial; no `import *`.
6. **Idioms** — `with`, `enumerate`, `zip`, comprehensions, `pathlib`, f-strings.
7. **Anti-patterns** — none from the table above.
8. **Imports** — grouped: stdlib / third-party / local.
9. **Docstrings** — one-liner on public functions/classes.
10. **No dead code** — no commented blocks, unused imports.

## When principles conflict

Tiebreakers, not laws — context wins:

1. **Readability** beats almost everything.
2. **Explicit** beats **simple** when simple hides important behavior.
3. **Practicality** beats **consistency** when the rule genuinely produces worse code (with a comment).
4. **Errors not silent** beats **simple** — robust handling is worth the lines.

## Out of scope

- Performance optimization (Zen is silent on perf — optimize after, not instead of).
- Type system gymnastics (over-engineering generics violates *simple > complex*).
- Forcing every principle onto every snippet (match rigor to scope).
