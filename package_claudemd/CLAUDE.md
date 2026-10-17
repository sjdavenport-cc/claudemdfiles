# CLAUDE.md - package_name Repository Guidelines

## Package Overview
package_name is a Python library for <purpose>. It provides utilities for:
- <area 1>
- <area 2>

## Code Structure Rules

### One Function Per File
- Each `.py` file should contain **exactly one function**
- The filename should match the function name (e.g., `filter_nones.py` contains `def filter_nones(...)`)
- When asked to create or modify a function (or its tests or examples), follow the Function Development Workflow below. 

## Code Changes
- Don't make changes to the code or the files in the repo unless asked to.
- Do not rename existing functions unless explicitly directed to.
- Maintain the current structure of the package unless explicitly directed otherwise.
- Warn the user when a change might make the current branch incompatible with its base branch.
- If a big change is made that deserves a change to the readme, suggest doing so to the user.

### Function Structure Template

All functions must follow this exact docstring structure:

```python
def function_name(arg1: type1, arg2: type2, optional_arg: type3 = default_value) -> return_type:
    """
    Brief one-line description of what the function does.

    Extended description if needed, explaining the function's purpose
    and behavior in more detail.

    ---------------------------------------------------------------------------
    ARGUMENTS:
    arg1 : type1
        Description of arg1.
    arg2 : type2
        Description of arg2.
    optional_arg : type3, optional (default=default_value)
        Description of optional_arg.

    ---------------------------------------------------------------------------
    OUTPUT:
    return_var : return_type
        Description of what is returned.
    ---------------------------------------------------------------------------
    """
    # Function implementation
    return return_var
```

### Key Conventions
- **Use Python type hints** in function signatures (arguments and return type)
- Docstrings use triple dashes `---` as section separators (exactly 75 dashes)
- Type annotations repeated in docstring format: `arg_name : type`
- Optional arguments noted as `optional (default=value)`
- Don't use classes unless its unavoidable.
- when writing prompts that span multiple lines in python code use """ """

### Import Convention
- Import package_name as: `import package_name as pn`

## Function Development Workflow

All paths below are relative to the repo root. `<module>` is the folder path
of the function inside `package_name/`, e.g. `module_a` or `subpackage/module_b`.

### Allowed Edit Directories

- Functions: `package_name/<module>/`
- Tests: `tests/<module>/`
- Examples: `examples/auto_examples/<module>/`

Do NOT edit files outside these three directories, except the `__init__.py`
and registry files named in steps 8-9. Do NOT git commit anything yourself.

### Search Scope

When searching for context, patterns, or existing implementations, search the
full package. This ensures you understand existing conventions and avoid
duplicating functionality.

### When asked to create or modify a function:

**Before starting:** If the user has not said where the function goes, ask,
suggesting sensible `<module>` options. Summarize the task and ask clarifying
questions about any ambiguities.

Then follow these steps:

1. **Check for existing functions first** - Search the full package for existing functions that provide the needed functionality. Reuse existing utilities rather than duplicating. Only create new functions when no suitable alternative exists.

2. **Create/modify the function** - Write it following the conventions above. One function per file, filename matching the function name.

3. **Reinstall the package** - Run: `uv pip install '.[dev]'` from the repo root if uv is installed, otherwise `pip install '.[dev]'`.

4. **Write tests** - Create a test file at:
   `tests/<module>/test_<function_name>.py`

5. **Execute tests** - Run the test file with:
   ```
   python tests/<module>/test_<function_name>.py
   ```
   Verify all tests pass before proceeding.

6. **Write examples** - Create examples at:
   `examples/<module>/ex_<function_name>.py`

   - Include 3 relatively simple examples
   - Separate each example with `#%%`
   - **For async functions:** Create a Jupyter notebook (`ex_<function_name>.ipynb`) instead of a `.py` file

7. **Execute examples** - Run to verify they work:
   - For `.py` files: `python <examples_file>.py`
   - For `.ipynb` notebooks: `jupyter execute <examples_file>.ipynb`

   Do NOT skip this step. Execution is mandatory.

8. **Update __init__.py** - Add the import to:
   `package_name/<module>/__init__.py`

9. **Regenerate the lazy registry** - The flat `pn.<func>` API is served by the
   auto-generated `package_name/_registry.py`. After changing any
   `__init__.py` (or adding/removing a function), rebuild it with:
   ```
   python -m package_name._gen_registry
   ```
   Then confirm it is in sync by running `tests/test_registry.py`.
   If a NEW leaf package is introduced, also add it to `_SUBMODULES` in
   `package_name/_gen_registry.py` (order decides name-collision winners).

### When modifying an existing function:
- Follow the same workflow as creating a function
- Update existing test and example files if they exist; create them if not
- Ensure backwards compatibility: existing examples must still work after modifications (unless explicitly told otherwise)

### When asked to create tests only:
Run steps 4-5 above.

### When asked to create examples only:
Run steps 6-7 above.
