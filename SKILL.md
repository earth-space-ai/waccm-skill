---
name: waccm
description: >
  Progressive-disclosure skill for the Whole Atmosphere Community Climate
  Model (WACCM), a configuration of CAM (NCAR/ESCOMP) that extends the
  model top to ~140 km and includes interactive middle-atmosphere
  chemistry. Covers WACCM and WACCM6/CAM6 compsets, gravity wave drag,
  middle-atmosphere chemistry preprocessor, and the differences between
  standard CAM, CAM-Chem, WACCM, and WACCM-X. Routes into cam-skill
  for build/run mechanics.
version: 0.1.0-scaffold
tags:
  - earth-science
  - climate-model
  - middle-atmosphere
  - stratosphere
  - mesosphere
  - chemistry
  - waccm
  - cesm
  - cam
  - ncar
---

# WACCM (Whole Atmosphere Community Climate Model) Guide

> **WACCM** = Whole Atmosphere Community Climate Model, a configuration of CAM in CESM that extends to the lower thermosphere with interactive chemistry.
> Maintainer: NSF NCAR / ACOM (Atmospheric Chemistry Observations and Modeling) and AMWG
> Source: WACCM lives **inside** [ESCOMP/CAM](https://github.com/ESCOMP/CAM) (no separate repo)
> Chemistry preprocessor: [ESCOMP/CHEM_PREPROCESSOR](https://github.com/ESCOMP/CHEM_PREPROCESSOR)
> Docs: https://www2.acom.ucar.edu/gcm/waccm
> Skill author: Koutian Wu (ktwu01@gmail.com)
> Skill version: 0.1.0-scaffold

**Important:** WACCM is **not a separate code repository**. It is a set of compsets, namelist options, and chemistry mechanisms within the CAM source tree. To get and build WACCM you follow the standard CESM/CAM workflow and select a WACCM compset.

**What WACCM does:** Extends the CAM atmosphere to ~140 km altitude (lower thermosphere), with interactive stratosphere–mesosphere chemistry, parameterized gravity wave drag (orographic, frontal, convective sources), and prognostic ozone. Used for stratospheric and mesospheric studies, ozone trends, sudden stratospheric warming, solar/volcanic forcing, and chemistry–climate interaction.

**Who this skill is for:** Researchers running CESM with a WACCM compset who need to understand what is different from standard CAM: which compset, which chemistry mechanism, which gravity-wave parameterization, which output fields.

---

## Quick Decision Tree

```
"What do I need?"
│
├─ 🆕 What is WACCM, and how is it different from CAM?
│  └─ Read: reference/what-is-waccm.md
│     (Vertical extent, chemistry, history of WACCM1–WACCM6)
│
├─ 🚀 I want to build and run WACCM
│  └─ Step 1: Use cam-skill / cesm-skill for build mechanics
│     Step 2: Read reference/compsets.md for WACCM compset selection
│
├─ 🧪 I need to understand the chemistry mechanism
│  └─ Read: reference/chemistry.md
│     (TS1, TSMLT, MOZART, preprocessor)
│
├─ 🌬️ Gravity wave drag and middle-atmosphere dynamics
│  └─ Read: reference/gravity-waves.md
│
├─ 📊 What history fields does WACCM add over CAM?
│  └─ Read: reference/output-fields.md
│
└─ 🐛 My WACCM run crashes in chemistry / dynamics
   └─ Read: reference/debugging.md
```

---

## Where the Code Lives

WACCM source files are scattered through `ESCOMP/CAM`:

- `src/chemistry/mozart/`, chemistry solver and species list
- `src/chemistry/pp_*`, preprocessed chemistry packages (TS1, TSMLT, etc.)
- `src/physics/cam/`, CAM physics shared with WACCM
- `src/physics/waccm/`, WACCM-specific physics. Note: ion drag and ambipolar diffusion belong to the **WACCM-X** extension, not standard WACCM (see waccmx-skill).
- `src/physics/cam/gw_*`, gravity wave drag
- `bld/namelist_files/`, WACCM namelist defaults (look for `*waccm*` files)

The chemistry mechanism itself is generated **outside** the CAM repo by the [chemistry preprocessor](https://github.com/ESCOMP/CHEM_PREPROCESSOR) and committed back to `src/chemistry/pp_<mech>/`.

---

## Compsets at a Glance

| Compset prefix | Atmosphere | Chemistry | Vertical | Use |
|---|---|---|---|---|
| `F`...`CAM6` | CAM6 | none / linoz | 32 levels, ~40 km top | climate |
| `F`...`CAM6_chem` | CAM6 | tropospheric MOZART | 32 levels | trop chem |
| `F`...`WACCM6` | CAM6 | TSMLT (full middle-atmos) | 110 levels (L110), ~140 km top | strat/meso climate |
| `F`...`WACCM_SC` | CAM | linoz / specified chem | 70 or 110 levels (check tag) | "specified-chemistry" WACCM |
| `B`...`WACCM6` | CAM6 | TSMLT | 110 levels | fully coupled |

**Vertical resolution gotcha:** the standard CESM2 WACCM6 vertical grid is **L110** (110 hybrid levels reaching ~140 km), not 70. The 70-level grid was the WACCM4/5 (CESM1) standard. Initial-condition (`ncdata`) files must match the level count; you cannot interpolate a 32-level CAM IC into an L110 WACCM grid without serious shocks and chemical imbalance.

Use `cime/scripts/query_config --compsets cam` to see the full live list in your installed tag.

---

## Critical Rules

1. **WACCM is selected by compset, not by build flag.** You don't compile a separate WACCM binary; you create a case with a WACCM compset.
2. **Vertical resolution matters.** WACCM6 (CESM2) uses **L110** (110 hybrid levels, ~140 km top). Older WACCM (CESM1) used 70 levels. Cases at the wrong vertical resolution silently misbehave or fail at IC interpolation. Match your `ncdata` to your level count.
3. **Chemistry is expensive.** A WACCM-TSMLT case can cost ~5–10x a CAM-only case at the same horizontal resolution. Plan compute accordingly.
4. **Specified chemistry (WACCM-SC)** is a much cheaper option for studies that don't need interactive chemistry feedback.
5. **For ionosphere/thermosphere extension, use WACCM-X.** See [waccmx-skill](https://github.com/Earth-Space-Modeling-skills/waccmx-skill).

---

## Reference Index

| File | Topic |
|---|---|
| reference/what-is-waccm.md | History, scope, vertical extent |
| reference/compsets.md | WACCM compsets and namelist defaults |
| reference/chemistry.md | TSMLT, MOZART, preprocessor |
| reference/gravity-waves.md | GW parameterizations (orographic, frontal, convective) |
| reference/output-fields.md | WACCM-specific history variables |
| reference/debugging.md | Common WACCM failures |

## Critical agent gotchas (Gemini-reviewed)

- **WACCM is "middle atmosphere" not "thermosphere".** Top is ~140 km. For ionosphere/thermosphere physics, use WACCM-X.
- **`ncdata` (initial conditions) must match the L110 vertical grid.** Do not try to interpolate from a 32-level CAM IC.
- **History file size scales fast.** L110 with full chemistry generates very large output. Use `fincl1`, `fincl2`, etc., to control which fields go to which `h*` stream, and `avgflag_pertape` for averaging vs instantaneous.
- **Internally generated QBO** depends on parameterized non-orographic gravity wave drag tuning. Wrong GW launch parameters are a common reason a WACCM run has wrong stratospheric climate.
- **Specified-dynamics WACCM (SD-WACCM)** is a separate workflow, requires nudging input data, and is the right choice when you need realistic large-scale dynamics for a chemistry-dominated study.

## Status

Scaffold (v0.1.0-scaffold). Source-grounded surface and routing, with Gemini critique pass on 2026-05-09 to correct vertical-level counts and code-path attribution. Operational depth being filled in.
