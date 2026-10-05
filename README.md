# Reflected BSDEs and Optimal Stopping for Variable-Annuity Surrender Guarantees

Super-Replication under Stochastic Mortality in Incomplete Markets: Well-Posedness,
an Optional-Decomposition Duality, and a Convergence-Guaranteed Deep Solver.

PhD research **proposal** in **Mathematical Statistics**, prepared for the University of the
Free State (UFS) 2027 intake. This repository holds the proposal and its supporting material;
the thesis manuscript will live in a separate repository once the topic is registered.

**Author:** Thabang Bongani Junior Baloyi
**Supervisor:** Dr. Jan Blomerous (FASSA)
**Programme:** PhD (Mathematical / Applied Statistics), three years, 2027 intake —
stochastic-analysis spine first, deep-learning solver as the computational layer, a calibrated
variable-annuity book as the empirical coda.

## Overview

A variable annuity lets the policyholder **surrender (lapse) at any time** before maturity and
collect a guaranteed benefit. That right is an **American-style option embedded in a life
contract**: the holder exercises at the stopping time that maximises value, so the liability is
a **Snell envelope**, characterised by a **reflected backward stochastic differential equation
(RBSDE)**. Two features move this beyond the classical theory and beyond the candidate's MSc on
super-hedging of American options:

1. the policyholder may **die before surrendering**, giving a random horizon driven by a
   **stochastic mortality intensity**; and
2. mortality risk is **not traded**, so the market is **incomplete** and the guarantee can only
   be **super-replicated**.

The proposal treats this as a problem of well-posedness, duality and finite-sample computation,
in three parts.

1. **Well-posedness** (stochastic analysis). Formulate and prove existence and uniqueness of the
   solution of the RBSDE for a VA surrender guarantee under a stochastic mortality intensity in an
   incomplete market (random-horizon, doubly-stochastic structure).

2. **Duality** (mathematical finance). Establish a **super-hedging / optional-decomposition**
   theorem for the mortality-driven supermartingale — the doctoral contribution, generalising the
   optional decomposition studied in the candidate's MSc from a purely financial market to one
   carrying unhedgeable mortality risk.

3. **Computation and validation** (deep learning + empirical). Design a **deep-BSDE / deep
   optimal-stopping** scheme for the solution, prove convergence / error bounds, and validate on
   Human Mortality Database intensities and a concrete VA contract, with a South African case
   study as an optional coda.

## Status

Proposal under construction. Results marked **To be established** are targets with a stated proof
route, not proved theorems. The current working draft is `Proposal/PhD_Proposal_Skeleton.md`
(section-by-section skeleton with a 10–12 page budget); full prose and a LaTeX build will follow.

## Methods and Tools

- **Stochastic analysis:** reflected BSDEs, Snell envelopes, optimal stopping, optional
  decomposition of supermartingales, enlargement of filtration, affine stochastic-mortality
  intensities.
- **Deep learning:** deep-BSDE solvers (Han–Jentzen–E), deep optimal stopping
  (Becker–Cheridito–Jentzen), deep hedging (Buehler et al.); PyTorch, GPU, Optuna.
- **Empirical:** Human Mortality Database calibration; a specified VA contract (fee, guarantee
  level, surrender schedule); South African mortality (StatsSA / ASSA) as an optional case study.
- **Typesetting:** LaTeX (to follow), natbib author-year per the UFS guideline.

## Repository Structure

```
.
├── README.md
├── Proposal/                         # The proposal itself
│   └── PhD_Proposal_Skeleton.md      #   section-by-section skeleton (10-12 pg budget)
├── References/                       # Bibliography and reading
│   └── References.bib                #   BibTeX (natbib author-year)
└── Offer/                            # Admission correspondence (to be added)
```

## Proposal Structure

| Section | Title | Content |
|---------|-------|---------|
| — | Abstract | The product, the RBSDE, the two complications, the method, the headline result |
| 1 | Introduction and Background | Variable annuities, the surrender option, positioning vs. the MSc |
| 2 | Problem Statement and Research Gap | RBSDEs under stochastic mortality + incompleteness are under-developed |
| 3 | Research Questions and Objectives | RQ1–RQ4 mapped to the theorems |
| 4 | Literature Landscape | RBSDEs, optional decomposition, stochastic mortality, VAs, deep BSDE |
| 5 | Mathematical Formulation | Snell envelope, the reflected BSDE, the mortality-intensity setup |
| 6 | Methodology | Theory / numerical / validation tracks |
| 7 | Theoretical Contributions | T1 well-posedness, T2 duality, T3 convergence, (T4 robust extension) |
| 8 | Computational Plan | Deep solver, reproducibility, open-source deliverable |
| 9 | Data | HMD, a VA contract spec, optional SA case study |
| 10 | Significance and Contribution | Theory, method, practice; publication and competition targets |
| 11 | Timeline | Three-year plan mapped to the theorems and three papers |
| 12 | Ethical Considerations | Public/aggregated data only; UFS ethics clearance |
| — | References | Author-year, accuracy emphasised |

## How to Cite

```
Baloyi, T.B.J. (2027). Reflected BSDEs and Optimal Stopping for Variable-Annuity Surrender
Guarantees: Super-Replication under Stochastic Mortality in Incomplete Markets. PhD research
proposal, University of the Free State.
```

## Keywords

reflected BSDE, optimal stopping, Snell envelope, optional decomposition, super-hedging,
variable annuity, surrender option, stochastic mortality, incomplete markets, deep BSDE,
deep optimal stopping

## Licence

All rights reserved under the intellectual property policy of the awarding institution.
