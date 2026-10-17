# claudemdfiles

CLAUDE.md files for Claude Code. Claude Code reads these files at the start of each session and follows the instructions in them.

| Folder | What it is | Where it goes |
| --- | --- | --- |
| `global_claudemd/` | Personal instructions for every project | `~/.claude/CLAUDE.md` |
| `package_claudemd/` | Template for a Python package repo | `CLAUDE.md` in the root of the package repo |

## Global CLAUDE.md

Claude Code loads `~/.claude/CLAUDE.md` in every session, in every project. To use this file as your global CLAUDE.md, copy it there:

```bash
mkdir -p ~/.claude
cp global_claudemd/CLAUDE.md ~/.claude/CLAUDE.md
```

To keep it in sync with this repo, you can use a symlink instead:

```bash
ln -sf "$(pwd)/global_claudemd/CLAUDE.md" ~/.claude/CLAUDE.md
```

It sets general working rules for Claude, for example:

- Only edit files inside the current working directory, and do not `cd`.
- State assumptions and ask when something is unclear, before writing code.
- Keep changes small and simple, and do not refactor unrelated code.
- Keep chat replies short, in British English and ASD-STE100, with no em-dashes.
- Use `uv pip` to install Python packages, and do not use classes.
- Write descriptive commit messages, with no Claude attribution and no new branches unless asked.
- Write maths in chat as Unicode glyphs, not LaTeX.

## Package CLAUDE.md

Copy `package_claudemd/CLAUDE.md` into the root of a Python package repo:

```bash
cp package_claudemd/CLAUDE.md /path/to/your_package/CLAUDE.md
```

Then edit it for your package:

1. Replace `package_name` with the name of your package.
2. Replace `pn` with the import alias you use.
3. Fill in `<purpose>`, `<area 1>` and `<area 2>` in the Package Overview.

Claude Code loads this file when you work in that repo, in addition to the global file. With it, Claude will:

- Keep **one function per file**, with the filename the same as the function name.
- Write docstrings in a fixed format, with ARGUMENTS and OUTPUT sections, and use type hints.
- Not change code, rename functions or change the package structure unless you ask.
- Only edit functions in `package_name/<module>/`, tests in `tests/<module>/` and examples in `examples/`, and not commit anything itself.
- Follow a fixed workflow when it creates or changes a function:
  1. Search the package for an existing function that already does the job.
  2. Write the function.
  3. Reinstall the package with `uv pip install '.[dev]'` (or `pip` if uv is not installed).
  4. Write tests in `tests/<module>/test_<function_name>.py` and run them.
  5. Write 3 examples in `examples/<module>/ex_<function_name>.py` (a notebook for async functions) and run them.
  6. Add the import to `package_name/<module>/__init__.py`.
  7. Regenerate the lazy registry with `python -m package_name._gen_registry` and run `tests/test_registry.py`.

Step 7 expects the package to have a `_gen_registry.py` script that builds `_registry.py`, which serves the flat `pn.<func>` API. If your package does not use a registry, delete that step from the file.
