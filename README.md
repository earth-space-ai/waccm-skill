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

## Acknowledgments

**Gold-standard references for WACCM** (use these to cross-check anything in this skill):
- WACCM project page (NCAR ACOM): https://www2.acom.ucar.edu/gcm/waccm
- ESCOMP/CAM repository (WACCM lives here): https://github.com/ESCOMP/CAM
- CAM User's Guide (covers WACCM compsets): https://ncar.github.io/CAM/doc/build/html/
- MUSICA chemistry framework: https://www2.acom.ucar.edu/sections/musica

This scaffold exists only because of the work of other people, and any value
it has is borrowed from theirs.

- **NSF NCAR** and the **Atmospheric Chemistry Observations and Modeling
  (ACOM)** + Atmosphere Model Working Group (AMWG) communities for
  developing the Whole Atmosphere Community Climate Model inside
  [ESCOMP/CAM](https://github.com/ESCOMP/CAM), including the high-top
  dynamics, TSMLT chemistry mechanisms, gravity-wave parameterizations, and
  WACCM-specific compsets this skill documents.
- The **MUSICA framework** team for the chemistry pre-processor and the
  WACCM-relevant emissions infrastructure.
- The maintainers of `cam-skill` and `cesm-skill` for the upstream build /
  run mechanics this skill routes users to.
- **Zesen Huang** for [laps-skill](https://github.com/huangzesen/laps-skill),
  the progressive-disclosure layout this repo borrows.

Any errors, oversimplifications, or out-of-date claims in this skill are the
skill author's responsibility, not the upstream community's. This is a
scaffold; operational depth is being filled in iteratively.

## License

MIT (skill content). The CAM/WACCM source code is governed by its own license; see https://github.com/ESCOMP/CAM.
