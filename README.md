# IAC 2026 — Hybrid AI-Augmented Orbit Tracking

Code, frozen experiment configuration, trained-model artifacts, numerical results, and reproducibility materials associated with:

**Hybrid AI-Augmented Framework for High-Fidelity Tracking of Non-Cooperative Objects**

**77th International Astronautical Congress (IAC 2026)**  
Antalya, Türkiye, 5–9 October 2026  
**Paper ID:** IAC-26,A6,9,8,x109530

---

## Overview

This repository contains the final computational workflow used to evaluate a physics-guided hybrid orbit-tracking framework combining a classical Extended Kalman Filter (EKF) with a compact Gated Recurrent Unit (GRU) residual corrector.

The central question of the study is whether a small recurrent model can improve a properly tuned physics-based estimator when the estimator is exposed to structured atmospheric-drag mismatch and sparse observations.

The EKF remains the primary state estimator and orbit propagator. The GRU does not replace the orbital dynamics, modify the EKF covariance, or alter future EKF propagation. Instead, it uses recent estimator and environmental history to predict the remaining three-component position residual in the local radial-transverse-normal (RTN) frame.

---

## Experimental Framework

The final benchmark combines:

- public Starlink orbital initial conditions from a frozen CelesTrak Supplemental GP / OMM snapshot;
- historically observed F10.7 and Ap space-weather forcing;
- NRLMSISE-00 atmospheric density;
- a numerical reference trajectory containing two-body gravity, J2, and atmospheric drag;
- a lower-fidelity Cartesian EKF containing two-body gravity and J2 but no explicit atmospheric-drag force;
- simulated noisy Cartesian position measurements;
- a GRU trained to estimate the remaining EKF position residual in RTN coordinates.

The experiment uses a controlled historical environmental replay. The Starlink orbital geometries and historical space-weather dates are not contemporaneous and therefore should not be interpreted as reconstructions of the exact spacecraft trajectories on those dates.

The reference trajectories and tracking observations are simulated. This study should therefore be interpreted as a physically informed LEO proxy benchmark rather than direct operational radar or debris-tracking validation.

---

## Final Hybrid Architecture

| Component | Configuration |
|---|---|
| Classical estimator | Cartesian Extended Kalman Filter |
| EKF dynamics | Two-body gravity + J2 |
| Reference dynamics | Two-body gravity + J2 + NRLMSISE-00-driven atmospheric drag |
| GRU target | 3-component RTN position residual |
| GRU hidden units | 16 |
| GRU layers | 1 |
| Historical context | 60 minutes |
| Sampling interval | 5 minutes |
| Sequence length | 12 samples |
| Dropout | 0.20 |
| Measurement model | Simulated Cartesian position |
| Observation regimes | 10 min, 20 min, and 10 min with a 4-hour blackout |

A Random Forest adaptive process-noise model was evaluated during model development but was not retained in the final Hybrid architecture.

---

## Repository Structure

```text
IAC2026-Hybrid-Orbit-Tracking/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── IAC_2026_FINAL_Week1b_RealSpaceWeather.ipynb
│   ├── IAC_2026_FINAL_Week2a_RealWeather_GRU_Development.ipynb
│   └── IAC_2026_FINAL_Week2b_LOCKED_FINAL_TEST.ipynb
│
├── data/
│   └── Frozen orbital, cohort, weather, and scenario inputs
│
├── models/
│   ├── week2a_RTN_GRU.pt
│   ├── week2a_feature_scaler.joblib
│   └── week2a_target_scaler.joblib
│
├── manifests/
│   └── Frozen reproducibility manifests
│
├── results/
│   └── Calibration, validation, and locked final-test numerical outputs
│
└── figures/
    └── Calibration, validation, and final-test figures
```

---

## Notebook Workflow

### 1. Week 1b — Physical Drag and Real Space Weather

`notebooks/IAC_2026_FINAL_Week1b_RealSpaceWeather.ipynb`

This notebook establishes the final physics benchmark by replacing the earlier synthetic drag-like disturbance with:

**historical F10.7 + Ap → NRLMSISE-00 density → physical atmospheric drag**

It also freezes the corrected 18/6/6 TRAIN/VALIDATION/TEST object split and calibrates the EKF process-noise setting using training and validation objects only.

No GRU training or final-test evaluation is performed at this stage.

### 2. Week 2a — GRU Development and Validation

`notebooks/IAC_2026_FINAL_Week2a_RealWeather_GRU_Development.ipynb`

This notebook develops and freezes the RTN residual-correction GRU.

Model development uses:

- 18 TRAIN spacecraft;
- 6 VALIDATION spacecraft;
- training space weather from 2021–2023;
- validation space weather from 2024;
- three predefined observation regimes.

