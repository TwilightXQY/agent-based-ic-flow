---
name: openroad-physical-design
description: Plan, debug, and explain digital ASIC physical design with OpenROAD or OpenROAD-based flows. Use when Codex needs to work from Verilog or netlists, SDC, Liberty, LEF or DEF, OpenROAD Tcl scripts, reports, or databases for floorplanning, placement, CTS, routing, extraction, congestion or timing triage, or migration of the flow onto a private PDK such as TSMC28 without exposing NDA data.
---

# OpenROAD Physical Design

## Overview

Use this skill to reason about OpenROAD as a generic digital backend flow, not as a Sky130-only tutorial. Start from the files the user already has: RTL or netlists, SDC, Liberty, LEF, DEF, OpenROAD Tcl, logs, reports, and database snapshots.

Treat private-node data such as TSMC28 platform files, RC corners, and signoff decks as local ground truth. Never invent missing rule values, layer stacks, or library names.

## Workflow

1. Establish the control surface.
   Determine whether the repo uses standalone `openroad` Tcl, `OpenROAD-flow-scripts`, a custom wrapper, or only existing reports and database files.
2. Validate the minimum inputs.
   Prefer a manifest with top module, Verilog or netlist paths, SDC, Liberty, tech LEF, cell LEF, macro LEF or DEF, and any failing logs or reports.
3. Choose the task mode.
   Work as bring-up, stage debug, or signoff triage instead of mixing all three at once.
4. Walk the flow stage by stage.
   Inspect floorplan, placement, CTS, routing, and finishing in order unless a later-stage artifact already proves where the failure lives.
5. Reduce failures to a narrow reproducer.
   Prefer one Tcl script, one stage, and one failing report over broad reruns.
6. Keep private-PDK handling abstract.
   Reason about wrapper scripts, file manifests, and reports when the platform is NDA-bound; do not rewrite proprietary tech data unless it is already available locally.

## What To Read

- Read [references/playbook.md](references/playbook.md) for the stage-by-stage procedure, common commands, and TSMC28/private-PDK guardrails.
- Read [references/upstream-doc-map.md](references/upstream-doc-map.md) when you need the right upstream OpenROAD file quickly.

## Default Habits

- Start with existing reports and logs before touching build scripts.
- Check clock and constraint correctness before tuning placement or routing knobs.
- Revisit floorplan, utilization, macro spacing, and pin density before blaming the router for congestion.
- Use OpenROAD database I/O commands when you can inspect a saved state instead of rerunning from scratch.
- Summarize root cause candidates in terms of artifacts the user can verify: a report, a log line, a Tcl command, or a specific stage output.

## Guardrails

- Do not fabricate PDK-specific layer names, RC data, antenna limits, or signoff thresholds.
- Do not recommend Sky130-specific fixes unless the local flow actually uses Sky130.
- Do not expose private PDK file contents when a file manifest or wrapper path is enough.
