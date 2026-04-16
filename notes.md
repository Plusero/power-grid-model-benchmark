# What I did to find the bugs
1. `uv venv .venv && uv pip install --python .venv/bin/python -r requirements.txt`
2. Ran the notebook `Power Grid Model Benchmark.ipynb`.

# Bugs and fixes

## Bug 1: Mismatch between to_ppc and makeYbus

 **The bug:**
- `makeYbus` uses fixed branch-column indices from `pandapower.pypower.idx_brch` (for example `BR_R_ASYM`, `BR_X_ASYM`, `BR_G`, `BR_B`) inside `branch_vectors`.
- The `ppci["branch"]` matrix from `to_ppc(pp_net, init="flat")` had fewer columns than the highest required `BR_*` index.
- When `makeYbus` accessed these missing columns, numpy raised `IndexError` (`index 22` and then `index 23` out of bounds), so `LightSim2GridNetInput.from_pandapower_net` failed at `Ybus, _, _ = makeYbus(baseMVA, bus, branch)`.
- Both `makeYbus` and `to_ppc` are from PandaPower.

 **The fix:**
- In `generate_fictional_dataset.py`, the fix computes the required branch width dynamically as `max(BR_*) + 1` from `idx_brch`.
- If `branch.shape[1]` is smaller then `makeYbus` requires, fills the missing columns with zeros. This is a neutral default for those terms for asymmetric calculations and prevents out-of-bounds indexing.

**PandaPower version:** `3.3.3`.


## Bug 2: Missing columns in pandapower grid

 **The bug:**
- `grid2op` with `PandaPowerBackend` failed during initial `pp.runpp(...)` with `KeyError: 'const_z_p_percent'`.
- In this pandapower version, `_init_runpp_options` checks `net["load"]` for four ZIP-model columns: `const_z_p_percent`, `const_i_p_percent`, `const_z_q_percent`, and `const_i_q_percent`. These `const_*` columns are used to define the parts of load that behave as a constant impedance load or a constant current load.
- The generated grid (`g2o_grid_sym/grid.json`) was built from `pp_net_sym.load` that only had legacy columns (`const_z_percent`, `const_i_percent`) and did not include all required columns.

 **The fix:**
- Added `LOAD_VOLTAGE_DEPENDENT_COLUMN_DEFAULTS` in `generate_fictional_dataset.py` with legacy and split ZIP column names, all defaulting to `0.0`. This 0 defaults do not affect the benchmark results because these 0 defaults sets the load to constant power, which is a common setting for Power Flow calculations.
- Added `_ensure_load_voltage_dependent_columns(load_df)` to guarantee those columns exist in generated load DataFrames.

**PandaPower version:** `3.3.3`.
**Grid2Op version:** `1.12.3`.
## Bug 3: Duplicate line names in Grid2Op grid.json

 **The bug:**
- Grid2Op environment creation failed with EnvError: Two lines have the same names because `pp_net_sym.line["name"]` serialized as blank/duplicate names in `grid.json`.

 **The fix:**
- In `generate_fictional_dataset.py`, assign deterministic line names (`line_<index>`) before exporting grid.json and enforce uniqueness with `assert pp_net_sym.line["name"].is_unique`.

**PandaPower version:** `3.3.3`.
**Grid2Op version:** `1.12.3`.