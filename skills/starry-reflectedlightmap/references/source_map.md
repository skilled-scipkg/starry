# starry source map: Reflectedlightmap

Generated from source roots:
- `starry`
- `docs`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-reflectedlightmap/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `ReflectedBase`
- `reflected`
- `source_npts`
- `roughness`
- `on94_exact`
- `illumination`
- `albedo`

## Fast source navigation
- `rg -n "class ReflectedBase|def (flux|intensity|render)|source_npts|roughness" starry/maps.py`
- `rg -n "reflected|_ReflectedLightMap" docs/hacks.py starry/kepler.py`
- `rg -n "test_(reflected|oren_nayar)" tests/greedy tests/lazy`

## Ranked source entry points
- `starry/maps.py` | Reflected-light map implementation (`ReflectedBase`).
- `docs/hacks.py` | Docs alias `_ReflectedLightMap` used in API docs.
- `starry/kepler.py` | System-level reflected flux weighting/coupling.
- `tests/greedy/test_reflected_normalization.py` | Normalization, `1/r^2`, and solve consistency checks.
- `tests/greedy/test_reflected_occultations.py` | Occultation geometry and edge-case tests.
- `tests/greedy/test_oren_nayar.py` | Roughness model continuity and approximation quality.
- `tests/lazy/test_reflected_occultation_derivs.py` | Lazy-mode derivative checks for reflected workflows.
- `notebooks/ReflectedLight.ipynb` | End-to-end reflected-light modeling examples.

## Function-level behavior checks
- `rg -n "class ReflectedBase|def (design_matrix|flux|intensity|render)\\(" starry/maps.py` then confirm reflected-map APIs and their implementation location.
- `rg -n "source_npts|roughness|on94_exact|reflected" starry/maps.py starry/kepler.py docs/hacks.py` then confirm geometry and roughness controls are wired through docs and runtime code.
- `pytest -q tests/greedy/test_reflected_normalization.py tests/greedy/test_reflected_occultations.py tests/greedy/test_oren_nayar.py --maxfail=1` then confirm normalization, occultation geometry, and roughness behavior.
