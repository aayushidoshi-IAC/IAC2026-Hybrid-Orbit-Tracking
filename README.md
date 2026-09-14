# IAC2026-Hybrid-Orbit-Tracking
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

The implemented framework retains orbital dynamics and recursive measurement assimilation inside the EKF. The GRU does not replace the orbit propagator or modify the EKF covariance. Instead, it receives recent estimator and environmental history and predicts only the remaining three-component position residual in the local radial-transverse-normal (RTN) frame.

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

The reference trajectories and tracking observations are simulated. This study is therefore a physically informed LEO proxy benchmark rather than direct operational radar or debris-tracking validation.

---

## Final Hybrid Architecture

The final model uses:

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
