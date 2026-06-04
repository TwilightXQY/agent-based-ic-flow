---
name: ic-eda-flow
description: >
  Global IC/EDA workflow router. Use automatically for any analog, mixed-signal,
  digital IC, ASIC, or EDA task, including xschem, ngspice, Magic, SPICE decks,
  PDK/model includes, Verilog-A, OpenVAF, EVAS, transistor models, gm/ID sizing,
  SAR ADC, StrongArm comparator, bootstrap switch, LDO, HDL verification with
  Verilator/cocotb, and OpenROAD physical design. Also use for private-PDK work
  such as TSMC28 when the task involves schematics, simulation, layout,
  extraction, HDL, timing, routing, or signoff-style reports.
---

# IC/EDA Flow

This is the single entry point for IC and EDA work. Keep it loaded first, then
read only the bundled specialist material needed for the current task.

## First Move

1. Classify the task before giving commands:
   - analog/custom IC schematic, SPICE simulation, layout, extraction, or PDK setup
   - Verilog-A authoring, Verilog-A simulation, compact-model compilation, or behavioral simulation
   - transistor characterization, gm/ID sizing, or model-library selection
   - SAR ADC or sub-block design such as comparator, bootstrap switch, or LDO
   - HDL lint/simulation/verification or digital physical design
2. Gather the controlling local artifacts:
   schematics, netlists, model includes, PDK paths, `.va` files, testbenches,
   Tcl scripts, Liberty/LEF/DEF/SDC, logs, reports, and waveforms.
3. Treat local/private PDK data as authoritative. Do not invent device names,
   BSIM parameters, layer stacks, extraction rules, timing corners, or signoff
   constraints.
4. Run the narrowest meaningful check first: lint, netlist, operating point,
   one transient/DC/AC run, one cocotb test, or one OpenROAD stage.

## Specialist Material

The fine-grained IC/EDA guides are bundled under [subskills/](subskills/).
Read [references/routing-map.md](references/routing-map.md) to choose the exact
guide. Load only the matching `subskills/<name>/SKILL.md` and its directly
referenced files.

## Operating Rules

- Prefer existing project scripts and decks over writing new flows.
- Copy reusable assets into the project working directory before editing them.
  Do not modify bundled skill assets unless the user explicitly asks to update
  this skill pack itself.
- For private PDKs, reason from visible manifests, wrappers, logs, reports, and
  local files. Keep proprietary values out of explanations unless already shown
  by the user or local artifacts.
- For simulations, record simulator, model corner, supplies, stimulus, and output
  files so results can be reproduced.
- When a tool is missing, state the missing executable or dependency and offer
  the minimal install or fallback path.
