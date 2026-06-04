# OpenROAD Playbook

## Scope

Use this playbook when the task is digital backend work with OpenROAD or an OpenROAD-based flow on either a public or private PDK.

## Artifact Checklist

Prefer gathering these artifacts before proposing fixes:

- Top module name
- RTL or synthesized netlist paths
- SDC paths
- Liberty paths and corner names
- Technology LEF, standard-cell LEF, and macro LEF or DEF
- Wrapper scripts, Make targets, or Tcl entrypoints
- Existing logs, reports, and saved database files

If the user is on a private node such as TSMC28, work from filenames, manifests, and wrapper scripts first. Avoid asking for full NDA decks unless the local task genuinely requires them.

## Stage Order

### 1. Bring Up The Design Database

Start with the minimum OpenROAD data-loading path when you need a narrow reproducer:

```tcl
read_lef tech.lef
read_lef stdcells.lef
read_verilog top.v
link_design top
write_db top.db
```

If a DEF already exists, prefer:

```tcl
read_lef tech.lef
read_lef stdcells.lef
read_def top.def
write_db top.db
```

Use `write_db` aggressively so later debugging can start from the failing state instead of reloading every input.

### 2. Floorplan

Check these first:

- Die and core area
- Utilization target
- Macro placement and halos
- IO pin density
- Tapcell and welltie insertion
- PDN blockage around macros and channels

Typical symptom cues:

- Severe early congestion usually points to floorplan density or macro spacing.
- Repeated macro or pin-placement conflicts usually point to area assumptions, not routing settings.

### 3. Placement

Focus on:

- Global placement overflow or congestion
- Resizing and buffering side effects
- Detailed placement legality
- Long-wire or fanout hotspots

Check constraints before tuning placement knobs. Bad clocks or IO assumptions often look like placement problems.

### 4. CTS

Inspect:

- Clock tree depth and buffer explosion
- Skew versus hold tradeoffs
- Setup or hold regressions that appear only after CTS

If hold collapses after CTS, look at clock structure and repair behavior before changing route settings.

### 5. Routing

Inspect:

- Global routing overflow
- Detailed routing DRCs
- Antenna repair behavior
- Macro channel starvation

If routing fails near macros, revisit floorplan and obstruction assumptions before searching for detailed-router flags.

### 6. Finishing And Handoff

Inspect:

- Extraction inputs and RC setup
- Final timing reports
- DRC and LVS handoff artifacts
- GDS and abstract outputs

Use existing output artifacts whenever possible instead of rerunning the full flow.

## Useful OpenROAD Entry Points

- Batch mode: `openroad -no_init -exit script.tcl`
- GUI mode: `openroad -gui script.tcl`
- Database inspection: `read_db design.db`
- DEF output: `write_def final.def`
- Abstract generation: `write_abstract_lef final.lef`

## Private-PDK / TSMC28 Habits

- Map the flow around local platform files rather than public examples.
- Keep discussions at the level of LEF, Liberty, SDC, DEF, reports, and Tcl wrappers unless the user explicitly provides deeper tech data.
- Name missing artifacts precisely, for example: technology LEF, macro LEF, liberty corner, RC deck, or clock constraint file.
- When a build or run breaks, ask which wrapper owns the platform setup before editing upstream OpenROAD assumptions.

## Common Failure Patterns

- Import or link failure:
  Check top-module naming, missing libraries, and LEF or Liberty path mismatches.
- Massive setup failure before placement:
  Recheck SDC, clocks, IO delays, and unconstrained paths.
- Congestion after floorplan:
  Recheck utilization, macro channels, pin placement, and PDN blockage.
- DRC or antenna failures late in routing:
  Recheck layer usage, antenna repair, and macro spacing.
- Non-reproducible wrapper behavior:
  Extract the narrowest Tcl reproducer and compare artifacts stage by stage.
