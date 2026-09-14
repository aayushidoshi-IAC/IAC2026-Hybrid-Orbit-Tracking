# Frozen Model Artifacts

This directory contains the trained machine-learning artifacts retained after Week 2a development and used unchanged during the locked Week 2b evaluation.

## Files

- `week2a_RTN_GRU.pt` — trained PyTorch GRU state dictionary.
- `week2a_feature_scaler.joblib` — fitted scaler used to normalize GRU input features.
- `week2a_target_scaler.joblib` — fitted scaler used for the RTN residual targets.

These artifacts were frozen before the final TEST spacecraft and 2025 test weather conditions were evaluated.

The GRU provides a post-filter RTN position-residual correction. It does not replace the Extended Kalman Filter, alter the EKF covariance, or modify subsequent EKF propagation.
