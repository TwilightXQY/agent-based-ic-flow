# Agent-Based IC Flow

`agent-based-ic-flow` is a Codex skill pack for IC and EDA work. It provides one
top-level router skill, `ic-eda-flow`, and a set of bundled specialist guides for
analog IC design, SPICE simulation, Verilog-A, transistor sizing, SAR ADC blocks,
HDL verification, and OpenROAD physical design.

The intent is to keep the active skill surface small while still preserving deep
domain procedures. The top-level skill decides which specialist material to load
for the current task.

## What It Covers

- Analog/custom IC flow with xschem, ngspice, Magic, layout extraction, and
  pre/post-layout comparison
- SPICE simulation setup, reusable ngspice examples, and PTM model selection
- Verilog-A authoring, EVAS behavioral simulation, and OpenVAF + ngspice compact
  model compilation
- gm/ID characterization and transistor sizing workflows
- SAR ADC system design and key sub-blocks: CDAC, comparator, bootstrap switch,
  LDO, and SAR logic
- StrongArm comparator, bootstrapped sampling switch, and PMOS-pass LDO
  simulation/sizing flows
- HDL linting and verification with Verilator and cocotb
- Digital ASIC physical-design triage with OpenROAD

## Repository Layout

```text
.
├── SKILL.md                       # Top-level ic-eda-flow router skill
├── agents/openai.yaml             # Codex UI metadata
├── references/routing-map.md      # Task-to-subskill routing table
└── subskills/
    ├── analog-ic-flow/
    ├── ngspice/
    ├── transistor-models/
    ├── veriloga/
    ├── openvaf/
    ├── evas-sim/
    ├── gmoverid/
    ├── sar-adc-skill/
    ├── comparator/
    ├── bootstrap_switch/
    ├── LDO/
    ├── hdl-verification-flow/
    └── openroad-physical-design/
```

## Installation

Install it as a Codex skill by cloning the repository into your global skills
directory:

```bash
git clone https://github.com/TwilightXQY/agent-based-ic-flow.git \
  ~/.codex/skills/ic-eda-flow
```

Restart or open a new Codex session so the skill metadata is reloaded.

If you already have a large set of individual IC/EDA skills installed, prefer
keeping only this top-level `ic-eda-flow` skill active and placing older
fine-grained skills outside the active skill directory. The bundled `subskills/`
directory gives the router the detailed material it needs without exposing every
specialist guide as a separate top-level skill.

## Usage

Ask naturally. The router should trigger for IC/EDA work such as:

```text
Simulate this StrongArm comparator and extract decision time.
```

```text
Write a Verilog-A model for a 4-bit SAR logic block.
```

```text
Debug this OpenROAD global routing congestion report.
```

```text
Set up a gm/ID characterization sweep for this MOSFET model.
```

When `ic-eda-flow` triggers, it first classifies the task, gathers the local
artifacts that control the result, and then loads the relevant specialist guide
from `subskills/`.

## Routing Model

The routing table is in [`references/routing-map.md`](references/routing-map.md).
Important rules:

- Use `veriloga` first when authoring a new `.va` model.
- Use `evas-sim` for event-driven behavioral Verilog-A simulation.
- Use `openvaf` for compact/device Verilog-A models compiled into ngspice OSDI
  plugins.
- Use `ngspice`, `gmoverid`, `comparator`, `bootstrap_switch`, `LDO`, or
  `sar-adc-skill` for circuit-level analog workflows.
- Use `hdl-verification-flow` for RTL lint/simulation and `openroad-physical-design`
  for digital backend work.

## Private PDK And Project Safety

Treat local PDK, model, rule, and signoff files as the source of truth. The skill
pack is intentionally conservative:

- Do not invent device names, BSIM parameters, layer stacks, extraction rules,
  timing corners, or signoff constraints.
- Do not copy private PDK data into public repositories.
- Prefer existing project scripts, decks, manifests, logs, reports, and waveform
  outputs before writing new flows.
- Copy reusable examples into the working project before editing them; do not
  modify the bundled skill assets unless you are intentionally updating this
  skill pack.

## Dependencies

Different subskills use different external tools. Common dependencies include:

- `ngspice`
- Python 3 with packages such as `numpy`, `matplotlib`, and `scipy`
- Verilator and cocotb for HDL verification flows
- OpenROAD for physical-design workflows
- OpenVAF for Verilog-A compact model compilation
- EVAS for supported event-driven Verilog-A simulations

The top-level router does not install tools automatically. It should identify
missing executables or Python packages and suggest the smallest useful setup for
the current task.

## Notes

This repository is a skill pack, not a single EDA tool. It helps an agent choose
and execute the right workflow, but simulation and implementation results still
depend on the local project files, installed tools, and PDK/model data available
in the user environment.
