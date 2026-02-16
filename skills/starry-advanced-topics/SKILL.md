---
name: starry-advanced-topics
description: This skill should be used for advanced or cross-cutting starry topics that each had only one documentation file in the baseline generation, including install/API/config/changelog and less-common map modules.
---

# starry: Advanced Topics

## Working directory
- If you start in `skills/`, switch to repo root before opening docs/source/test paths: `cd "$(git rev-parse --show-toplevel)"`.

## Scope
- Handle previously one-doc topics consolidated for lower routing noise: install/build, tutorials index, API overview, config, changelog, linalg, DopplerMap, extensions, map factory, Kepler overview, limb-darkened maps, and oblate maps.
- Keep answers docs-first and route simulation-core questions to dedicated core skills.

## Route list inside this skill
- Install/build and environment setup: `docs/install.rst`
- Package/API overview and module entrypoints: `docs/api.rst`, `docs/index.rst`
- Tutorials discovery and notebook routing: `docs/tutorials.rst`
- Runtime configuration (`lazy`, `profile`, `quiet`): `docs/config.rst`
- Release history and behavior shifts: `docs/changes.rst`
- Map factory selection (`Map`, mode flags): `docs/MapFactory.rst`
- Limb-darkened map reference: `docs/LimbDarkenedMap.rst`
- Oblate-map reference: `docs/OblateMap.rst`
- DopplerMap reference: `docs/DopplerMap.rst`
- Linear algebra helper module: `docs/linalg.rst`
- Kepler overview page (router to Primary/Secondary/System): `docs/Kepler.rst`
- Extension module (`nexsci`): `docs/extensions.rst`

## Primary documentation references
- `docs/index.rst`
- `docs/install.rst`
- `docs/tutorials.rst`
- `docs/api.rst`
- `docs/config.rst`
- `docs/changes.rst`
- `docs/MapFactory.rst`
- `docs/Kepler.rst`
- `docs/LimbDarkenedMap.rst`
- `docs/OblateMap.rst`
- `docs/DopplerMap.rst`
- `docs/linalg.rst`
- `docs/extensions.rst`

## Workflow
- Start with the exact doc page listed for the user's subtopic.
- If details are missing, inspect `skills/starry-advanced-topics/references/doc_map.md` for the merged inventory.
- Use `skills/starry-advanced-topics/references/source_map.md` for targeted implementation files (avoid broad vendor tree scans first).
- Route back to core skills (`starry-system`, `starry-primary`, `starry-secondary`, map-specific core skills) when the request becomes simulation-behavior specific.

## Tutorials and examples
- `notebooks`

## Test references
- `tests`
- `starry/extensions/tests`

## Optional deeper inspection
- `starry`

## Source entry points for unresolved issues
- `setup.py`
- `pyproject.toml`
- `setup.cfg`
- `starry/__init__.py`
- `starry/maps.py`
- `starry/kepler.py`
- `starry/doppler.py`
- `starry/linalg.py`
- `starry/_config.py`
- `starry/extensions/nexsci/nexsci.py`
- Prefer targeted source search (for example: `rg -n "Map\(|DopplerMap|config|lazy|profile|quiet|linalg|nexsci" starry docs tests notebooks`).
