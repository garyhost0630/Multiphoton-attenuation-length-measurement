# Multiphoton Attenuation Length Measurement

This repository provides a pipeline for measuring the **effective attenuation length (EAL)** of tissue phantoms. The workflow is designed to be compatible with **Bruker multiphoton systems** and includes both data acquisition guidelines and analysis methods.

---

## Overview

The goal of this project is to estimate the effective attenuation length (EAL) by analyzing fluorescence signals acquired at different imaging depths and excitation powers. The method ensures that only non-saturated two-photon signals are used for accurate estimation.

---
## Experimental Procedure

### 1. Background Acquisition
- Acquire a background image with:
  - Laser **turned off**
  - PMT **operating normally**

### 2. Z-stack Acquisition
- Scan the same volume using a series of different excitation powers.
- Use a **step size of 10 µm** between imaging planes.

---

## Data Processing Pipeline

### 1. Background Subtraction
All images are corrected by subtracting the background image.

---

### 2. Power-Dependence Analysis (Per Depth)

For each imaging depth:
- Extract fluorescence signals under different excitation powers.
- Perform a **linear fit in log-log space** (signal vs. power).
- The expected slope for ideal two-photon excitation is **~2**.

This step is used to:
- Identify the **optimal power range** where the signal is not saturated.
- Exclude powers that deviate from two-photon behavior.

---

### 3. Signal Selection and Normalization

For each depth:
- Select the **maximum or second-highest power** within the valid (non-saturated) power range.
- Extract the corresponding fluorescence signal.
- Normalize the signal across depths.

---

### 4. EAL Estimation

- Perform a **linear fit** between:
  - Imaging depth
  - Log of normalized fluorescence signal

- Obtain the slope `k`

- Compute EAL using: `EAL = -2/K`

---

## Important Notes

### 1. Fluorescence Signal Definition
- The fluorescence signal at each depth is defined as:
  - The **mean intensity of the top 1% brightest pixels**
- This threshold is a **tunable parameter**.

---

### 2. Two-Photon Validation

- The code generates:
  - `depth_vs_slope.png`

This plot shows the fitted slope (signal vs. power) at each depth.

**Best practice:**
- Only include depths where the slope is **close to 2** when calculating EAL.
- This ensures the signal remains within the **true two-photon excitation regime**.

---

## Files in This Repository

- `2P_EAL.ipynb` — Main analysis notebook  
- `depth_analysis_topPercent.xlsx` — Intermediate processed data  
- `depth_vs_slope.png` — Power-law validation across depths  
- `EAL.png` — Final EAL fitting result  
- `ZSeries-*` — Raw imaging datasets
- `lookuptable.xlsx` — EOM vs Power

---

## Summary

This pipeline provides:
- A robust method to avoid **two-photon saturation artifacts**
- A reproducible way to compute **effective attenuation length**
- Compatibility with **Bruker imaging systems**
