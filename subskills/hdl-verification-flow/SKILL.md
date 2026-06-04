---
name: hdl-verification-flow
description: Plan, debug, and explain HDL linting, simulation, and verification with Verilator and cocotb. Use when Codex needs to work from Verilog, SystemVerilog, VHDL, Makefiles, pytest files, cocotb tests, waveform settings, or simulator logs for lint-only checks, smoke simulations, Python-driven testbenches, waveform or coverage setup, or choosing between pure Verilator and cocotb-based flows.
---

# HDL Verification Flow

## Overview

Use this skill to combine fast Verilator feedback with cocotb-driven Python testing. Start by deciding whether the task is lint-only, a fast smoke simulation, or a reusable verification harness.

Prefer narrow reproductions: one toplevel, one source list, one failing test, and explicit waveform settings. Escalate to another simulator only when the required language feature or mixed-language setup is beyond Verilator's practical support.

## Workflow

1. Determine the verification goal.
   Separate linting, smoke simulation, and full Python testbench work.
2. Start with Verilator when speed matters.
   Use it for linting and quick compile-based simulations before adding cocotb layers.
3. Add cocotb when stimulus and checking logic belong in Python.
   Choose the Makefile flow for small projects and the Python Runner or pytest flow for programmable regression control.
4. Enable waveforms early.
   Make wave dumping explicit so every failure has an inspectable trace.
5. Keep simulator assumptions visible.
   Record whether the run uses Verilator, Icarus, Questa, or another simulator instead of assuming the default.
6. Stop broad debugging.
   Reduce failures to the smallest runnable example, then grow back to the full environment.

## What To Read

- Read [references/verilator-cocotb-playbook.md](references/verilator-cocotb-playbook.md) for common commands, waveforms, coverage, and runner choices.
- Read [references/example-map.md](references/example-map.md) for local upstream example paths worth copying from.

## Default Habits

- Run a lint pass before writing elaborate testbench code.
- Make the toplevel, source list, simulator choice, and waveform settings explicit.
- Use cocotb on top of Verilator when the design fits the supported feature set and Python stimulus is valuable.
- Switch to a different simulator only when the required HDL feature or mixed-language behavior justifies it.

## Guardrails

- Do not present Verilator as a full replacement for every event-based, mixed-signal, or SDF-heavy workflow.
- Do not leave simulator choice implicit in bug reports or reproduction steps.
- Do not debug a large regression first when one focused failing test can reproduce the issue.
