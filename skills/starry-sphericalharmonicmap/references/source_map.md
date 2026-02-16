# starry source map: Sphericalharmonicmap

Generated from source roots:
- `starry`
- `docs`
- `tests`
- `notebooks`

Use this map only after exhausting the topic docs in `skills/starry-sphericalharmonicmap/references/doc_map.md`.

## Working directory
- If you start in `skills/`, switch to repo root first: `cd "$(git rev-parse --show-toplevel)"`.

## Topic query tokens
- `Map`
- `YlmBase`
- `design_matrix`
- `flux`
- `intensity`
- `render`
- `Y00`

## Fast source navigation
- `rg -n "def Map\(|class YlmBase|class MapBase" starry/maps.py`
- `rg -n "def (design_matrix|flux|intensity|render)" starry/maps.py`
- `rg -n "_SphericalHarmonicMap|CustomBase" docs/hacks.py`
- `rg -n "test_(indices|exceptions|multi_wavelength)" tests/greedy tests/lazy`

## Ranked source entry points
- `starry/maps.py` | `Map` factory and `YlmBase` emitted-light implementation.
- `docs/hacks.py` | Docs alias `_SphericalHarmonicMap` used by Sphinx/autoclass pages.
- `starry/legacy.py` | Parent class behavior inherited by `YlmBase`.
- `tests/greedy/test_indices_greedy.py` | Coefficient indexing behaviors.
- `tests/greedy/test_exceptions_greedy.py` | Indexing/constructor failure modes.
- `tests/greedy/test_multi_wavelength_greedy.py` | Spectral-map behavior checks.
- `tests/lazy/test_multi_wavelength_lazy.py` | Lazy-mode spectral behavior checks.
- `notebooks/Basics.ipynb` | Core spherical-harmonic map usage.
- `notebooks/Orientation.ipynb` | Orientation effects for map rendering/flux.

## Function-level behavior checks
- `rg -n "def Map\\(|class YlmBase|def (design_matrix|flux|intensity|render)\\(" starry/maps.py` then confirm emitted-light map entry points and core methods.
- `rg -n "_SphericalHarmonicMap|CustomBase" docs/hacks.py` then confirm docs aliases map back to runtime classes.
- `pytest -q tests/greedy/test_indices_greedy.py tests/greedy/test_multi_wavelength_greedy.py --maxfail=1` then confirm indexing behavior and multi-wavelength dispatch.
- `pytest -q tests/greedy/test_exceptions_greedy.py -k "bad_ylm_indices or bad_map_types" --maxfail=1` then confirm invalid-index and mixed-mode failure guards.
