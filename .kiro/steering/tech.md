# Technology & Conventions

## Stack

- **Language**: Python 3.7+ (CI runs 3.7–3.10 on Ubuntu and macOS).
- **Web**: CherryPy + cherrypy-cors, Mako templates.
- **Data**: SQLite (via `spiderfoot/db.py`).
- **Key libraries**: dnspython, netaddr, requests, beautifulsoup4, lxml,
  phonenumbers, cryptography, pyOpenSSL, networkx, pyyaml, publicsuffixlist.
- Dependencies are pinned to version ranges in `requirements.txt`; keep new
  dependencies pinned the same way and prefer the libraries already present.

## Environment setup

```bash
pip3 install -r requirements.txt
pip3 install -r test/requirements.txt   # for running tests
```

## Common commands

```bash
# Run the web UI
python3 ./sf.py -l 127.0.0.1:5001

# Run unit + integration tests (excludes live module tests) — same as CI
./test/run

# Lint (must pass for CI to go green)
python3 -m flake8 . --count --show-source --statistics

# Full test run including live module integration tests
python3 -m pytest -n auto --dist loadfile --durations=5 --cov=. test/integration/modules/
```

Prefer `pytest ... --run`-style single runs over watch mode; never leave the web
server or a watcher running in an automated step.

## Linting rules (authoritative config in `setup.cfg` and `.pylintrc`)

- flake8 with max line length **120** (`E501` is in the ignore list but stay under
  120 anyway). Docstring convention is **Google** style (darglint).
- `max-complexity = 60`. A handful of files have targeted `per-file-ignores`; do not
  broaden ignores to work around a lint failure — fix the code instead.
- flake8 must pass with zero violations; CI fails the build on any.

## Style conventions

- Files start with the UTF-8 coding comment and the boxed header block
  (Name / Purpose / Author / Created / Copyright / Licence) seen across `modules/`.
- Class names: modules use the exact `sfp_<name>` form; core classes use
  `CamelCase`. Methods are typically camelCase in this codebase (e.g.
  `handleEvent`, `watchedEvents`, `notifyListeners`) — match the surrounding file
  rather than imposing PEP 8 snake_case on existing APIs.
- Use f-strings for logging/formatting (`self.debug(f"...")`).
- Add type hints and Google-style docstrings on new functions where practical; the
  existing code is partially annotated, so follow the patterns in the file you edit.

## Verification expectation

After changing Python code, run flake8 and the relevant tests before reporting the
change as done. If a change touches a module, run that module's unit test under
`test/unit/`.
