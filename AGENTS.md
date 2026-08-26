# Project instructions

## Mistake Log

### 2026-08-27 - Skill validator lacked PyYAML
**Tried:** Ran the bundled `quick_validate.py` script with plain `uv run python`.
**Why it failed:** The isolated environment did not include the `yaml` module.
**Don't do this:** Run the validator with `uv run --with pyyaml python` so the dependency is explicit.
