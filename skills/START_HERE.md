# starry Skills: Start Here

Use this file when an agent begins in `skills/` and needs a reliable first path to simulation work.

## Bootstrap from `skills/`
1. `cd "$(git rev-parse --show-toplevel)"`
2. `python -m pip install -e .`
3. `python -c "import starry; print('starry import OK')"`
4. `python -c "import starry; starry.config.lazy=False; m=starry.Map(ydeg=1); print(float(m.flux(theta=0.0)))"`

Validation checkpoints:
- Step 2 completes without build/install errors.
- Step 3 prints `starry import OK`.
- Step 4 returns one finite flux value.

## Route to the right topic skill
- System-level simulation, solves, RV coupling: `skills/starry-system/SKILL.md`
- Primary body setup and units: `skills/starry-primary/SKILL.md`
- Secondary orbit/body setup: `skills/starry-secondary/SKILL.md`
- Emitted-light spherical harmonics: `skills/starry-sphericalharmonicmap/SKILL.md`
- Reflected-light phase/occultation workflows: `skills/starry-reflectedlightmap/SKILL.md`
- Radial-velocity map workflows: `skills/starry-radialvelocitymap/SKILL.md`
- Install/config/API/advanced modules: `skills/starry-advanced-topics/SKILL.md`

## For function-level verification
- Open the selected topic source map file (for example `skills/starry-system/references/source_map.md`).
- Run its `rg` command(s) first to locate functions/classes.
- Run its listed `pytest` smoke checks before broad simulations.
