# starry source map: Primary

Generated from source roots:
- `starry`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-primary/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `primary`
- `Body`
- `length_unit`
- `mass_unit`
- `time_unit`
- `angle_unit`

## Fast source navigation
- `rg -n "class Body|class Primary|def __init__" starry/kepler.py`
- `rg -n "length_unit|mass_unit|time_unit|angle_unit|_check_kwargs" starry/kepler.py`
- `rg -n "test_(units|misc|exceptions)" tests/greedy`

## Ranked source entry points
- `starry/kepler.py` | `Body` unit/property logic and `Primary` constructor behavior.
- `starry/maps.py` | Map base/type semantics referenced by body validation.
- `tests/greedy/test_units.py` | Unit defaults and conversion checks for body/system properties.
- `tests/greedy/test_misc.py` | Invalid-keyword warning behavior (`_check_kwargs`).
- `tests/greedy/test_exceptions_greedy.py` | Primary misuse in system construction.
- `notebooks/Exoplanet.ipynb` | Practical primary setup in transit/phase-curve context.
- `notebooks/OrbitViz.ipynb` | Primary-secondary visualization workflow.

## Function-level behavior checks
- `rg -n "class Body|class Primary|def _check_kwargs|length_unit|mass_unit|time_unit|angle_unit" starry/kepler.py` then confirm `Primary` and unit-handling logic live in one file.
- `pytest -q tests/greedy/test_units.py -k "default_body_units or unit_conversions or omega_w" --maxfail=1` then confirm body-unit conversions and aliases behave as documented.
- `pytest -q tests/greedy/test_misc.py -k "check_kwargs_body" --maxfail=1` then confirm unknown kwargs are surfaced via warnings.
