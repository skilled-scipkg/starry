---
name: starry-secondary
description: This skill should be used when users ask about secondary in starry; it prioritizes documentation references and then source inspection only for unresolved details.
---

# starry: Secondary

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## High-Signal Playbook
### Route conditions
- Use this skill for `starry.Secondary` initialization, orbit-element semantics (`porb` vs `a`, `ecc`, `w/omega`, `Omega`, `inc`), and per-secondary units.
- Route central-body-only questions to `starry-primary`.
- Route multi-body interactions and `System` behavior to `starry-system`.
- Route map-physics questions to map-specific skills.

### Triage questions
- Is the orbit parameterized by `porb` or by `a`?
- Are angle and length units customized?
- Is the secondary map emitted-light, reflected-light, or RV-enabled?
- Is this a single-secondary or multi-secondary system?
- Do you need only forward modeling or linear inversion as well?

### Canonical workflow
1. Instantiate a map with desired physics and shared lazy/eager mode.
2. Build `Secondary` with either `porb` or `a` plus body/orbit properties.
3. Validate orbit-angle semantics (`w` alias `omega`, `Omega`, `inc`) and unit conversions.
4. Add to `System` and run `position(t)`/`flux(t)` smoke tests.
5. If needed, adjust map orientation (`map.inc`, `map.obl`) separately from orbital orientation.

### Minimal working example
```python
import starry

starry.config.lazy = False

planet = starry.Secondary(
    starry.Map(ydeg=1, amp=1e-2),
    porb=3.0,
    r=0.1,
    m=1e-3,
    t0=0.0,
    ecc=0.1,
    w=60.0,
    Omega=20.0,
    inc=88.0,
)

planet.omega = 75.0
assert planet.w == 75.0
```

### Pitfalls and fixes
- You must provide one of `porb` or `a`: passing neither raises `ValueError`.
- Setting `porb` clears `a`, and setting `a` clears `porb`: treat them as mutually exclusive parameterizations.
- Orbit angles are in `angle_unit`: convert explicitly when mixing radians/degrees.
- System construction fails if secondaries use incompatible map modes (`rv` mismatch, reflected mixed with non-reflected, spectral-bin mismatch).
- Oblate secondary bodies are currently unsupported in `System`.

### Convergence and validation checks
- Compare trajectories from `position(t)` under small perturbations in `ecc`, `w`, `inc`, and `Omega`.
- If using `a` instead of `porb`, verify derived period behavior in `System` against expected Kepler scaling.
- For transits/eclipses, increase time resolution until ingress/egress timing is stable.
- In multi-body setups, check each secondary contribution using `System.flux(total=False)`.

### Evidence anchors
- `docs/Secondary.rst`
- `docs/Kepler.rst`
- `starry/kepler.py`
- `tests/greedy/test_units.py`
- `tests/greedy/test_system_greedy.py`
- `tests/greedy/test_exceptions_greedy.py`

## Scope
- Handle questions about documentation grouped under the 'secondary' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/Secondary.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/starry-secondary/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/starry-secondary/references/source_map.md` and start with the ranked source entry points.
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
- `tests/greedy/test_system_greedy.py`
- `tests/greedy/test_exceptions_greedy.py`
- `notebooks/Exoplanet.ipynb`
- `notebooks/Orientation.ipynb`
- Prefer targeted source search (for example: `rg -n "class Secondary|porb|a|ecc|omega|Omega|inc" starry tests notebooks`).
