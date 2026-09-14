# Analysis Notebooks

This directory contains the three final computational notebooks associated with the IAC 2026 study.

Run/read them in the following order:

1. **`IAC_2026_FINAL_Week1b_RealSpaceWeather.ipynb`**  
   Establishes the final physical-drag benchmark using historical F10.7 and Ap space weather with NRLMSISE-00, freezes the corrected TRAIN/VALIDATION/TEST object split, and calibrates the EKF process noise.

2. **`IAC_2026_FINAL_Week2a_RealWeather_GRU_Development.ipynb`**  
   Develops and validates the RTN GRU residual corrector using the TRAIN and VALIDATION spacecraft only. The trained GRU and associated scalers are frozen at the end of this stage.

3. **`IAC_2026_FINAL_Week2b_LOCKED_FINAL_TEST.ipynb`**  
   Performs the final held-out evaluation using the untouched TEST spacecraft and previously unused 2025 historical space-weather conditions.

## Important

The Week 2b notebook represents the locked final test used for the reported IAC 2026 results.

It should not be used to retune the model, alter the architecture, change the process-noise configuration, or select new hyperparameters after observing the final-test results.
