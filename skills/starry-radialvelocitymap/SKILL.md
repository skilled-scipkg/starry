---
name: starry-radialvelocitymap
description: This skill should be used when users ask about radialvelocitymap in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: Radialvelocitymap

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for velocity-weighted map modeling with `starry.Map(..., rv=True)`, including `veq`, differential rotation (`alpha`), and RM-style signals.
- Route emitted-light map-only questions to `starry-sphericalharmonicmap`.
- Route reflected-light questions to `starry-reflectedlightmap`.
- Route multi-body RV decomposition and Keplerian coupling to `starry-system`.

### Triage questions
- Is the target standalone map RV (`map.rv`) or system RV (`sys.rv`)?
- Should Keplerian stellar motion be included (`keplerian=True`) or removed for anomaly-only modeling?
- Are all bodies in the system RV-enabled maps?
- What units are required for velocity (`velocity_unit`)?
- Is differential rotation needed (`alpha > 0`)?

### Canonical workflow
1. Instantiate an RV map with `starry.Map(..., rv=True, veq=..., alpha=...)`.
2. Set map coefficients and limb darkening as needed.
3. Compute map-level velocity signal with `map.rv(...)` and visualize with `render(rv=True)`.
4. For orbit-coupled RV, build `Primary`/`Secondary` RV maps and call `System.rv(...)`.
5. Use `keplerian=False` when isolating RM/tomography anomalies.
6. Validate decomposition (`total=False`) and unit consistency.

### Minimal working example
```python
import numpy as np
import starry

starry.config.lazy = False

m = starry.Map(ydeg=1, udeg=2, rv=True, veq=5e4, alpha=0.1)
m[1, 0] = 0.2

theta = np.linspace(0.0, 360.0, 256)
rv_map = m.rv(theta=theta)

A = starry.Primary(starry.Map(udeg=2, rv=True, amp=1.0, veq=5e4), m=1.0, r=1.0, prot=1.0)
b = starry.Secondary(starry.Map(rv=True, amp=0.0, veq=0.0), porb=2.0, r=0.1, m=1e-3)
sys = starry.System(A, b)
rv_sys = sys.rv(np.linspace(-0.1, 0.1, 200), keplerian=True, total=True)
```

### Pitfalls and fixes
- `System.rv` asserts if any map lacks `rv=True`: ensure RV mode is enabled for all bodies.
- `veq` does not auto-update when body `r`/`prot` changes inside a system: explicitly reset `map.veq`.
- `sys.rv(total=False)` returns contributions to stellar RV, not each body's own barycentric velocity.
- Mixed lazy/eager modes across maps can break evaluation flow: keep mode consistent.
- If you want standard intensity maps, call `intensity(rv=False)`/`render(rv=False)`.

### Convergence and validation checks
- For a single transiter, compare `map.rv(...)` against `sys.rv(..., keplerian=False)`.
- Verify `sys.rv(total=True)` equals sum over rows of `sys.rv(total=False)`.
- Test sensitivity to temporal resolution near transit ingress/egress.
- Confirm unit conversion by changing `velocity_unit` and checking scaling.
- Compare Keplerian-only baseline against external orbital RV implementations when relevant.

### Evidence anchors
- `docs/RadialVelocityMap.rst`
- `docs/MapFactory.rst`
- `docs/hacks.py`
- `starry/maps.py`
- `starry/kepler.py`
- `tests/greedy/test_system_rv_greedy.py`
- `notebooks/RossiterMcLaughlin.ipynb`

## Scope
- Handle questions about documentation grouped under the 'radialvelocitymap' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/RadialVelocityMap.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-radialvelocitymap/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-radialvelocitymap/references/source_map.md` and start with the ranked source entry points.
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
- `tests/greedy/test_system_rv_greedy.py`
- `tests/lazy/test_op_gradients.py`
- `notebooks/RossiterMcLaughlin.ipynb`
- `notebooks/RVDerivation.ipynb`
- Prefer targeted source search (for example: `rg -n "class RVBase|def rv|veq|alpha|_set_RV_filter|System\.rv" starry tests notebooks`).
