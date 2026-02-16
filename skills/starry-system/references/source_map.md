# starry source map: System

Generated from source roots:
- `starry`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-system/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `system`
- `Primary`
- `Secondary`
- `flux`
- `rv`
- `position`
- `solve`
- `lnlike`

## Fast source navigation
- `rg -n "class (Primary|Secondary|System)|def (flux|rv|position|solve|lnlike)" starry/kepler.py`
- `rg -n "set_data|set_prior|design_matrix" starry/kepler.py starry/maps.py`
- `rg -n "test_(system|exceptions|units)" tests/greedy`

## Ranked source entry points
- `starry/kepler.py` | `Primary`, `Secondary`, `System`, and all system-level forward/inverse methods.
- `starry/maps.py` | Map properties consumed by system ops (`rv`, reflected, oblate, spectral, coefficients).
- `starry/_core/core.py` | Lower-level ops wiring used by `OpsSystem`.
- `tests/greedy/test_system_greedy.py` | Forward-model regression tests (orientation, integration, reflected cases).
- `tests/greedy/test_system_solve_greedy.py` | System linear-solve and likelihood workflows.
- `tests/greedy/test_system_rv_greedy.py` | RV decomposition and external-consistency checks.
- `tests/greedy/test_exceptions_greedy.py` | Constructor and mode-compatibility failure cases.
- `tests/greedy/test_light_delay_greedy.py` | Light-travel delay behavior check.
- `notebooks/Exoplanet.ipynb` | End-to-end system setup example.
- `notebooks/ExposureTime.ipynb` | `texp`/`oversample` usage pattern.
- `notebooks/LightTravelDelay.ipynb` | Light-delay comparison workflow.

## Function-level behavior checks
- `rg -n "class (Primary|Secondary|System)|def (position|flux|rv|set_data|solve|draw|lnlike)\\(" starry/kepler.py` then confirm forward/inverse method locations.
- `rg -n "set_prior|design_matrix|rv|reflected|oblate" starry/maps.py starry/kepler.py` then confirm map-system compatibility pathways.
- `pytest -q tests/greedy/test_system_greedy.py tests/greedy/test_system_solve_greedy.py tests/greedy/test_system_rv_greedy.py --maxfail=1` then confirm flux, solve, and RV workflows.
- `pytest -q tests/greedy/test_light_delay_greedy.py --maxfail=1` then confirm light-delay behavior.
