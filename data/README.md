# Data

This directory contains the frozen input data and experiment metadata used across the IAC 2026 analysis.

## Main Files

- `starlink_30_object_cohort.csv` — original frozen 30-object Starlink cohort.
- `starlink_30_object_cohort_WEEK1B.csv` — corrected cohort used for the final TRAIN/VALIDATION/TEST split.
- `starlink_supgp_snapshot.json` — frozen CelesTrak Supplemental GP / OMM snapshot used for orbital initialization.
- `starlink_supgp_snapshot_metadata.json` — metadata associated with the frozen orbital snapshot.
- `real_space_weather_daily_activity_scan.csv` — historical daily space-weather activity scan used during Week 1b.
- `week2a_weather_split.csv` — frozen TRAIN/VALIDATION weather split used for GRU development.
- `week2a_scenario_metadata.csv` — scenario metadata generated during Week 2a.
- `FINAL_2025_weather_days.csv` — six previously unused 2025 historical space-weather days selected for the locked final test.
- `week2a_skipped_truth_cases.csv` — record of any truth cases skipped during Week 2a.
- `FINAL_skipped_truth_cases.csv` — record of any truth cases skipped during the locked Week 2b evaluation.

The two `skipped_truth_cases` files may be empty when no scenarios were rejected. Their presence is retained as part of the frozen experiment record.
