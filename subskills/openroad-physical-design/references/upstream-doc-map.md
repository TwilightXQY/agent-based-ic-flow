# Upstream Doc Map

## Clone Root

The upstream clone for this skill lives at:

`upstream-sources/OpenROAD`

## Read These Files First

| File | Use When |
| --- | --- |
| `upstream-sources/OpenROAD/README.md` | You need the high-level flow stages, supported usage modes, and PDK-independence claims. |
| `upstream-sources/OpenROAD/docs/user/Build.md` | You need build, prebuilt, Docker, or local-install guidance. |
| `upstream-sources/OpenROAD/src/README.md` | You need OpenROAD Tcl database I/O commands such as `read_lef`, `read_def`, `read_verilog`, `write_db`, or `write_abstract_lef`. |

## Stage-Specific Module Docs

| File | Focus |
| --- | --- |
| `upstream-sources/OpenROAD/src/ifp/README.md` | Floorplan initialization |
| `upstream-sources/OpenROAD/src/pdn/README.md` | Power-distribution network generation |
| `upstream-sources/OpenROAD/src/gpl/README.md` | Global placement |
| `upstream-sources/OpenROAD/src/dpl/README.md` | Detailed placement |
| `upstream-sources/OpenROAD/src/rsz/README.md` | Resizing and timing repair |
| `upstream-sources/OpenROAD/src/cts/README.md` | Clock tree synthesis |
| `upstream-sources/OpenROAD/src/grt/README.md` | Global routing |
| `upstream-sources/OpenROAD/src/drt/README.md` | Detailed routing |
| `upstream-sources/OpenROAD/src/ant/README.md` | Antenna checking and repair |
| `upstream-sources/OpenROAD/src/rcx/README.md` | RC extraction |
| `upstream-sources/OpenROAD/src/psm/README.md` | Power or IR-drop style analysis |
| `upstream-sources/OpenROAD/src/fin/README.md` | Finishing steps |

## Search Tips

Use fast searches instead of opening many files blindly:

```bash
rg '^#|^##|```tcl|^read_|^write_|^report_' upstream-sources/OpenROAD/src/<module>/README.md
```

If you only know the failing command name, search the whole clone:

```bash
rg '<command_name>' upstream-sources/OpenROAD/src
```
