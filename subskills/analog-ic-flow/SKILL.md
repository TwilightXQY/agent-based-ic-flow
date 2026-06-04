---
name: analog-ic-flow
description: Plan, debug, and explain analog or custom IC work with xschem, ngspice, and Magic. Use when Codex needs to work from schematics, symbols, SPICE decks, model includes, extracted netlists, Magic layout files, technology files, or macOS/XQuartz build issues for schematic capture, simulation, layout, extraction, or pre/post-layout comparison on public or private PDKs such as TSMC28.
---

# Analog IC Flow

## Overview

Use this skill for the common custom-IC loop: capture or inspect a schematic in xschem, simulate it with ngspice, then move between Magic layout and extracted netlists as needed. Work from the model files, technology files, and include paths that actually exist locally.

Treat private-PDK collateral as authoritative and private. Never invent BSIM parameters, layer rules, device names, or extraction assumptions for TSMC28 or any other NDA-bound node.

## Workflow

1. Determine the task mode.
   Separate schematic or netlisting work, simulation work, and layout or extraction work before suggesting commands.
2. Gather the controlling artifacts.
   Prefer the `.sch` files, symbol or library paths, SPICE decks, model includes, corner names, Magic tech file, and any extracted netlists or DRC output.
3. Stabilize the schematic and netlist first.
   Confirm hierarchy, pin naming, parameters, supplies, and include order before chasing simulator behavior.
4. Simulate from a minimal testbench.
   Start with one operating point, AC, DC, or transient setup, then add corners, sweeps, or extracted parasitics only after the base case is sound.
5. Move to layout and extraction deliberately.
   Open the correct Magic technology, check DRC, extract a netlist, and compare pre- versus post-layout behavior with the same stimulus.
6. Handle macOS tooling explicitly.
   Expect XQuartz and Tcl/Tk details to matter for xschem and Magic on macOS.

## What To Read

- Read [references/toolchain-notes.md](references/toolchain-notes.md) for upstream tool capabilities, install notes, and macOS-specific setup.
- Read [references/workflow.md](references/workflow.md) for the schematic-to-simulation-to-layout loop and common failure patterns.

## Default Habits

- Use the exact local model include paths and technology files instead of public substitutes.
- Reduce simulation failures to one netlist, one analysis, and one corner before expanding the matrix.
- Compare schematic and extracted netlists under the same testbench when layout changes behavior.
- Name missing artifacts precisely: model include, symbol library, Magic tech file, extracted netlist, or corner deck.

## Guardrails

- Do not fabricate model parameters, corner decks, or design-rule values.
- Do not assume Sky130 examples apply to TSMC28 without local confirmation.
- Do not treat GUI build issues as circuit issues; isolate tool-install problems from design behavior.
