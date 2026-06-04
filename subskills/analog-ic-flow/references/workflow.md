# Analog Workflow

## 1. Gather Inputs

Prefer this minimal set:

- xschem schematics and symbol libraries
- Model includes or `.lib` files
- Corner or process selection names
- Supply values and stimulus assumptions
- Magic technology file and layout files if layout exists
- Extracted netlists if post-layout comparison is needed

## 2. Stabilize The Schematic

Check these before running a long simulation:

- Pin order and naming
- Supply and ground conventions
- Parameter defaults
- Include order for model files
- Which netlist backend the flow expects

If the task is on a private node such as TSMC28, trust the local model and tech files over public examples.

## 3. Build A Minimal Simulation

Start with one small deck and one analysis:

- `.op` for bias sanity
- `.dc` for transfer curves
- `.ac` for small-signal behavior
- `.tran` for time-domain behavior

Only after the minimal deck behaves correctly, add:

- Sweeps
- Corners
- Monte Carlo or statistical variation
- Extracted parasitics
- Mixed-signal co-simulation glue

## 4. Compare Pre- And Post-Layout

When layout exists:

1. Run DRC in Magic with the correct technology.
2. Extract the netlist.
3. Re-run the same stimulus on both schematic and extracted views.
4. Attribute differences to parasitics, pin mismatches, missing includes, or extraction setup before changing the circuit.

## 5. Common Failure Patterns

- Convergence failure:
  Simplify the testbench, verify supplies, initial conditions, and unrealistic ideal sources.
- Missing model:
  Recheck `.include` or `.lib` paths and corner selection names.
- Wrong gain or bias:
  Recheck pin order, parameter overrides, and whether the intended device model was actually loaded.
- Post-layout mismatch:
  Recheck extraction setup, subcircuit naming, parasitic loading, and pin mapping.
- Tool does not start on macOS:
  Recheck XQuartz, Tcl/Tk, and display setup before debugging the design itself.

## 6. Private-PDK Habits

- Ask for the names and paths of the local tech and model files, not their contents, unless the task requires direct editing.
- Avoid rephrasing proprietary rule values into the output.
- Keep fixes centered on flow shape: include order, subcircuit naming, environment variables, launch commands, and analysis setup.
