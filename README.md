# 8-bit shift register — Tiny Tapeout worked example

This completed EE6019 example shows how an existing Verilog shift register fits
into Tiny Tapeout. Start with the [design explanation](docs/info.md), then follow
the [test instructions](test/README.md).

To start a separate project, use the [official Verilog template](https://github.com/TinyTapeout/ttsky-verilog-template).
Use this repository to compare your adapted files and results.

[Download the seven-file starter ZIP](https://raw.githubusercontent.com/mdkamrulislam-web/tt-shift-register/main/docs/shift-register-starter.zip)
and extract it before following the lesson. Keep the template's other files:
add `src/shift_register.v` and replace only the other six files in the ZIP.

## How it works

On each rising clock edge, a low `rst_n` clears all eight bits. Otherwise, when
shift enable is high, the new serial bit enters `q[0]` and the stored bits move
towards `q[7]`. When shift enable is low, the value stays the same.
Reset is synchronous: it takes effect on a rising clock edge.

| Tiny Tapeout port | Use in this design |
|---|---|
| `clk` | Clock; declared target is 50 MHz, or 20 ns per cycle |
| `rst_n` | Reset when 0; the wrapper converts it for the core |
| `ui_in[0]` | Serial input bit |
| `ui_in[1]` | Shift enable: 1 shifts, 0 holds |
| `uo_out[7:0]` | Stored value `q[7:0]` |
| `ena` | Tiny Tapeout project-selection signal |
| `ui_in[7:2]`, `uio_in[7:0]` | Unused inputs |
| `uio_out[7:0]`, `uio_oe[7:0]` | Both 0; bidirectional pins remain inputs |

## Where to look

- [src/shift_register.v](src/shift_register.v): the original design.
- [src/project.v](src/project.v): the wrapper connecting it to Tiny Tapeout's ports.
- [info.yaml](info.yaml): project details, pin descriptions and source filenames.
- [test/](test/): the testbench and Python checks.
- [docs/info.md](docs/info.md): the project's datasheet text.

The module name `tt_um_student_shift_register` and both source filenames are
already configured. Keep all other template files and folders, including
`.github/`, `src/config.json` and `test/requirements.txt`.
Before a future submission, choose a unique top-module name and update it
consistently in `info.yaml`, `src/project.v` and `test/tb.v`.

## Verified results

The following runs passed for commit
[`7a9eabb6`](https://github.com/mdkamrulislam-web/tt-shift-register/tree/7a9eabb6ab7d228519b93c70375c13b43ce666be):

| Check | Evidence |
|---|---|
| RTL simulation: test the Verilog behaviour | [Passing test run](https://github.com/mdkamrulislam-web/tt-shift-register/actions/runs/38906373262) |
| Datasheet generation | [Passing docs run](https://github.com/mdkamrulislam-web/tt-shift-register/actions/runs/38902985069) |
| GDS layout build, precheck, functional GLS and viewer | [Passing gds run](https://github.com/mdkamrulislam-web/tt-shift-register/actions/runs/38906540398) |

GLS means gate-level simulation: the same behaviour checks run against the
generated logic cells. This is functional GLS; inspect the timing reports
separately to check the clock target. Passing these checks does not mean the
design has been fabricated.

[Open the published layout viewer](https://mdkamrulislam-web.github.io/tt-shift-register/).
For a new run, open **Actions → gds → Run workflow → main → Run workflow**.
See [test/README.md](test/README.md) for the results and waveform downloads.
