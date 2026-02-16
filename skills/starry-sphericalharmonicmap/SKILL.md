---
name: starry-sphericalharmonicmap
description: This skill should be used when users ask about sphericalharmonicmap in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: Sphericalharmonicmap

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for emitted-light spherical-harmonic maps from `starry.Map(...)` with `rv=False`, `reflected=False`, and `oblate=False`.
- Route reflected-light behavior to `starry-reflectedlightmap`.
- Route velocity-weighted behavior to `starry-radialvelocitymap`.
- Route system-level orbit coupling and solves to `starry-system`.

### Triage questions
- What degree/resolution is needed (`ydeg`, optionally `udeg`, optionally `nw`)?
- Are you doing forward flux modeling, inversion (`design_matrix`), rendering, or all three?
- Is evaluation lazy or greedy?
- Are occultations included (`xo`, `yo`, `zo`, `ro`) or pure rotation (`theta`)?
- Is map orientation controlled via `inc/obl` and does that match orbit geometry?

### Canonical workflow
1. Instantiate map with `starry.Map(ydeg=..., udeg=..., nw=...)`.
2. Set non-constant coefficients via `map[l, m]` and set flux scale via `map.amp`.
3. Generate a forward model with `flux(...)` and/or `render(...)`.
4. Build `design_matrix(...)` for linear inversion or fast repeated evaluations.
5. Validate `flux` against `amp * A y` for representative points.
6. Increase degree/time sampling until convergence checks pass.

### Minimal working example
```python
import numpy as np
import starry

starry.config.lazy = False

m = starry.Map(ydeg=2, udeg=1)
m[1, 0] = 0.2
m[2, 1] = -0.05

theta = np.linspace(0.0, 360.0, 200)
A = m.design_matrix(theta=theta)
flux = m.flux(theta=theta)
flux_lin = m.amp * A.dot(m.y)

assert np.allclose(flux, flux_lin)
img = m.render(res=200, projection="ortho", theta=30.0)
```

### Pitfalls and fixes
- `Y_{0,0}` cannot be set directly (except when assigning the full coefficient vector): change `map.amp` instead.
- `u_0` cannot be set directly: only set higher-order limb-darkening coefficients.
- Invalid index types/ranges raise `ValueError` (or silently no-op in some out-of-range `m` cases): validate index arrays early.
- Combined physics flags (`rv`, `reflected`, `oblate`) are not jointly supported: choose exactly one mode family.
- Multi-wavelength limb-darkened maps (`ydeg=0`, `udeg>0`, `nw!=None`) are unsupported.

### Convergence and validation checks
- Increase `ydeg` until target observables (`flux`, inferred coefficients) stabilize.
- Verify rendering convergence by increasing `res`.
- For rotational curves, increase `theta` sampling density and check peak/trough stability.
- Cross-check `flux` against `design_matrix` for random test points.
- In inverse problems, monitor residual structure to detect underfit degree choice.

### Evidence anchors
- `docs/SphericalHarmonicMap.rst`
- `docs/MapFactory.rst`
- `docs/hacks.py`
- `starry/maps.py`
- `tests/greedy/test_exceptions_greedy.py`
- `tests/greedy/test_indices_greedy.py`
- `tests/greedy/test_multi_wavelength_greedy.py`

## Scope
- Handle questions about documentation grouped under the 'sphericalharmonicmap' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/SphericalHarmonicMap.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-sphericalharmonicmap/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-sphericalharmonicmap/references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `notebooks`

## Test references
- `tests`
- `starry/extensions/tests`

## Optional deeper inspection
- `starry`

## Source entry points for unresolved issues
- `starry/maps.py`
- `docs/hacks.py`
- `starry/legacy.py`
- `tests/greedy/test_indices_greedy.py`
- `tests/greedy/test_exceptions_greedy.py`
- `tests/greedy/test_multi_wavelength_greedy.py`
- `notebooks/Basics.ipynb`
- Prefer targeted source search (for example: `rg -n "class YlmBase|def flux|def design_matrix|def render|def intensity" starry/maps.py`).
