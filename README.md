# Evaluation data for "Spreadsheet Fault Localisation under a Fallible Oracle: Probabilistic Diagnosis Informed by the Workbook"

This package contains the per-file evaluation results underlying every quantitative claim in the paper, the ground-truth/observation annotations the evaluation consumed, and the sampling frames and seeds needed to reproduce the samples. It does not contain the original workbooks (see "Corpora" below) or the solver code.



>Due to multiple typesetting and different LaTeX templates, some page numbers and table numbers may differ from those in the officially published paper. Thank you for your understanding.

## Contents

- `data/rq1/` — RQ1 per-file results (full corpora, faithful oracle, eta = 0): one row per (file, method). These files also carry the four RQ3 factorial configurations at eta = 0 (methods FM+PB, PB, FM, PQDM-v3.2), i.e. paper Tables 12–14 and 26–28.
- `data/rq2/` — RQ2 per-file results (observation noise): one row per (file, condition, repetition). Columns Arm (sym / biasC / coneD / cov-key / cov-uniform), Knowledge (oracle / misspec / est / -), Eta, Coverage, Policy and Seed record the full condition of each run. Paper Tables 7, 16–18 and 22–24.
- `data/rq3/` — RQ3 within-component increments at eta = 0 (FM_Excluded_Class, PB_Relativize, PB_Mask_Literals record the varied design choice). `rq3_EUSES_full.csv` is the full-corpus  re-run whose values the paper reports; paper Table 32.
- `data/rq4/` — RQ4 parameter study. Stage A: single-factor sweeps at eta = 0; Stage A2: alpha/gamma re-sweeps at true eta = 0.3; Stage B: the assumed-by-true noise-rate surface (True_Eta, Assumed_Eta); Stage C: prior odds-cap conditions on INFO1 (the rows with empty Param are the uncapped reference). `rq4_summary.csv` is the aggregated summary (means, paired Wilcoxon, Holm-adjusted p) from which paper Tables 9, 10 and 33 are drawn. EUSES rows use the 200-file frame in `sampling/euses_200.txt`.
- `data/rq5/` — RQ5 timing batch (single-worker re-runs): per-file stage timings (T_Load_ms … T_EndToEnd_ms) for all nine methods. The EXAM columns are the parity by-product of this batch (see "Batch notes"). Paper Tables 11 and 34.
- `gt_observations/<CORPUS>/` — one JSON per workbook: `ground_truth (list of faulty cell ids) and `observations` (list of {cell_id, label} with label in {TOO_LOW, CORRECT, TOO_HIGH}), as taken from the corpora's annotation files.
- `sampling/` — the sampling frames (file lists) below.

## Method labels

The CSVs use the runner's method labels. Mapping to the paper: FM+PB = IPQDM; PB = IPQDM without the fault model (pattern prior only); FM = IPQDM without the pattern prior (fault model only); PQDM-v3.2 = the uninformed configuration; QDM-v3 = QDM; DStar, Ochiai, Tarantula, ConeVote as named in the paper. In `data/rq3/`, labels are IPQDM-<component change>, e.g. IPQDM-FM_no_op_swap, IPQDM-PB_no_relativize.

## Metrics

One row is one (file, method, condition) run. Exam_Expected is the EXAM score of that run (expected fraction of the candidate ranking inspected before the first true fault, ties averaged); Top1_Exp … Top10_Exp, Recall@5_Exp, Recall@10_Exp and MRR_Exp are the corresponding ranking metrics; Fault_Score is the score assigned to the (first) ground-truth cell; Hit, Rank_Best / Rank_Expected / Rank_Worst describe the best fault's position. FM_Partial flags files whose fault-model build hit the 1,200 s budget and fell back to the uniform fault branch for the remaining cells. Aggregation, as in the paper: repetitions are averaged within each file first; corpus means and all tests are over files.

## Sampling frames and seeds

- RQ2 EUSES sample (160 files): family-stratified, seed 20261029, restricted to Space <= 300 — `sampling/rq2_euses_160.txt`.
- RQ2 / RQ5 INFO1 sample (40 files): size-stratified — `sampling/info1_40.txt` (the same frame in both research questions).
- EUSES 200-file frame (RQ3 increments subset, all RQ4 EUSES runs, RQ5 EUSES sample): the first 200 EUSES files in filename order — `sampling/euses_200.txt`.
- Observation-noise realisations use deterministic hash-derived seeds per (file, condition, repetition); the seed of every RQ2 run is recorded in its row's Seed column.

## Batch notes (read before comparing across folders)

The evaluation was run in several batches; three documented differences matter when recomputing paper numbers:

1. The RQ3 EUSES increments batch (data/rq3) has an IPQDM reference mean of 0.0733, against 0.0734 in the RQ1 batch (data/rq1): per-file EXAM is identical on 883 of the 892 INTEGER+EUSES files and within 0.013 on the remaining nine (fault-model builds truncated by the deadline under different machine load). The paper's Table 32 note states this; each table must be reproduced from its own batch's files.
2. The RQ5 timing batch is a separate single-worker re-run. Its EXAM columns agree bit-for-bit with the RQ1/RQ2 results on 443 of 461 files; the 18 exceptions are deadline-bound fault-model builds.
3. The RQ4 Stage C reference (uncapped, INFO1) mean is 0.2177, a different batch from RQ1's 0.2241; the paper reports both numbers in their respective sections.

## Corpora

INTEGER, EUSES and INFO1 are third-party corpora and are not redistributed here; obtain the workbooks from their original sources. This package releases our per-file results and the annotation extracts in `gt_observations/` (fault locations and observation labels keyed by workbook file name).
