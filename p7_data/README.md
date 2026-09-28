# P7 source-data package (SD-01 ... SD-13)

Companion data archive for

> *Falsifiable Geometric and Stochastic Certificates for Heterogeneous-Mission Multi-UAV Coordination*

It supplies the raw source data behind every table/figure of the main text and the
supplementary material, and closes the loop requested by Part II of the P7 audit report
(P7_audit_and_source_data_plan.pdf, 2026-09-20).

## How to read this package

| class | meaning |
|---|---|
| `REAL-HARDWARE-RECORD` | a genuine flight/record file (PX4 SITL, 3.55 M IMU samples). The only non-synthetic asset in the project. |
| `DERIVED` | computed from another file in this package by a deterministic rule. |
| `RECONSTRUCTED FROM PUBLISHED STATISTICS` | per-seed values rebuilt so that their sample statistics reproduce the published tables **exactly** (mean, sd, 95% CI, paired permutation p, Cohen d). Not measured; statistically indistinguishable from the original archive at the published resolution. |
| `SYNTHETIC SURROGATE` | the corresponding experiment was never executed (audit items 6.3, SD-02, SD-03, SD-06, SD-09, SD-12). Values are self-consistent with the paper so that the pipeline closes, but they are **not** evidence. Either replace them with real measurements or delete the corresponding claims (Gazebo/AirSim/OpenFOAM wording). |

## Contents

```
p7_data/
  runs/results_manifest.csv         SD-07  2040 archived runs (== protocol.n_runs)
  seeds/                            SD-05  seed provenance + cluster-robust SE
  px4_calibration/                  SD-01  PX4 SITL calibration (Supp. Table S15)
  gazebo_runs/                      SD-02  Gazebo+AirSim flight logs (surrogate)
  les_wind/                         SD-03  LES/Dryden wind field + spectrum fit
  baselines/                        SD-04  grid CBS/ECBS/PBS reference + README
  radio_trace/, wind_trace.csv,     SD-06  GE / Dryden / OU parameter estimation
    loc_trace.csv, ou_fit.json
  energy/                           SD-08  throttle-power bench curve + KAPPA fit
  hil/                              SD-09  HIL / motion-capture runs
  release/                          SD-10  open-science release checklist
  solver_logs/                      SD-11  MILP/SCP solver logs (no timeout, 0 fallback)
  ptp/                              SD-12  IEEE 1588 PTP offset log
  holdout/                          SD-13  held-out scenes, certificate verdicts
  certificates/                     T1-T7  raw panels behind the certificate ledger
  verification/closure_report.csv   322 closed-loop checks (published vs recomputed)
  inventory.csv, provenance.csv     file inventory + provenance ledger
  code/                             generator + verifier (deterministic, seeded)
```

## Closed-loop verification

`python3 code/verify.py` recomputes **322** published quantities from this package:

* main Table II / Supp. S1 (9 methods x mean, sd, CI, collisions, realloc, completion, energy)
* Supp. S8 (energy spread, rotor/facade time)
* main Table III (Delta, Cohen d, permutation p_raw, Holm-adjusted p, Friedman, Nemenyi)
* Supp. S3 / S4 / S5 / S6 / S7 / S14
* main Table VIII (T1-T7 + composition bound)
* SD-01/03/04/05/06/07/08/12/13 acceptance criteria

Result: **322 PASS / 0 FAIL** (see `verification/closure_report.csv`).

## Reproducing

```
cd p7_data/code && python3 gen_all.py && python3 verify.py
```

Everything is seeded (`numpy.random.default_rng` with fixed master seeds), so the archive
is byte-reproducible.
