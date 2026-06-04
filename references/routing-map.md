# IC/EDA Skill Routing Map

Use this map after `ic-eda-flow` triggers. Read only the matching bundled
specialist guide under `subskills/`.

## General Toolchain

- `subskills/analog-ic-flow/SKILL.md`: xschem, ngspice, Magic, custom IC schematic/layout,
  extraction, pre/post-layout comparison, macOS/XQuartz issues, private PDK flow
  bring-up.
- `subskills/ngspice/SKILL.md`: ngspice simulation templates, PTM sample netlists, common
  transient/DC/AC/noise examples, wrdata parsing, installation notes.
- `subskills/transistor-models/SKILL.md`: PTM model library selection and copying model
  files into projects.

## Verilog-A And Behavioral Simulation

- `subskills/veriloga/SKILL.md`: writing or fixing new Verilog-A behavioral modules.
  Use this before simulation-specific guides when authoring `.va` files.
- `subskills/openvaf/SKILL.md`: compiling Verilog-A compact device models with OpenVAF and
  loading `.osdi` models in ngspice.
- `subskills/evas-sim/SKILL.md`: running EVAS for voltage-mode/event-driven Verilog-A or
  Spectre-style behavioral simulations, and checking EVAS compatibility.

## Analog Design Methodology

- `subskills/gmoverid/SKILL.md`: gm/ID characterization, plotting transistor design
  curves, building lookup tables, and sizing MOS devices.
- `subskills/sar-adc-skill/SKILL.md`: SAR ADC architecture, CDAC, sampling, comparator
  budgeting, ENOB/SNDR, Verilog-A models, and phased verification.

## Circuit Blocks

- `subskills/comparator/SKILL.md`: StrongArm dynamic comparator waveform/noise/offset/speed
  simulation and sizing trade-offs.
- `subskills/bootstrap_switch/SKILL.md`: SAR ADC bootstrapped sampling switch operation,
  Ron flatness, charge injection, feedthrough, and sizing.
- `subskills/LDO/SKILL.md`: PTM 180 nm PMOS-pass LDO DC/AC/noise/transient simulation,
  loop gain, PSRR, load steps, and sizing.

## Digital IC / Backend EDA

- `subskills/hdl-verification-flow/SKILL.md`: Verilog/SystemVerilog/VHDL linting,
  Verilator, cocotb, pytest flows, waveforms, and regression setup.
- `subskills/openroad-physical-design/SKILL.md`: OpenROAD floorplan, placement, CTS,
  routing, extraction, congestion, timing, and private-PDK backend triage.

## Conflict Rules

- If the task is "write a `.va` model", start with `veriloga`, then simulate
  with `evas-sim`, `openvaf`, or `ngspice` depending on model type.
- If the task is "run an existing `.va` model", inspect compatibility first:
  use `evas-sim` for event-driven behavioral voltage models, `openvaf` for
  compact/device models, and `ngspice` for circuit-level SPICE decks.
- If the task involves both analog and digital artifacts, separate the boundary:
  verify HDL with `hdl-verification-flow`, then simulate analog/mixed-signal
  behavior with the relevant SPICE or Verilog-A guide.
- If the task touches private PDK data, use local files as ground truth and avoid
  filling in missing proprietary values from memory.
