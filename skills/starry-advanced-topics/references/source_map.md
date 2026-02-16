# starry source map: Advanced Topics

Generated from source roots:
- `starry`
- `tests`
- `notebooks`
- project root build/config files

Use this map only after exhausting the topic docs in `skills/starry-advanced-topics/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `install`
- `api`
- `config`
- `changes`
- `Map`
- `DopplerMap`
- `linalg`
- `extensions`
- `oblate`
- `limb darkened`

## Fast source navigation
- `rg -n "def Map\(|class DopplerMap|class .*Base" starry/maps.py starry/doppler.py`
- `rg -n "lazy|profile|quiet" starry/_config.py docs/config.rst`
- `rg -n "nexsci|extension" starry/extensions docs/extensions.rst`
- `rg -n "install|setup|pyproject|develop" docs/install.rst setup.py pyproject.toml setup.cfg`

## Ranked source entry points
- `setup.py` | Development install entrypoint used by docs (`python setup.py develop`).
- `pyproject.toml` | Build-system and packaging metadata.
- `setup.cfg` | Packaging/test/lint configuration.
- `starry/__init__.py` | Public API exports (`Map`, `DopplerMap`, `Primary`, `Secondary`, `System`).
- `starry/maps.py` | `Map` factory and base-map classes (incl. limb-darkened/oblate/reflected/RV modes).
- `starry/doppler.py` | `DopplerMap` implementation.
- `starry/doppler_solve.py` | Doppler inversion/solve routines.
- `starry/doppler_visualize.py` | Doppler visualization helpers.
- `starry/linalg.py` | Linear algebra helper module referenced by docs.
- `starry/_config.py` | Runtime config flags (`lazy`, `profile`, `quiet`).
- `starry/extensions/nexsci/nexsci.py` | Contributed extension module.
- `tests/greedy/test_doppler_greedy.py` | Doppler map regression test entry point.
- `tests/lazy/test_doppler_lazy.py` | Lazy-mode Doppler behavior check.
- `tests/greedy/test_oblate_system.py` | Oblate-map system behavior checks.
- `tests/greedy/test_ld.py` | Limb-darkening behavior checks.
- `starry/extensions/tests/greedy/test_greedy_nexsci.py` | Extension behavior checks (greedy mode).
- `starry/extensions/tests/lazy/test_lazy_nexsci.py` | Extension behavior checks (lazy mode).

## Function-level behavior checks
- `python -m pip install -e .` then run `python -c "import starry"` and confirm package import before deeper simulation checks.
- `rg -n "def Map\\(|class DopplerMap|def (flux|solve)\\(" starry/maps.py starry/doppler.py` then confirm `Map` and `DopplerMap` entry points are present.
- `rg -n "lazy|profile|quiet" starry/_config.py docs/config.rst` then confirm config flag names match docs and implementation.
- `pytest -q tests/greedy/test_doppler_greedy.py tests/greedy/test_ld.py tests/greedy/test_oblate_system.py --maxfail=1` then confirm advanced-map simulation smoke tests pass.
