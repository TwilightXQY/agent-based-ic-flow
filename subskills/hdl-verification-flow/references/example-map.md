# Example Map

## Verilator Examples

Clone root:

`upstream-sources/verilator`

Recommended starting points:

- `upstream-sources/verilator/examples/make_hello_binary`
  Use for the smallest `--binary` example.
- `upstream-sources/verilator/examples/make_hello_c`
  Use when a tiny C++ harness is clearer than `--binary`.
- `upstream-sources/verilator/examples/make_tracing_c`
  Use when you need tracing and waveform-oriented setup.

## cocotb Examples

Clone root:

`upstream-sources/cocotb`

Recommended starting points:

- `upstream-sources/cocotb/examples/doc_examples/quickstart`
  Use for the smallest quickstart with both Makefile and Runner flows.
- `upstream-sources/cocotb/examples/simple_dff`
  Use for a tiny single-register example that works well with pytest-style flows.
- `upstream-sources/cocotb/examples/adder`
  Use when you want a simple scoreboard or model comparison shape.
- `upstream-sources/cocotb/examples/mixed_language`
  Use when the design spans Verilog and VHDL.
- `upstream-sources/cocotb/examples/mixed_signal`
  Use only when the simulator and project actually support the mixed-signal setup.

## Search Tips

Find all example test entrypoints:

```bash
rg 'cocotb.test|get_runner|--binary|--trace' upstream-sources/verilator upstream-sources/cocotb/examples
```
