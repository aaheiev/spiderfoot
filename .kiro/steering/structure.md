# Project Structure

## Top-level layout

```
sf.py            # Entry point: web server + scan launcher
sfcli.py         # Command-line client
sflib.py         # SpiderFoot helper class (self.sf inside modules)
sfscan.py        # Scan orchestration
sfwebui.py       # CherryPy web UI controller
VERSION          # Current version string
requirements.txt # Runtime dependencies (pinned ranges)

spiderfoot/      # Core library package
modules/         # 230+ scanner modules (sfp_*.py)
correlations/    # YAML correlation rules
test/            # unit / integration / acceptance test suites
docs/            # Sphinx documentation
```

## Core package (`spiderfoot/`)

- `plugin.py` — `SpiderFootPlugin` base class every module extends.
- `event.py` — `SpiderFootEvent`, the unit of data passed between modules.
- `target.py` — `SpiderFootTarget`, the scan target and its aliases.
- `db.py` — SQLite persistence layer.
- `correlation.py` — correlation rule engine.
- `helpers.py`, `logger.py`, `threadpool.py` — shared utilities.
- `__init__.py` re-exports the public names (`SpiderFootPlugin`, `SpiderFootEvent`,
  `SpiderFootTarget`, etc.) so modules import from `spiderfoot` directly.

Import core types from the package root:

```python
from spiderfoot import SpiderFootEvent, SpiderFootPlugin
```

## Modules (`modules/`)

- Every module file is named `sfp_<name>.py` and defines a class with the same name
  `sfp_<name>` that subclasses `SpiderFootPlugin`.
- Modules are discovered by filename convention; keep the filename, class name, and
  the `meta['name']` consistent.
- See `.kiro/steering/modules.md` for the module authoring contract.

## Correlation rules (`correlations/`)

- One `.yaml` file per rule. `template.yaml` is the annotated reference and the
  existing rules are good starting points.
- `README.md` in that folder is the full rule-set reference.

## Tests (`test/`)

- `test/unit/` — unit tests, run on every PR.
- `test/integration/` — integration tests; `test/integration/modules/` hits live
  third-party sources and is excluded from the default CI run.
- `test/acceptance/` — Robot Framework browser tests against a running web UI.
- `test/run` is the canonical local test script.

## Conventions for new files

- New scanner modules go in `modules/` as `sfp_<name>.py`.
- New correlation rules go in `correlations/` as `<name>.yaml`.
- New tests mirror the source layout under the matching `test/` subdirectory.
- Do not add files outside the workspace root.
