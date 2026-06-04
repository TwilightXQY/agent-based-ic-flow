# Verilator + cocotb Playbook

## Choose The Flow

Use this quick split:

- Lint only:
  Start with Verilator `--lint-only`.
- Fast compiled smoke simulation:
  Use Verilator `--binary` or a small C++ harness.
- Python-driven checking:
  Use cocotb with either a Makefile flow or the Python Runner.
- Unsupported HDL features or specialized simulator behavior:
  Keep the cocotb layer if useful, but move to a simulator that supports the feature set.

## Minimal Verilator Commands

Lint:

```bash
verilator --lint-only -Wall top.sv
```

Fast standalone build and run:

```bash
verilator --binary -j 0 top.v
./obj_dir/Vtop
```

Waveforms with FST:

```bash
verilator --binary --trace --trace-fst --trace-structs top.v
```

## Minimal cocotb Makefile Shape

The upstream quickstart Makefile uses these key variables:

- `SIM`
- `TOPLEVEL_LANG`
- `VERILOG_SOURCES` or `VHDL_SOURCES`
- `COCOTB_TOPLEVEL`
- `COCOTB_TEST_MODULES`
- `WAVES`

Typical invocation:

```bash
make SIM=verilator WAVES=1 EXTRA_ARGS="--trace --trace-fst --trace-structs"
```

Use `SIM=icarus` or another supported simulator when Verilator is not the right backend.

## Python Runner / pytest Shape

The Runner flow has three steps:

1. Get the runner: `get_runner(sim)`
2. Build HDL: `runner.build(...)`
3. Run tests: `runner.test(...)`

Use pytest when you want regression orchestration, fixtures, or parametrization. Use direct Python execution for the smallest reproducer.

## Waveforms And Coverage

From the upstream cocotb simulator-support docs:

- For Verilator waveform files, use `--trace` plus `--trace-fst` or `--trace-structs`.
- For cocotb, set `WAVES=1` or pass waveform-related build args through the Runner.
- For GUI viewing from Runner flows, set `GUI=1`.
- For Verilator coverage, add `--coverage`.

## Common Failure Patterns

- Lint fails immediately:
  Fix syntax, width, or unsupported construct issues before building a testbench.
- cocotb cannot find the DUT:
  Recheck toplevel naming and source list.
- No waveform file appears:
  Recheck `WAVES`, `EXTRA_ARGS`, and whether the selected simulator supports the requested format.
- Python test imports fail:
  Recheck `COCOTB_TEST_MODULES`, pytest discovery, and local Python environment.
- Simulator mismatch:
  Recheck `SIM` and document it explicitly in commands and summaries.

## Upstream Anchors

- Verilator README: `upstream-sources/verilator/README.rst`
- cocotb quickstart: `upstream-sources/cocotb/docs/source/quickstart.rst`
- cocotb runner docs: `upstream-sources/cocotb/docs/source/runner.rst`
- cocotb simulator support: `upstream-sources/cocotb/docs/source/simulator_support.rst`
