# Test the shift register

[test.py](test.py) uses cocotb, a Python simulation tool, to drive the inputs and
check the outputs. It checks reset, shift direction, holding a value, dropping
the oldest bit, repeated bits and unused pin directions.
The module name and source filenames are already set for this example.

## Check in GitHub Actions

1. Open **Actions → test**. Open the latest run for your commit and check that
   the `test` job passed. This is RTL simulation: it tests the Verilog behaviour.
2. Open **Actions → gds**. To start a fresh run, choose **Run workflow → main →
   Run workflow**. Wait for the `gds`, `precheck`, `gl_test` and `viewer` jobs.
3. Open `gl_test` and expand **GL test**. Check the test summary for zero failures.
   GLS means gate-level simulation: it tests the generated logic cells using
   the same Python checks. The workflow supplies the netlist and cell models.
4. Return to the run summary. Under **Artifacts**, download `gatelevel_test_results`
   for GLS, or `test-results` from the RTL run. Unzip the download to inspect
   `results.xml` and the `tb.fst` waveform.

The [verified GLS job](https://github.com/mdkamrulislam-web/tt-shift-register/actions/runs/38906540398/job/116992507980)
at commit `7a9eabb6` reports **TESTS=1 PASS=1 FAIL=0**.
This is functional GLS; use the timing reports separately to check timing.
The `viewer` job publishes the layout webpage and is separate from `gl_test`.

## Run RTL simulation locally

Use a Linux or WSL terminal with Python, Icarus Verilog and GNU Make installed.
From the repository root:

```sh
python -m pip install -r test/requirements.txt
cd test
make -B
python -m cocotb_tools.check_results results.xml
```

The final command checks the result file for test failures. To inspect the
waveform with GTKWave installed, run `gtkwave tb.fst tb.gtkw` from `test/`.

For a first attempt at GLS, use the GitHub workflow above. The local command
`make -B GATES=yes` also needs the generated `test/gate_level_netlist.v`, the
Sky130 cell models and a correctly configured `PDK_ROOT`.
