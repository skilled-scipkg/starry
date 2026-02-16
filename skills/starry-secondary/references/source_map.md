# starry source map: Secondary

Generated from source roots:
- `starry`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-secondary/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `secondary`
- `porb`
- `a`
- `ecc`
- `w`
- `omega`
- `Omega`
- `inc`

## Fast source navigation
- `rg -n "class Secondary|def porb|def a|def ecc|def w|def Omega|def inc" starry/kepler.py`
- `rg -n "_get_periods|position|System\(" starry/kepler.py`
- `rg -n "test_(units|system|exceptions)" tests/greedy`

## Ranked source entry points
- `starry/kepler.py` | `Secondary` constructor, orbital properties, and period conversion in `_get_periods`.
- `starry/maps.py` | Map-mode flags that must be compatible across secondaries in a system.
- `tests/greedy/test_units.py` | `porb`/`a` semantics, unit conversions, and `w`/`omega` alias checks.
- `tests/greedy/test_system_greedy.py` | Orientation and integration behavior with secondaries.
- `tests/greedy/test_exceptions_greedy.py` | Missing-orbit and mixed-mode failure cases.
- `notebooks/Orientation.ipynb` | Orbit orientation and map orientation interplay.
- `notebooks/Exoplanet.ipynb` | Typical secondary setup for transit/phase curves.

## Function-level behavior checks
- `rg -n "class Secondary|def (porb|a|ecc|w|Omega|inc)\\(" starry/kepler.py` then confirm orbital-parameter setters/getters and aliases.
- `rg -n "_get_periods|position|class System" starry/kepler.py` then confirm period conversion and orbit-position pathways.
- `pytest -q tests/greedy/test_units.py -k "period_semi or omega_w" --maxfail=1` then confirm `porb`/`a` semantics and `w`/`omega` aliasing.
- `pytest -q tests/greedy/test_system_greedy.py tests/greedy/test_exceptions_greedy.py -k "orientation or bad_sys_settings" --maxfail=1` then confirm secondary coupling and failure modes.
