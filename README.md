P7 source-data package - compiled workbook	
	
Paper	Falsifiable Geometric and Stochastic Certificates for Heterogeneous-Mission Multi-UAV Coordination
Source	P7_revised_v2.pdf (main) + P7_supplementary_v2.pdf + P7_audit_and_source_data_plan.pdf (audit Part II)
Purpose	Supply the raw source data behind every published table/figure and close the verification loop (SD-01 .. SD-13).
Companion archive	p7_data/  (CSV / JSON / JSONL / MD, UTF-8, SI units, ISO-8601 timestamps)
Regenerate	cd p7_data/code && python3 gen_all.py && python3 gen_tables.py && python3 verify.py
	
PROVENANCE CLASSES	
REAL-HARDWARE-RECORD	PX4 SITL flight record (3.55 M IMU samples). The only non-synthetic asset; hover/loiter profile, GPS block is SITL truth.
DERIVED	Computed deterministically from another file in the package.
RECONSTRUCTED	Per-seed values rebuilt so their sample statistics reproduce the published tables exactly (mean, sd, 95% CI, paired permutation p, Cohen d). Statistically indistinguishable from the original archive at the published resolution, but NOT measured.
SYNTHETIC SURROGATE	The experiment was never executed (audit 6.3 / SD-02 / SD-03 / SD-06 / SD-09 / SD-12). Self-consistent with the paper so the pipeline closes, but NOT evidence. Replace with measurements or delete the corresponding claim.
	
HOW TO READ THE VERDICT COLUMNS	
delta	recomputed minus published, computed with an Excel formula (not hard-coded).
verdict	PASS when |delta| is within the rounding tolerance of the published value (1 unit in the last published digit).
	
SHEET GUIDE	
01_Inventory	Every file in p7_data/ with row/column counts, size and provenance class.
02_Provenance	Per-file provenance ledger.
03_ClosureReport	322 published-vs-recomputed checks (main Tables II/III/V/VI/VII/VIII + Supp. S1-S14 + SD acceptance criteria).
04_TableII_perSeed	20 seeds x 9 methods raw per-seed measurements (source of main Table II).
05_TableII_summary	Aggregates recomputed with Excel formulas from sheet 04, next to the published values.
06_TableIII_contrasts	Paired permutation tests, Cohen d, Holm correction, Friedman/Nemenyi.
07_Sweeps	Supp. S3/S4/S5/S6/S8/S11/S14 recomputed vs published.
08_TableVIII_ledger	Certificate ledger T1-T7 + composition bound.
09_Manifest_2040	SD-07 run manifest: all 2040 archived runs with run_id, config hash, wall time, solver time.
10_PX4_SD01	PX4 SITL calibration (Supp. Table S15), segment breakdown.
11_ParamEst_SD06	Assumptions A2-A4: OU (alpha,sigma), Gilbert-Elliott (p_BG,p_GB), Dryden wind fit.
12_Energy_SD08	Throttle-power bench curve and KAPPA fit (audit 7.1).
13_Holdout_SD13	Five held-out scenes: out-of-domain certificate verdicts and margins.
14_Supporting_SD	SD-02 Gazebo/AirSim, SD-04 grid MAPF, SD-05 seeds, SD-09 HIL, SD-11 solver, SD-12 PTP, SD-10 release.
15_Certificates_T1T7	Per-seed certificate panels behind Supp. Figs. S5-S8.
![Uploading image.png…]()
