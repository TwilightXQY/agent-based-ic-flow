# Toolchain Notes

## Clone Roots

- `upstream-sources/xschem`
- `upstream-sources/ngspice`
- `upstream-sources/magic`

## xschem

Primary upstream files:

- `upstream-sources/xschem/README`
- `upstream-sources/xschem/README_MacOS.md`
- `upstream-sources/xschem/INSTALL`

Key takeaways:

- Use xschem for hierarchical schematic capture and netlisting.
- Generate Spice, Verilog, or VHDL netlists from the same schematic source.
- Expect XQuartz and Tcl/Tk details to matter on macOS.
- Use the bundled libraries and examples under `xschem_library` when you need symbol or example structure guidance.

macOS build notes from upstream:

```bash
./configure --prefix=/Users/$(whoami)/xschem-macos
make
make install
```

Upstream also documents a more manual XQuartz plus Tcl/Tk path for Big Sur and similar systems.

## ngspice

Primary upstream files:

- `upstream-sources/ngspice/README`
- `upstream-sources/ngspice/INSTALL`
- `upstream-sources/ngspice/tests/README`

Key takeaways:

- Use ngspice for mixed-level and mixed-signal simulation.
- Prefer a separate build directory.
- For a git checkout, upstream expects `./autogen.sh` before `configure`.
- Useful configure flags from upstream include `--with-x`, `--with-readline=yes`, `--disable-debug`, `--enable-xspice`, and `--enable-openmp`.
- Use the upstream `tests/` tree as a pattern library for small reproducible decks.

Typical git-based build flow from upstream:

```bash
./autogen.sh
mkdir release
cd release
../configure --with-x --with-readline=yes --disable-debug
make
sudo make install
```

## Magic

Primary upstream files:

- `upstream-sources/magic/README.md`
- `upstream-sources/magic/INSTALL_MacOS.md`
- `upstream-sources/magic/README.Tcl`

Key takeaways:

- Use Magic for layout editing, DRC, extraction, and technology-file-driven work.
- Expect hierarchical SPICE output, extraction, and LEF/DEF support to be part of the normal workflow.
- On macOS, upstream documents both a Homebrew path and a manual XQuartz plus Tcl/Tk path.
- Tcl-wrapped Magic allows scripted window targeting and automation when layout debugging gets repetitive.

Homebrew-oriented macOS path from upstream:

```bash
brew install cairo tcl-tk@8 python3 gnu-sed
brew install --cask xquartz
./scripts/configure_mac
make database/database.h
make -j$(sysctl -n hw.ncpu)
make install
```

## Search Tips

Use targeted searches instead of reading the whole clones:

```bash
rg 'configure|make install|xquartz|tcl|tk|extract|netlist|spice' upstream-sources/xschem upstream-sources/ngspice upstream-sources/magic
```
