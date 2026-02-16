---
name: starry-system
description: This skill should be used when users ask about system in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: System

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for `starry.System` construction, orbital flux/RV evaluation, exposure-time integration, and system-level linear solves (`set_data`, `solve`, `lnlike`, `draw`).
- Route single-body parameter questions to `starry-primary` or `starry-secondary`.
- Route map-physics questions (`rv`, reflected-light, spherical harmonics) to `starry-radialvelocitymap`, `starry-reflectedlightmap`, or `starry-sphericalharmonicmap`.
- Route install/config/API navigation to `starry-advanced-topics`.

### Triage questions
- How many secondaries are modeled, and do they all share compatible map modes (`rv`, reflected, spectral bins)?
- Are orbit elements provided as `porb` or `a` for each secondary?
- Is finite exposure integration needed (`texp`, `oversample`, `order`)?
- Is light-travel delay required (`light_delay=True`)?
- Do you need total system flux/RV or per-body contributions (`total=False`)?
- Are you solving an inverse problem (priors + covariance) or just forward simulation?

### Canonical workflow
1. Instantiate map objects with consistent mode flags across bodies.
2. Build `Primary` and one or more `Secondary` objects.
3. Construct `starry.System(primary, *secondaries, texp=..., oversample=..., order=..., light_delay=...)`.
4. Validate baseline behavior with `position(t)` and `flux(t)` on a small time grid.
5. For RV workflows, call `rv(t, keplerian=..., total=...)` after ensuring all maps were built with `rv=True`.
6. For inference, set per-map priors (`map.set_prior(...)`) and attach data with `sys.set_data(flux, C=... or cho_C=...)`.
7. Solve with `sys.solve(t=t)` (or pass a precomputed design matrix), then sample with `sys.draw()` as needed.
8. Re-check with `sys.lnlike(...)` and convergence checks below.

### Minimal working example
```python
import numpy as np
import starry

starry.config.lazy = False

A = starry.Primary(starry.Map(ydeg=1), prot=0.0)
b = starry.Secondary(starry.Map(amp=0.0), porb=1.0, r=0.1, t0=-0.05, Omega=30.0)
c = starry.Secondary(starry.Map(amp=0.0), porb=1.0, r=0.1, t0=0.05, Omega=-30.0)
sys = starry.System(A, b, c, texp=0.0)

t = np.linspace(-0.1, 0.1, 200)
flux = sys.flux(t)

A.map.set_prior(L=1.0)
sys.set_data(flux, C=1e-6)
mu, cho_cov = sys.solve(t=t)
```

### Pitfalls and fixes
- `System` needs at least one secondary: pass `starry.System(primary, secondary, ...)`.
- Primary must be `starry.Primary` and cannot use reflected maps: build primary with emitted/RV/oblate maps only.
- Mixed map modes are invalid: keep `rv` enabled for all maps or none; keep reflected mode consistent across all secondaries.
- `texp < 0`, `oversample <= 0`, or `order` not in `{0,1,2}` triggers assertions: sanitize these before construction.
- `sys.solve()` fails without `set_data()` and at least one prior-enabled body: call `set_data` plus `map.set_prior` first.
- Spectral map support is incomplete for some system-level inference methods: avoid spectral solves unless implementation explicitly supports it.

### Convergence and validation checks
- Increase temporal sampling and confirm `flux(t)`/`rv(t)` change below tolerance.
- For finite exposures, compare increasing `oversample` and switching `order=0/1/2`; require stability of transit/eclipse integrals.
- Verify `rv(total=True)` matches row-sum of `rv(total=False)`.
- Check period consistency when any secondary uses `a` (Kepler conversion uses body masses and units).
- For linear solves, inspect residuals and ensure posterior draws from `sys.draw()` reproduce the observed flux scale.

### Evidence anchors
- `docs/System.rst`
- `docs/Kepler.rst`
- `starry/kepler.py`
- `tests/greedy/test_system_greedy.py`
- `tests/greedy/test_system_solve_greedy.py`
- `tests/greedy/test_system_rv_greedy.py`
- `tests/greedy/test_exceptions_greedy.py`

## Scope
- Handle questions about documentation grouped under the 'system' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/System.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-system/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-system/references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `notebooks`

## Test references
- `tests`
- `starry/extensions/tests`

## Optional deeper inspection
- `starry`

## Source entry points for unresolved issues
- `starry/kepler.py`
- `starry/maps.py`
- `tests/greedy/test_system_greedy.py`
- `tests/greedy/test_system_solve_greedy.py`
- `tests/greedy/test_system_rv_greedy.py`
- `tests/greedy/test_exceptions_greedy.py`
- `notebooks/Exoplanet.ipynb`
- `notebooks/ExposureTime.ipynb`
- Prefer targeted source search (for example: `rg -n "System|Primary|Secondary|set_data|solve|rv|flux" starry tests notebooks`).
