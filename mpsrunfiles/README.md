# MPS run files

The linear programme (LP) of each scenario of this model, as the TIMES model
generator builds it, in MPS format. Any LP solver can read these files, with
no GAMS, Veda or TIMES installation.

The files are attached to the release
[`mps-times-v4.9.7`](https://github.com/esma-ie/OTUS9r/releases/tag/mps-times-v4.9.7) rather than kept in git, so that cloning
this repository stays fast. This folder holds only this description.

## Files

| File | What | Size | SHA-256 |
| --- | --- | --- | --- |
| [`OTUS9r+reference+as_run.mps.gz`](https://github.com/esma-ie/OTUS9r/releases/download/mps-times-v4.9.7/OTUS9r%2Breference%2Bas_run.mps.gz) | the LP, free MPS, gzipped | 39.0 MB | `9437bf8e74a766fccada0fd7051d549d6f09ffaecd9f24ab37fdf8d049de5fca` |
| [`OTUS9r+reference+as_run.dict.txt.gz`](https://github.com/esma-ie/OTUS9r/releases/download/mps-times-v4.9.7/OTUS9r%2Breference%2Bas_run.dict.txt.gz) | GAMS names of its rows and columns, gzipped | 14.4 MB | `fee9625af8327aa9a6607d2c825ce745a44ef4755146f48e21f31864b4854620` |
| [`OTUS9r+tax1_wo_mou+as_run.mps.gz`](https://github.com/esma-ie/OTUS9r/releases/download/mps-times-v4.9.7/OTUS9r%2Btax1_wo_mou%2Bas_run.mps.gz) | the LP, free MPS, gzipped | 45.6 MB | `142dc3d42419ec5346bc8f85096ccccfa789fc94f58afc14a0eed7b74f0f5e3e` |
| [`OTUS9r+tax1_wo_mou+as_run.dict.txt.gz`](https://github.com/esma-ie/OTUS9r/releases/download/mps-times-v4.9.7/OTUS9r%2Btax1_wo_mou%2Bas_run.dict.txt.gz) | GAMS names of its rows and columns, gzipped | 16.8 MB | `62ad5e32d81e6a9993488e8a226725c607ef37705d79a43bf68e452943e59804` |

| Scenario | Rows | Columns | Nonzeros | Objective (CPLEX, in GAMS) |
| --- | --- | --- | --- | --- |
| `reference` | 1,028,923 | 1,647,278 | 7,186,412 | 192,206,507,156.18 |
| `tax1_wo_mou` | 1,199,533 | 1,922,664 | 8,402,583 | none (see below) |

Rows and columns as GAMS counted them; the rows include the objective equation `EQ_OBJ`.
- `tax1_wo_mou`: in the reference run CPLEX stopped at a feasible point it did not prove optimal (GAMS model status 7), so no objective is given. The MPS file itself is complete.

## How they were made

- **Scenario, "as run":** the run file `dd/reference/reference.run`, `dd/tax1_wo_mou/tax1_wo_mou.run` of this repository, unchanged:
  every option, REDUCE included, as the model sets it.
- **TIMES:** v4.9.7, [etsap-TIMES/TIMES_model](https://github.com/etsap-TIMES/TIMES_model) @ `d9a6a25`.
- **GAMS:** GAMS 54.6.1, the `CONVERT` tool, which writes the model GAMS generated
  for the SOLVE statement: same rows, columns, coefficients, right-hand sides
  and bounds.
- **By:** the reference harness of [TIMES.jl](https://github.com/esma-ie/TIMESjl)
  (`reference/README.md`), the open Julia/JuMP translation of TIMES by ESMA.
  TIMES.jl's checks compare its own LP of each scenario with GAMS's
  `CONVERT` output of these same runs, entry by entry, at 1e-9.

## Reading them

- The objective is a minimisation (`OBJSENSE MIN`) of the single column
  `OBJz`, so its optimal value is the TIMES objective, in the model's
  currency unit.
- GAMS `CONVERT` gives rows and columns generic names: `e1`, `e2`, ... and
  `x1`, `x2`, ... The `.dict.txt.gz` file beside each LP maps them to the TIMES
  names, for example `e1  EQ_OBJ` or `x1  VAR_ACT(REG1,2005,2005,AOTETOT,ANNUAL)`
  (demo12).
- When TIMES moves to a new version, the files are made again and attached to
  a new release (for example `mps-times-v4.9.8`); earlier releases stay.

## Licence

These files are derived from this model and carry its licence,
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). When you use them:

- credit "OTUS9r, Energy Systems Modelling Analytics Ltd (ESMA)" and the original EPAUS9rT database by the US EPA Office of Research and Development;
- link to the licence and say whether you made changes;
- share your adaptations under the same licence (ShareAlike).
