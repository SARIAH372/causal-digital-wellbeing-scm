# causal-digital-wellbeing-scm

## Causal Modeling of Digital Well-Being and Mental-Health Burden

---

## Introduction

This repository contains a **reproducible, research-grade causal inference framework** for studying modern drivers of mental-health burden using **synthetic but scientifically realistic data**. Unlike symptom-scoring or purely predictive machine-learning systems, this project focuses on **causal estimands, identifiability, robustness, and uncertainty**, with explicit assumptions and diagnostics.

The framework models contemporary exposures—academic pressure, sleep debt, screen exposure, doomscrolling, social support, and physical activity—using a **Structural Causal Model (SCM)**. Because the data-generating process is known, the pipeline enables **ground-truth benchmarking** of causal estimators under realistic noise, confounding, and distribution shift.

This project is intended for **methodological research, causal ML benchmarking, and education**, not for clinical or policy deployment.

---

## Research Question

**Which modern lifestyle and environmental factors causally increase mental-health burden, and how reliable are those conclusions under confounding, uncertainty, and distribution shift?**

---

## Data Generation: Structural Causal Model (SCM)

All data are generated inside the pipeline via an explicit **Structural Causal Model**.

### Key Properties
- No PHQ-9, GAD-7, or PCL-5 scales
- Bounded and heaped (ordinal-like) variables
- Heteroskedastic noise
- Nonlinear effects and interactions
- Explicit mediators (rumination, sleep variability)
- Missingness mechanisms linked to stress and SES

### Core Variables
- Socioeconomic context: `ses`, `urbanicity`
- Baseline state: `baseline_stress`, `baseline_health`
- Exposures:
  - academic pressure
  - sleep debt
  - screen time
  - doomscrolling
  - social support
  - physical activity
- Outcome:
  - continuous **burnout / distress index**

Because the SCM is known, **true counterfactual outcomes and true ATEs** are available for evaluation.

---

## Methods

### Causal Estimand

For each binary treatment \(T\), the estimand of interest is the **Average Treatment Effect (ATE)**:

\[
\tau = \mathbb{E}[Y(1) - Y(0)]
\]

---

### Treatments Analyzed

- `T_high_pressure`
- `T_sleep_debt`
- `T_doomscroll`
- `T_high_screen`
- `T_low_support`
- `T_low_activity`

Each treatment is assigned via a **confounded mechanism** reflecting realistic selection bias.

---

### Causal Estimators

Each treatment is evaluated using **three complementary estimators**:

#### 1. G-formula (Outcome Regression)
\[
\hat{\tau}_{G} = \mathbb{E}[\hat{m}_1(X) - \hat{m}_0(X)]
\]

#### 2. Inverse Probability Weighting (IPW)
\[
\hat{\tau}_{IPW} =
\mathbb{E}\left[
\frac{T Y}{\hat{e}(X)} -
\frac{(1-T) Y}{1-\hat{e}(X)}
\right]
\]

#### 3. Doubly Robust AIPW
\[
\hat{\tau}_{AIPW} =
\mathbb{E}\left[
\hat{m}_1(X) - \hat{m}_0(X)
+ \frac{T}{\hat{e}(X)} (Y - \hat{m}_1(X))
- \frac{1-T}{1-\hat{e}(X)} (Y - \hat{m}_0(X))
\right]
\]

AIPW remains consistent if **either** the propensity model or the outcome model is correctly specified.

---

### Identifiability & Positivity Diagnostics

Before interpreting causal effects, the pipeline evaluates:

- Propensity score estimation for each treatment
- **Overlap / positivity plots** comparing treated vs control groups
- Identification assessed prior to effect interpretation

---

### Sensitivity Analysis (Unmeasured Confounding)

For each treatment, the pipeline computes **sensitivity curves**:

- AIPW estimates are adjusted under increasing hypothetical confounding strength \(b\) (in SD units)
- Curves show how strong unmeasured confounding would need to be to attenuate or reverse the estimated effect

---

### Distribution Shift Robustness

Causal effects are re-estimated under simulated shifts:

- SES downshift
- screen-time surge
- increased missingness

This evaluates **transport stability**, not just in-sample accuracy.

---

### Uncertainty Quantification

Prediction uncertainty is quantified using **split conformal inference**:

- Distribution-free **90% prediction intervals**
- Empirical coverage evaluated
- No parametric assumptions required

---

## Results

Each execution produces a **complete causal analysis record**.

### Causal Effect Estimates
- Stored in `effects.xlsx`
- Contains:
  - True ATE (from SCM)
  - G-formula estimates
  - IPW estimates
  - AIPW estimates
- Visualized in:
  - `effects_bar.png` (True ATE vs AIPW ATE)

### Overlap / Positivity
- Propensity overlap plots for each treatment:
  - `overlap_T_sleep_debt.png`
  - `overlap_T_high_pressure.png`
  - `overlap_T_high_screen.png`
  - `overlap_T_low_activity.png`
  - `overlap_T_low_support.png`
  - `overlap_T_doomscroll.png`
- Summary statistics stored in `overlap.json`

### Sensitivity to Unmeasured Confounding
- Sensitivity curves generated for each treatment:
  - `sensitivity_T_sleep_debt.png`
  - `sensitivity_T_high_pressure.png`
  - `sensitivity_T_high_screen.png`
  - `sensitivity_T_low_activity.png`
  - `sensitivity_T_low_support.png`
  - `sensitivity_T_doomscroll.png`
- Numeric results stored in `sensitivity.json`

### Distribution Shift Stability
- Shifted ATEs stored in `shift_stability.xlsx`
- Visualized in:
  - `shift_stability.png`

### Uncertainty Results
- Conformal interval statistics stored in `conformal.json`
- Visualization in:
  - `conformal_coverage.png`
- Confirms near-target empirical coverage without distributional assumptions
outputs_project2/run_YYYYMMDD_HHMMSS/
tables/
effects.xlsx
overlap.json
sensitivity.json
conformal.json
shift_stability.xlsx
run_metadata.json
figures/
effects_bar.png
overlap_T_sleep_debt.png
overlap_T_high_pressure.png
overlap_T_high_screen.png
overlap_T_low_activity.png
overlap_T_low_support.png
overlap_T_doomscroll.png
sensitivity_T_sleep_debt.png
sensitivity_T_high_pressure.png
sensitivity_T_high_screen.png
sensitivity_T_low_activity.png
sensitivity_T_low_support.png
sensitivity_T_doomscroll.png
shift_stability.png
conformal_coverage.png
MODEL_CARD.md
DATASET_CARD.md

---

## Ethics & Scope

- No real individuals or patient data
- No diagnosis or clinical claims
- Synthetic data used to validate **methods**, not prevalence
- Explicit assumptions and limitations documented

This project is intended for **methodological research, benchmarking, and education**, not deployment.

---

## Usage

### Installation
pip install numpy pandas scikit-learn matplotlib

### Run
python project2_run.py

Each execution generates a complete causal analysis bundle under `outputs_project2/`.

---

## Intended Audience

- Causal inference and ML researchers
- Computational social science researchers
- Digital-wellbeing researchers
- Ethics-focused ML practitioners
- Students learning modern causal ML pipelines

---

## License & Disclaimer

For **research and educational use only**.  
Not intended for clinical, policy, or individual-level decision-making.

---

## Output Structure

