---
name: starry-reflectedlightmap
description: This skill should be used when users ask about reflectedlightmap in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: Reflectedlightmap

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for reflected-light maps (`starry.Map(..., reflected=True)`), including illumination geometry (`xs, ys, zs, rs`), roughness, and reflected flux normalization.
- Route emitted-light map behavior to `starry-sphericalharmonicmap`.
- Route RV map behavior to `starry-radialvelocitymap`.
- Route multi-body coupling and transit/eclipse orchestration to `starry-system`.

### Triage questions
- Is the illumination source point-like (`source_npts=1`) or finite-sized?
- Are you modeling flux, intensity/albedo, rendered images, or all three?
- Is Oren-Nayar roughness needed (`roughness`, `on94_exact`)?
- Are occultations included (`xo`, `yo`, `zo`, `ro`) in addition to illumination?
- Is this standalone map usage or inside a `System` with reflected secondaries?

### Canonical workflow
1. Instantiate `starry.Map(ydeg=..., reflected=True, source_npts=...)`.
2. Set albedo structure via spherical-harmonic coefficients and `amp`.
3. Evaluate `flux(...)` for light curves and `intensity(...)`/`render(...)` for spatial diagnostics.
4. If rough surfaces matter, tune `roughness` and compare `on94_exact=True` against approximation.
5. In systems, keep reflected mode consistent across all secondaries.
6. Validate normalization and geometry scaling with the checks below.

### Minimal working example
```python
import numpy as np
import starry

starry.config.lazy = False

m = starry.Map(ydeg=1, reflected=True, source_npts=1)
m[1, 0] = 0.1

phase = np.linspace(0.0, 360.0, 200)
flux = m.flux(theta=phase, xs=0.0, ys=0.0, zs=1.0)
albedo_center = m.intensity(lat=0.0, lon=0.0, xs=0.0, ys=0.0, zs=1.0, illuminate=False)
```

### Pitfalls and fixes
- `source_npts != 1` is experimental: use only when needed and run convergence tests.
- Reflected maps cannot be combined with `rv=True`/`oblate=True` in the same map construction.
- Illumination/source coordinates are in body-radius units: unit mistakes can bias phase-curve amplitude.
- In `System`, reflected mode must be enabled for all secondaries or none.
- `amp` scales spherical albedo normalization; do not interpret it as emitted-light luminosity.

### Convergence and validation checks
- Verify full-phase normalization (uniform map gives `2/3` flux at `xs=ys=0, zs=1` when `amp=1`).
- Check inverse-square falloff with source distance (`flux ~ 1/zs^2` in simple geometries).
- Increase `source_npts` and ensure light curves are stable for short-period/high-extent illumination cases.
- Compare approximate vs exact Oren-Nayar (`on94_exact=True`) for large roughness.
- Cross-check `render`-integrated flux against `flux()` for representative geometries.

### Evidence anchors
- `docs/ReflectedLightMap.rst`
- `docs/MapFactory.rst`
- `docs/hacks.py`
- `starry/maps.py`
- `tests/greedy/test_reflected_normalization.py`
- `tests/greedy/test_reflected_occultations.py`
- `tests/greedy/test_oren_nayar.py`

## Scope
- Handle questions about documentation grouped under the 'reflectedlightmap' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/ReflectedLightMap.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-reflectedlightmap/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-reflectedlightmap/references/source_map.md` and start with the ranked source entry points.
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
- `starry/kepler.py`
- `docs/hacks.py`
- `tests/greedy/test_reflected_normalization.py`
- `tests/greedy/test_reflected_occultations.py`
- `tests/greedy/test_oren_nayar.py`
- `notebooks/ReflectedLight.ipynb`
- Prefer targeted source search (for example: `rg -n "class ReflectedBase|def flux|def intensity|def render|roughness|source_npts" starry/maps.py`).
