<p align="center">
  <img src="assets/ufs_logo.png" alt="University of the Free State" width="180"/>
</p>

<h1 align="center">Deep Neural Network Emulation and Uncertainty Quantification for Variable-Annuity Guarantee Valuation under Stochastic Mortality</h1>

<p align="center"><em>A calibrated deep surrogate for the present value of variable-annuity guarantee
liabilities — delivering not only a point value but a well-founded predictive interval.</em></p>

PhD research **proposal** in **Mathematical Statistics**, prepared for the **University of the
Free State (UFS)**, 2027 intake. This repository holds the proposal and its supporting material;
the thesis manuscript will live in a separate repository once the topic is registered.

**Author:** Thabang Bongani Junior Baloyi
**Supervisor:** Dr. Jan Blomerous (FASSA)
**Programme:** PhD (Mathematical / Applied Statistics), three years, 2027 intake —
a statistics spine (emulation + uncertainty quantification) with variable-annuity guarantee
valuation under stochastic mortality as the application.

## Overview

Valuing the guarantees embedded in variable annuities (GMAB / GMDB / GMWB) requires **nested
Monte Carlo**: an outer set of real-world scenarios, each carrying an inner set of risk-neutral
valuation paths. Under **stochastic mortality** the state space grows further, and the nested
simulation becomes prohibitively expensive for an insurer that must revalue a large book daily
for pricing, hedging and capital.

This proposal builds a **deep neural network emulator** that maps a contract-and-market-and-
mortality state vector directly to the **present value of the guarantee liability**, replacing the
inner simulation — and, crucially, attaches a **quantified, calibrated uncertainty** to every
prediction. The work is organised in three parts.

1. **Emulation** (deep learning). Train a DNN surrogate
   `f_theta : x -> Y` from state vector `x` (policy, financial-state and mortality features) to
   `Y`, the present value of the guarantee liability, learned against nested-Monte-Carlo targets.
   This extends the neural-network portfolio-valuation line of Hejazi & Jackson and Gan & Lin to a
   genuinely **stochastic-mortality** setting.

2. **Uncertainty quantification** (statistics — the doctoral contribution). Move from a point `Y`
   to a **predictive interval** for `Y`, separating **epistemic** uncertainty (the emulator's own
   error) from **aleatoric** uncertainty (financial and mortality randomness). Candidate machinery:
   deep ensembles, Bayesian last-layer / MC-dropout, and **split-conformal prediction** with
   finite-sample coverage guarantees.

3. **Validation and application** (empirical). Benchmark emulator value and intervals against full
   nested Monte Carlo; study the capital/reserve impact of the quantified uncertainty; optional
   South African case study (StatsSA / ASSA mortality).

## Status

Proposal under construction. Results marked **To be established** are targets with a stated proof
route, not proved theorems. The current working draft is `Proposal/PhD_Proposal_Skeleton.md`
(section-by-section skeleton with a 10–12 page budget); full prose and a LaTeX build will follow.

## Model at a Glance

**Input** — a single state vector `x` with three blocks:

| Block | Features |
|-------|----------|
| Policy characteristics | age; sex (if in the mortality basis); contract duration; initial premium; current account value; guarantee base; time to maturity; surrender-charge schedule; withdrawal rate; guarantee type |
| Financial-state variables | interest rate; equity level / moneyness; equity volatility; stochastic-volatility state; correlation parameters; fee rate |
| Mortality variables | current mortality intensity; mortality trend; mortality volatility; cohort effect; financial–mortality correlation; mortality-model parameters |

**Output** — a single continuous response plus its uncertainty:

```
Y  = present value of the guarantee liability           (point prediction)
Y  ∈ [ L(x), U(x) ]  with coverage ≥ 1 − α              (calibrated predictive interval)
```

## Methods and Tools

- **Deep learning / emulation:** feedforward and sequence (GRU) surrogates; training against
  nested-MC labels; PyTorch, GPU, Optuna hyperparameter search.
- **Uncertainty quantification:** deep ensembles, Bayesian last-layer, MC-dropout,
  **split-conformal prediction** (finite-sample coverage), aleatoric/epistemic decomposition,
  calibration diagnostics (coverage, interval width, CRPS).
- **Stochastic mortality:** affine mortality intensities (Biffis), Lee–Carter / CBD dynamics.
- **Benchmark:** nested Monte Carlo valuation of VA guarantees (Bauer–Kling–Russ framework).
- **Empirical:** Human Mortality Database; a specified VA contract; optional SA case study.

## Repository Structure

```
.
├── README.md
├── assets/
│   └── ufs_logo.png                  # University of the Free State crest
├── Proposal/
│   └── PhD_Proposal_Skeleton.md      # section-by-section skeleton (10-12 pg budget)
├── References/
│   └── References.bib                # BibTeX (natbib author-year)
└── Offer/                            # Admission correspondence (to be added)
```

## Proposal Structure

| Section | Title | Content |
|---------|-------|---------|
| — | Abstract | VA guarantees, nested-MC cost, the DNN emulator, UQ, the headline result |
| 1 | Introduction and Background | Variable annuities, nested simulation, emulation, positioning vs. the MSc |
| 2 | Problem Statement and Research Gap | Emulation exists; *calibrated uncertainty* under stochastic mortality does not |
| 3 | Research Questions and Objectives | RQ1–RQ4 mapped to the contributions |
| 4 | Literature Landscape | VA valuation, NN emulators (Hejazi, Gan–Lin), stochastic mortality, UQ / conformal |
| 5 | Model Specification | Input vector (policy / financial / mortality), output Y, predictive intervals |
| 6 | Methodology | Emulation / UQ / validation tracks |
| 7 | Statistical Contributions | Calibration & coverage guarantees; emulation error; uncertainty decomposition |
| 8 | Computational Plan | Nested-MC label generation, surrogate training, reproducibility |
| 9 | Data | HMD, a VA contract spec, optional SA case study |
| 10 | Significance and Contribution | Theory, method, practice; publication and competition targets |
| 11 | Timeline | Three-year plan mapped to the contributions and three papers |
| 12 | Ethical Considerations | Public/aggregated data only; UFS ethics clearance |
| — | References | Author-year, accuracy emphasised |

## How to Cite

```
Baloyi, T.B.J. (2027). Deep Neural Network Emulation and Uncertainty Quantification for
Variable-Annuity Guarantee Valuation under Stochastic Mortality. PhD research proposal,
University of the Free State.
```

## Keywords

variable annuity, guarantee valuation, nested Monte Carlo, deep neural network emulator,
surrogate model, uncertainty quantification, conformal prediction, calibrated prediction
intervals, stochastic mortality, GMAB, GMDB, GMWB

## Licence

All rights reserved under the intellectual property policy of the University of the Free State.
