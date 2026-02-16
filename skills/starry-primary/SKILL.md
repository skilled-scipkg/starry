---
name: starry-primary
description: This skill should be used when users ask about primary in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: Primary

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for `starry.Primary` construction, body units (`length_unit`, `mass_unit`, `time_unit`, `angle_unit`), and primary-body map attachment.
- Route orbit-element questions (`porb`, `a`, `ecc`, `w/Omega/inc`) to `starry-secondary`.
- Route multi-body simulation behavior (`System.flux`, `System.rv`, solves) to `starry-system`.
- Route map physics details to map-specific skills (`starry-sphericalharmonicmap`, `starry-reflectedlightmap`, `starry-radialvelocitymap`).

### Triage questions
- What map class is attached to the primary (plain, RV, oblate, reflected)?
- Are custom units needed or defaults acceptable (`Rsun`, `Msun`, `day`, `degree`)?
- Is the primary rotating (`prot`, `theta0`, map `inc/obl`) or static?
- Will this primary be used in a `System` with RV and/or reflected secondaries?
- Is lazy mode needed for probabilistic modeling, or should values be eager numerics?

### Canonical workflow
1. Instantiate a map with the required physics and evaluation mode.
2. Build `starry.Primary(map, r=..., m=..., prot=..., t0=..., theta0=..., units...)`.
3. Validate unit round-trips on body properties and map orientation.
4. If part of a system, ensure map mode compatibility with every secondary.
5. Run a smoke test via `System(...).flux(t)` before larger workflows.

### Minimal working example
```python
import starry
import astropy.units as u

starry.config.lazy = False

m = starry.Map(ydeg=1, inc=60.0)
star = starry.Primary(
    m,
    r=1.0,
    m=1.0,
    prot=25.0,
    t0=0.0,
    theta0=15.0,
    length_unit=u.Rsun,
    mass_unit=u.Msun,
    time_unit=u.day,
    angle_unit=u.degree,
)

assert star.length_unit == u.Rsun
assert star.map.inc == 60.0
```

### Pitfalls and fixes
- Passing non-map objects as `map` fails type checks: always pass an instance returned by `starry.Map(...)`.
- Mixed lazy/eager modes between body and map are invalid: instantiate all maps in a system with the same lazy flag.
- Unknown keyword args are ignored with warnings: remove typos to avoid silent misconfiguration.
- Reflected primary maps are rejected by `System`: keep reflected light on secondaries only.
- Angle/length/mass/time units must have the correct physical type: pass valid `astropy.units` objects.

### Convergence and validation checks
- Round-trip each body property (`r`, `m`, `prot`, `theta0`) after setting custom units.
- Verify primary-only flux baseline (uniform map) before adding secondaries.
- If using RV maps, check that `veq` and map orientation produce expected sign conventions.
- In lazy mode, verify representative expressions `.eval()` cleanly before scaling to full inference models.

### Evidence anchors
- `docs/Primary.rst`
- `docs/Kepler.rst`
- `starry/kepler.py`
- `tests/greedy/test_units.py`
- `tests/greedy/test_misc.py`
- `tests/greedy/test_exceptions_greedy.py`

## Scope
- Handle questions about documentation grouped under the 'primary' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/Primary.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-primary/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-primary/references/source_map.md` and start with the ranked source entry points.
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
- `tests/greedy/test_units.py`
- `tests/greedy/test_misc.py`
- `tests/greedy/test_exceptions_greedy.py`
- `notebooks/Exoplanet.ipynb`
- `notebooks/OrbitViz.ipynb`
- Prefer targeted source search (for example: `rg -n "class Primary|length_unit|mass_unit|time_unit|angle_unit" starry tests notebooks`).