The final six TEST spacecraft and 2025 final-test weather conditions remain untouched during model development.

The trained GRU, feature scaler, target scaler, and reproducibility manifest are saved for the locked evaluation.

### 3. Week 2b — Locked Final Test

`notebooks/IAC_2026_FINAL_Week2b_LOCKED_FINAL_TEST.ipynb`

This notebook performs the frozen held-out evaluation using:

- 6 previously untouched TEST spacecraft;
- 6 previously unused 2025 historical space-weather days;
- 3 fixed observation regimes.

This produces **108 final scenarios**.

The model architecture, feature definitions, scalers, process-noise configuration, object assignments, observation schedules, and trained network weights are frozen before this evaluation.

**The final-test notebook should not be used for model tuning or architecture selection after observing the test results.**

---

## Locked Final-Test Results

Across all 108 held-out scenarios:

| Method | Mean 3D Position RMSE |
|---|---:|
| SGP4 open-loop baseline | 84.145 km |
| Tuned Cartesian EKF | 1.971 km |
| Hybrid EKF + RTN GRU | **0.559 km** |

The Hybrid reduced aggregate mean RMSE relative to the tuned EKF by **71.63%**.

The descriptive scenario-level Hybrid win rate was **74.1%**.

### Sparse-Observation Performance

For the observation regime containing a four-hour measurement blackout:

| Metric | Tuned EKF | Hybrid | Reduction |
|---|---:|---:|---:|
| Full 24-h trajectory containing blackout | 3.216 km | 0.861 km | 73.22% |
| Actual 10–14 h outage interval only | 7.159 km | 1.514 km | **78.85%** |

The outage-only metric quantifies estimator behavior specifically while measurements are unavailable.

---

## Reproducibility Files

The `manifests/` directory contains frozen machine-readable records of the experiment configuration.

The `models/` directory contains the retained GRU weights and the corresponding feature and target scalers used in the locked evaluation.

The `results/` directory includes calibration outputs, validation outputs, scenario-level locked-test results, object-level results, weather and observation-regime summaries, shell summaries, tail-error metrics, cluster-level analysis, and leave-one-object-out robustness results.

`results/FINAL_RESULTS_SUMMARY.json` contains a compact machine-readable record of the locked final-test configuration and headline results.

---

## Installation

The notebooks were developed for Python and Google Colab.

To install the principal dependencies in another Python environment:

```bash
pip install -r requirements.txt
```

The principal packages are:

- NumPy
- pandas
- SciPy
- Matplotlib
- scikit-learn
- SGP4
- PyMSIS
- Astropy
- joblib
- PyTorch

---

## Important Interpretation Boundaries

The following limitations should be considered when interpreting the benchmark:

- historical space weather is replayed on frozen Starlink orbital geometries rather than reconstructing contemporaneous historical trajectories;
- the drag coefficient and area-to-mass ratio are representative benchmark assumptions rather than measured object-specific aerodynamic properties;
- tracking measurements are simulated noisy Cartesian positions rather than raw radar range/azimuth/elevation or optical observations;
- the reference model does not contain object-specific attitude dynamics, time-varying ballistic coefficient, maneuvers, atmospheric winds, solar-radiation pressure, or mass evolution;
- the final evaluation uses held-out Starlink orbital geometries as a controlled LEO proxy and does not constitute direct Iridium/Cosmos debris validation.

---

## Data Provenance

Orbital initial conditions are derived from a frozen public CelesTrak Supplemental GP / OMM Starlink snapshot archived with the experiment.

Historical F10.7 and Ap forcing is obtained through the PyMSIS/CelesTrak historical space-weather interface and used with NRLMSISE-00 to calculate atmospheric mass density.

The archived snapshot and reproducibility manifests are included so that the experiment does not depend on a future re-download of a changing orbital catalog.

---

## Citation

If you use this repository, please cite the associated IAC 2026 paper:

> *Hybrid AI-Augmented Framework for High-Fidelity Tracking of Non-Cooperative Objects*  
> 77th International Astronautical Congress (IAC 2026), Antalya, Türkiye, 5–9 October 2026.  
> Paper ID: IAC-26,A6,9,8,x109530.

A machine-readable `CITATION.cff` file will be added with the final publication metadata.

---

## Status

This repository represents the frozen experiment associated with the IAC 2026 manuscript.

The repository is being finalized prior to public release. The locked final-test artifacts should remain unchanged apart from documentation, packaging, and reproducibility improvements that do not alter the reported experiment.

## License

Original code in this repository is released under the MIT License. Third-party data and source materials remain subject to their respective provider terms.
