# Numerical Results

This directory contains numerical data products produced during calibration, validation, and the locked final evaluation.

## Week 1b — Physics and EKF Calibration

- `real_MSIS_density_summary.csv`
- `real_weather_multiobject_divergence.csv`
- `real_weather_outage_stress_summary.csv`
- `real_weather_Q_sweep_TRAIN.csv`
- `real_weather_Q_sweep_VALIDATION.csv`

These files document the real-space-weather benchmark and EKF process-noise calibration stage.

## Week 2a — GRU Validation

- `week2a_validation_results.csv`
- `week2a_validation_by_weather.csv`
- `week2a_validation_by_schedule.csv`
- `week2a_validation_by_shell.csv`

These results were produced using TRAIN and VALIDATION objects only and were used during model development.

## Week 2b — Locked Final Test

- `FINAL_scenario_results.csv`
- `FINAL_object_weather_cluster_results.csv`
- `FINAL_object_level_results.csv`
- `FINAL_leave_one_object_out.csv`
- `FINAL_by_weather.csv`
- `FINAL_by_schedule.csv`
- `FINAL_by_shell.csv`
- `FINAL_tail_error_summary.csv`
- `FINAL_RESULTS_SUMMARY.json`

These files correspond to the frozen held-out evaluation using six untouched TEST spacecraft, six previously unused 2025 historical space-weather days, and three observation regimes, producing 108 final scenarios.

`FINAL_RESULTS_SUMMARY.json` provides a compact machine-readable record of the locked configuration and headline results.

The Week 2b results should not be used for retrospective model tuning.
