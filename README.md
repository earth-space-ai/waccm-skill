# WACCM Skill

Progressive-disclosure skill for the Whole Atmosphere Community Climate Model (WACCM), a configuration of CAM in CESM that extends to the lower thermosphere with interactive chemistry.

> **WACCM is not a separate repo.** It lives inside [ESCOMP/CAM](https://github.com/ESCOMP/CAM). This skill documents the WACCM-specific compsets, chemistry, dynamics, and output. For build mechanics, use [cam-skill](https://github.com/earth-space-ai/cam-skill) or [cesm-skill](https://github.com/earth-space-ai/cesm-skill).

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

> ⚠️ **Disclaimer — please read before using this skill.**
> This skill is **not a gold-standard reference**. It is a helper that lowers
> the barrier for new users to **get their hands dirty** with the model. AI
> agents (and the humans drafting this material) make mistakes; commands, file
> paths, namelist options, and physics explanations here can be wrong,
> incomplete, or out of date. **Always cross-check with the official model
> documentation, the source code, and a human expert before trusting any
> output for research, publication, or operational use.**

## What This Is

A self-contained guide for researchers who already know how to build and run CAM/CESM, and now want to run a WACCM configuration: which compset, which chemistry mechanism, which gravity-wave parameterization, which output fields, and what the common failure modes are.

## Status

Scaffold (v0.1.0-scaffold). Source-grounded routing verified against CAM source tree. Operational depth (specific TSMLT species lists, namelist defaults per CESM tag, common crash modes) is being filled in.

## Related skills in this org

- [cam-skill](https://github.com/earth-space-ai/cam-skill)
- [cesm-skill](https://github.com/earth-space-ai/cesm-skill)
- [waccmx-skill](https://github.com/earth-space-ai/waccmx-skill)

## License

MIT (skill content). The CAM/WACCM source code is governed by its own license; see https://github.com/ESCOMP/CAM.
