# Reproducibility Manifests

This directory contains machine-readable JSON records describing frozen experiment settings from the major stages of the IAC 2026 analysis.

## Files

- `week1_reproducibility_manifest.json` — configuration metadata from the initial benchmark stage.
- `week1b_reproducibility_manifest.json` — configuration metadata for the real-space-weather and physical-drag benchmark.
- `week2a_reproducibility_manifest.json` — frozen GRU-development configuration used before the locked final test.

The manifests preserve important configuration details so that the final experiment can be interpreted independently of the original Google Colab runtime.
