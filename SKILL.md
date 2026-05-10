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
- `src/physics/waccm/`, WACCM-specific physics (e.g., ion drag for WACCM-X, see waccmx-skill)
- `src/physics/cam/gw_*`, gravity wave drag
- `bld/namelist_files/`, WACCM namelist defaults (look for `*waccm*` files)

The chemistry mechanism itself is generated **outside** the CAM repo by the [chemistry preprocessor](https://github.com/ESCOMP/CHEM_PREPROCESSOR) and committed back to `src/chemistry/pp_<mech>/`.

---

## Compsets at a Glance

| Compset prefix | Atmosphere | Chemistry | Vertical | Use |
|---|---|---|---|---|
| `F`...`CAM6` | CAM6 | none / linoz | 32 levels, ~40 km top | climate |
| `F`...`CAM6_chem` | CAM6 | tropospheric MOZART | 32 levels | trop chem |
| `F`...`WACCM6` | CAM6 | TSMLT (full middle-atmos) | 70 levels, ~140 km top | strat/meso climate |
| `F`...`WACCM_SC` | CAM | linoz / specified chem | 70 levels | "specified-chemistry" WACCM |
| `B`...`WACCM6` | CAM6 | TSMLT | 70 levels | fully coupled |

Use `cime/scripts/query_config --compsets cam` to see the full live list in your installed tag.

---

## Critical Rules

1. **WACCM is selected by compset, not by build flag.** You don't compile a separate WACCM binary; you create a case with a WACCM compset.
2. **Vertical resolution matters.** A WACCM compset uses 70 (or more) vertical levels. Cases at the wrong vertical resolution will silently misbehave or fail at namelist resolution.
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

## Status

Scaffold (v0.1.0-scaffold). Source-grounded surface and routing. Operational depth being filled in.
