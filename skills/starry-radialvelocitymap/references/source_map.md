# starry source map: Radialvelocitymap

Generated from source roots:
- `starry`
- `docs`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-radialvelocitymap/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `RVBase`
- `rv`
- `veq`
- `alpha`
- `velocity_unit`
- `Rossiter`

## Fast source navigation
- `rg -n "class RVBase|def rv|veq|alpha|velocity_unit" starry/maps.py`
- `rg -n "def rv\(" starry/kepler.py`
- `rg -n "_RadialVelocityMap" docs/hacks.py`
- `rg -n "test_(system_rv|op_gradients)" tests/greedy tests/lazy`

## Ranked source entry points
- `starry/maps.py` | RV map implementation (`RVBase`) and RV filtering.
- `starry/kepler.py` | System-level RV composition and `keplerian` toggle behavior.
- `docs/hacks.py` | Docs alias `_RadialVelocityMap` used by API docs.
- `tests/greedy/test_system_rv_greedy.py` | Map-vs-system RV equivalence and external orbit consistency.
- `tests/greedy/test_multi_wavelength_greedy.py` | Greedy RV map shape/dispatch checks (`test_rv`, `test_rv_vector`).
- `tests/lazy/test_op_gradients.py` | Lazy-mode RV gradient checks.
- `notebooks/RossiterMcLaughlin.ipynb` | RM-focused examples.
- `notebooks/RVDerivation.ipynb` | RV formulation details and derivation workflow.

## Function-level behavior checks
- `rg -n "class RVBase|def rv\\(|veq|alpha|velocity_unit" starry/maps.py` then confirm RV-map parameters and kernels.
- `rg -n "def rv\\(|keplerian|total" starry/kepler.py` then confirm `System.rv` decomposition toggles.
- `pytest -q tests/greedy/test_system_rv_greedy.py --maxfail=1` then verify map-level and system-level RV consistency.
- `pytest -q tests/lazy/test_op_gradients.py -k "test_rv" --maxfail=1` then verify RV op gradients for probabilistic workflows.
