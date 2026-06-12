## Python Project Skill

### Onboarding — ask these questions first, before doing anything else

Ask the user all of the following in a single message. Do not proceed until answered.

1. **Project type** — CLI / library / script / other?
2. **CLI type** *(skip if not CLI)* — few flags/no subcommands → `argparse`; subcommands/completion → `click`; or let the user decide
3. **First module/feature** — what to start with?
4. **New project or existing?** — if existing, check whether `.git`, `.venv`, `pyproject.toml` already exist before touching setup

Use the answers to skip irrelevant sections: omit the CLI section entirely if not a CLI project; omit setup if the project already exists.

---

### Setup
- `uv init --python python3.12`; add `pytest` to dev dependencies only if `.git`, `.venv`, or `pyproject.toml` doesn't exist; never modify existing `pyproject.toml`
- `uv venv`; never commit `.venv/`

### Workflow
- Confirm scope before starting — ask "which module/feature?"; never assume
- Order per module: code → tests → docs; commit after each complete module before the next

### Writing code — style rules
- Single quotes; `'''` for docstrings and all multiline strings/comments — never `"""`
- f-strings only — never `%`/`.format()`/concat
- Import order: stdlib → (blank line) → third-party → (blank line) → local → (blank line if next line is not `def`/`class`)
- Absolute imports only; no relative dots
- No trailing commas — sigs, calls, dicts, lists, imports
- 2 blank lines around every `def`/`class`; inline if <~100 chars
- Annotate all public signatures; `X | Y` not `Union[X, Y]`; `list[x]`/`dict[k,v]` not `List`/`Dict`; explicit `-> None`; avoid `Any`
- `raise X from e` always; never bare `except:`; catch most specific type
- `pathlib.Path` over `os.path`
- Names: `snake_case` funcs/vars · `PascalCase` classes · `SCREAMING_SNAKE_CASE` constants
- Comments: only when WHY is non-obvious; no dividers; never restate what code does

### CLI (only if building a CLI)
- Simple (few flags, no subcommands): `argparse`; complex (subcommands, completion): `click`
- Click: completion via `BashComplete`/`ZshComplete`; errors to stderr via `click.echo(msg, err=True)`; exit codes 0=success · 1=error · 2=misuse; `--verbose`/`-v` on root group via `ctx.obj`

### Testing
- `pytest` only; never `unittest`
- Naming: `test_<what>_<condition>_<expected>`
- Structure: arrange → act → assert; one logical assertion per test
- Fixtures in `conftest.py`; prefer function scope
- `@pytest.mark.parametrize` over duplicated test bodies
- Mock at system boundaries only — never mock internal functions
- `tmp_path` over manual temp dirs; `pytest.raises(E, match=...)` for error assertions
- Never `assert (a, b)` — always `assert a == b`
- Remind user to run `pytest` after each module; do not run it yourself

### Before committing
- Stage explicit files; one commit per module type — code, tests, docs separate
- Branch: `type/short-description`; message: `type: short description` (`feat`/`fix`/`refactor`/`test`/`docs`/`chore`)
- Never `git add .` or `-A`; never `--no-verify`
