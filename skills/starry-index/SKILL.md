---
name: starry-index
description: This skill should be used when users ask how to use starry and the correct generated documentation skill must be selected before going deeper into source code.
---

# starry Skills Index

## Route the request
- Start docs-first. Route to the narrowest skill below; only inspect source when docs leave behavior ambiguous.
- Prefer core skills for simulation setup and model behavior (`System`, `Primary`, `Secondary`, spherical-harmonic/reflected/RV maps).
- Route broad package navigation, install/config, and less-common map classes to `starry-advanced-topics`.
- If the agent session begins inside `skills/`, use `skills/START_HERE.md` first.

## Generated topic skills
- `starry-system`: Multi-body Keplerian simulations, system flux/RV/positions, exposure integration, and system-level linear solves.
- `starry-primary`: Central-body parameterization and unit handling for `starry.Primary`.
- `starry-secondary`: Orbiting-body parameterization and orbit element handling for `starry.Secondary`.
- `starry-sphericalharmonicmap`: Emitted-light map behavior from `starry.Map(..., rv=False, reflected=False, oblate=False)`.
- `starry-reflectedlightmap`: Reflected-light maps, illumination geometry, roughness, and reflected flux normalization.
- `starry-radialvelocitymap`: Velocity-weighted maps (`rv=True`) and Rossiter-McLaughlin style modeling.
- `starry-advanced-topics`: Install/build, API overview, tutorials index, config, linalg, DopplerMap, extensions, map factory, limb-darkened/oblate maps, and changelog/history.

## Documentation-first inputs
- `docs`

## Working directory
- If you start in `skills/`, switch to repo root before running file or test commands: `cd "$(git rev-parse --show-toplevel)"`.

## Tutorials and examples roots
- `notebooks`

## Test roots for behavior checks
- `tests`
- `starry/extensions/tests`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, inspect the selected topic doc map (for example `skills/starry-system/references/doc_map.md`).
- If documentation still leaves ambiguity, inspect the selected topic source map (for example `skills/starry-system/references/source_map.md`) and jump into ranked implementation files.
- Use targeted symbol search while inspecting source (for example: `rg -n "<symbol_or_keyword>" starry tests notebooks`).

## Source directories for deeper inspection
- `starry`
- `tests`
- `notebooks`
