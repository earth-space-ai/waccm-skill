# WACCM Skill

Progressive-disclosure skill for the Whole Atmosphere Community Climate Model (WACCM), a configuration of CAM in CESM that extends to the lower thermosphere with interactive chemistry.

> **WACCM is not a separate repo.** It lives inside [ESCOMP/CAM](https://github.com/ESCOMP/CAM). This skill documents the WACCM-specific compsets, chemistry, dynamics, and output. For build mechanics, use [cam-skill](https://github.com/Earth-Space-Modeling-skills/cam-skill) or [cesm-skill](https://github.com/Earth-Space-Modeling-skills/cesm-skill).

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

## What This Is

A self-contained guide for researchers who already know how to build and run CAM/CESM, and now want to run a WACCM configuration: which compset, which chemistry mechanism, which gravity-wave parameterization, which output fields, and what the common failure modes are.

## Status

Scaffold (v0.1.0-scaffold). Source-grounded routing verified against CAM source tree. Operational depth (specific TSMLT species lists, namelist defaults per CESM tag, common crash modes) is being filled in.

## Related skills in this org

- [cam-skill](https://github.com/Earth-Space-Modeling-skills/cam-skill)
- [cesm-skill](https://github.com/Earth-Space-Modeling-skills/cesm-skill)
- [waccmx-skill](https://github.com/Earth-Space-Modeling-skills/waccmx-skill)

## License

MIT (skill content). The CAM/WACCM source code is governed by its own license; see https://github.com/ESCOMP/CAM.
