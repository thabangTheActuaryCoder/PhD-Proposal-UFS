# Deep Neural Network Emulation and Uncertainty Quantification for Variable-Annuity Guarantee Valuation under Stochastic Mortality

**PhD Research Proposal**

**Candidate:** Thabang Bongani Junior Baloyi
**Supervisor:** Dr. Jan Blomerous (FASSA)
**Degree:** PhD, Mathematical / Applied Statistics, University of the Free State
**Proposed commencement:** 2027 · **Date:** _____________

---

## Abstract

Variable annuities are long-dated savings contracts whose embedded guarantees — minimum
accumulation, death, and withdrawal benefits — expose the insurer to a joint financial and
mortality liability that has no closed-form value. Market practice evaluates these guarantees by
nested Monte Carlo simulation, which becomes prohibitively expensive once the mortality basis is
itself stochastic and the insurer must revalue a large book repeatedly for pricing, hedging, and
regulatory capital. This research proposes a deep neural network *emulator* that maps a single
contract-, market-, and mortality-state vector directly to the present value of the guarantee
liability, replacing the inner valuation and reducing a day's revaluation from hours to seconds.
The distinctive contribution is not the point surrogate but its *uncertainty quantification*:
rather than a single number, the emulator returns a calibrated predictive interval, with the
error decomposed into an epistemic component (the surrogate's own approximation error) and an
aleatoric component (the irreducible noise of the simulated labels and mortality randomness).
Finite-sample coverage of the intervals is to be established through split-conformal prediction,
whose exchangeability requirement is satisfiable by construction because training states are
sampled independently. The method is benchmarked against full nested Monte Carlo and against the
2026 deep least-squares Monte Carlo literature, and validated on Human Mortality Database
calibrations with an optional South African case study. The outcome is a fast, uncertainty-aware
valuation tool together with the statistical theory that certifies its predictions.

---

## 1. Introduction and Background

A variable annuity (VA) combines investment in a reference fund with a set of insurance
guarantees. The policyholder pays a premium that is invested on their behalf; in return, the
insurer promises a floor on the benefit paid at maturity (the guaranteed minimum accumulation or
maturity benefit), on death before maturity (the guaranteed minimum death benefit), or on a
stream of withdrawals (the guaranteed minimum withdrawal benefit). These guarantees are funded by
fees deducted continuously from the account value, and they transfer to the insurer a liability
that is simultaneously exposed to equity risk, interest-rate risk, and mortality risk. Because the
guarantees are options on the policyholder's fund, their fair value cannot be read from a formula;
it must be computed as a discounted expectation under an appropriate measure.

The industry standard for this computation is nested Monte Carlo simulation. An outer layer of
real-world scenarios describes how the market and the policyholder's account might evolve, and,
at each outer node, an inner layer of risk-neutral paths values the remaining guarantee. The cost
is the product of the two layers, and it grows sharply when the valuation state is
high-dimensional. Introducing a *stochastic* mortality model — so that the force of mortality is
itself a diffusion or a longer-memory process rather than a fixed life table — enlarges the state
further and couples the actuarial and financial sides of the problem. For an insurer that must
revalue a portfolio of hundreds of thousands of heterogeneous contracts every day, and stress it
under many scenarios for solvency capital, direct nested simulation is simply too slow.

Emulation, or metamodelling, is the established remedy. Instead of re-solving the valuation from
scratch for every contract and every state, one learns the valuation *function* once, from a
designed set of expensively computed examples, and then evaluates that learned function cheaply
wherever it is needed. Neural networks are a natural choice of emulator because they approximate
high-dimensional, nonlinear maps without hand-crafted basis functions, and because a single
trained network can value an entire portfolio in a single forward pass. The seminal work of
Hejazi and Jackson (2016) and the metamodelling programme of Gan and Lin (2015, 2018) established
that this approach can value large VA portfolios to actuarial accuracy at a fraction of the cost
of nested simulation.

What those emulators return, however, is a single number. For pricing that may suffice; for risk
and capital it does not. A valuation that drives a reserve or a hedge needs to be accompanied by a
statement of how much it can be trusted — a quantified uncertainty that distinguishes the error
introduced by the surrogate itself from the irreducible randomness of the underlying financial and
mortality world. Supplying that quantified, *calibrated* uncertainty, under a stochastic-mortality
model, is the purpose of this research.

The proposal also builds naturally on the candidate's prior work. The MSc dissertation developed
deep-learning methods for super-hedging path-dependent options in incomplete markets, combining
feedforward and recurrent architectures with path-signature features and interpreting the outputs
through Monte Carlo and Shapley analysis. The present project reuses that craft — network design,
simulation-based training, feature construction, GPU experimentation — but redirects it from
hedging toward *emulation and uncertainty quantification*, and from purely financial options
toward insurance guarantees carrying mortality risk.

## 2. Problem Statement and Research Gap

The relevant literature has advanced along two tracks. The first is cross-sectional emulation:
Hejazi and Jackson (2016, 2017) and Gan and Lin (2015, 2018) build fast surrogates that value many
contracts as a function of their characteristics. These deliver point values, typically assume a
static mortality basis, and provide no uncertainty quantification. The second track is recent deep
least-squares Monte Carlo (LSMC) solving. Li and Lyu (2026) develop a deep *signature* LSMC scheme
to value a VA with an early-surrender option under a rough-Heston equity model and a Volterra
(long-range-dependent) stochastic-mortality model, and supply a convergence proof that decomposes
the pricing error into Monte Carlo, time-discretisation, signature-truncation, and
neural-network-approximation parts. Langrené, Luo, Shevchenko and Zhang (2026) extend deep LSMC to
guaranteed minimum withdrawal benefits under optimal dynamic withdrawal, compare polynomial with
neural-network regression, and — importantly for this proposal — show how to obtain confidence
intervals for the contract value.

These two 2026 papers are the closest prior art, and the research gap must be stated carefully
against them. Both are *solvers*: they price a single contract by regressing continuation values
backward through time, and the uncertainty they quantify is the Monte Carlo *estimator* error of
that one price. Neither constructs a *cross-sectional emulator* — a function of the full
policy, financial, and mortality state that can value arbitrary contracts and states instantly —
and neither attaches to such an emulator a *predictive* uncertainty with a coverage guarantee.
The gap this thesis addresses is therefore precise: there is no VA-guarantee emulator that (i)
ingests a stochastic-mortality state as a first-class input, (ii) returns a distribution-free,
finite-sample coverage-guaranteed predictive interval for the guarantee value, and (iii)
decomposes that interval into epistemic (emulator) and aleatoric (market-and-mortality)
components. Rather than competing with the LSMC solvers, this work uses them — in particular Li and
Lyu's calibrated stochastic-mortality dynamics — as the nested-Monte-Carlo ground truth that
generates the emulator's training labels and as the benchmark against which its accuracy is judged.

In one sentence: this thesis builds a calibrated, uncertainty-aware deep emulator for
variable-annuity guarantee valuation under stochastic mortality, and establishes the statistical
guarantees that make its predictive intervals trustworthy.

## 3. Research Questions and Objectives

The research is organised around four questions.

**RQ1.** Can a deep neural network emulate the present value of a variable-annuity guarantee
liability, as a function of the full policy, financial-state, and mortality state vector, to
actuarial accuracy when measured against nested-Monte-Carlo targets?

**RQ2.** How should the emulator's predictive uncertainty be represented and decomposed into an
epistemic component, attributable to the surrogate, and an aleatoric component, attributable to the
finite-sample labels and the financial and mortality randomness?

**RQ3.** Can the predictive intervals be endowed with a finite-sample coverage guarantee — for
example through split-conformal prediction — and under what conditions does the required
exchangeability hold given the way the labels are generated?

**RQ4.** What is the practical effect of the quantified uncertainty on reserves and solvency
capital, demonstrated on realistic, data-calibrated contracts?

The corresponding objectives are: to design and train the emulator and document its accuracy
against nested simulation (addressing RQ1); to develop and compare uncertainty-quantification
schemes — deep ensembles, Bayesian last-layer approximations, and a learned-variance head — and to
separate epistemic from aleatoric error (RQ2); to prove and empirically verify coverage of the
conformal predictive intervals, accounting for the Monte-Carlo noise in the labels (RQ3); and to
conduct a capital-impact study on Human Mortality Database calibrations, with an optional South
African case study (RQ4). Each objective maps to a stated contribution in Section 7.

## 4. Literature Review

**Variable-annuity guarantees and nested valuation.** The pricing of guaranteed minimum benefits
under a unifying framework was set out by Bauer, Kling and Russ (2008), who showed how the various
GMxB riders can be valued within a single nested-simulation architecture. This framework underlies
the ground-truth labels used in this project and fixes the contract conventions — fee deduction,
guarantee bases, surrender charges — that define the inputs.

**Emulation and metamodelling of VA portfolios.** Hejazi and Jackson (2016, 2017) introduced a
neural-network approach to the efficient valuation of large VA portfolios, learning the valuation
map from a representative set of contracts. Gan and Lin (2015, 2018) developed a complementary
metamodelling programme, including functional-data and two-level designs for both values and
Greeks. This literature establishes the feasibility and accuracy of emulation but stops at point
estimates under static mortality.

**Deep learning solvers for VA valuation (closest 2026 prior art).** Li and Lyu (2026) price a VA
with early surrender under rough-Heston volatility and Volterra stochastic mortality using deep
signature LSMC, with a rigorous convergence analysis. Langrené, Luo, Shevchenko and Zhang (2026)
solve the GMWB optimal-withdrawal control problem by deep LSMC and construct confidence intervals
for the contract value. The present proposal is positioned explicitly against these: it emulates
across the state space rather than solving a single contract, and it quantifies predictive rather
than estimator uncertainty.

**Stochastic mortality.** The Lee–Carter model (1992) remains the reference for mortality
forecasting, and affine stochastic-intensity models (Biffis, 2005) provide the continuous-time,
tractable dynamics most convenient for coupling with financial simulation. Longer-memory Volterra
specifications, as calibrated by Li and Lyu (2026), offer a richer data-generating process that
this project can adopt for its labels.

**Uncertainty quantification in deep learning.** Deep ensembles (Lakshminarayanan et al., 2017)
provide a simple and strong estimate of epistemic uncertainty; Monte-Carlo dropout interprets
dropout as approximate Bayesian inference (Gal and Ghahramani, 2016); and the decomposition of
predictive uncertainty into aleatoric and epistemic parts is formalised by Kendall and Gal (2017).
These supply the uncertainty representation but, on their own, carry no finite-sample guarantee.

**Conformal prediction.** The conformal framework (Vovk et al., 2005) and its split-conformal
regression form (Lei et al., 2018) produce prediction intervals with distribution-free,
finite-sample coverage under exchangeability. The synthesis pursued here — a neural VA emulator
whose predictive intervals are both uncertainty-decomposed and conformally certified, under
stochastic mortality — sits at the intersection of these strands and is, to the candidate's
knowledge, unaddressed.

## 5. Model Specification

**The emulator map.** Let `x` denote the state of a contract together with its market and
mortality environment, and let `Y` be the present value of the guarantee liability. The emulator is
a neural network `f_θ` trained to approximate the map `x ↦ Y`. The regression targets are generated
by nested Monte Carlo under a specified stochastic-mortality model: for each sampled state `x_i`,
an inner simulation produces a label `Y_i`, an unbiased but noisy estimate of the true value
`Y(x_i) = E[ discounted guarantee cashflows | x_i ]`. The true target is a deterministic function
of the state; the label carries Monte-Carlo noise whose variance is controlled by the inner path
count. This distinction between the deterministic target and the noisy label is central to the
uncertainty analysis in Section 6.

**The input vector.** The state `x` is assembled from three blocks.

- *Policy characteristics:* policyholder age; sex, where it enters the mortality basis and is
  handled appropriately; contract duration; initial premium; current account value; guarantee
  base; time to maturity; surrender-charge schedule; withdrawal rate; and guarantee type.
- *Financial-state variables:* the interest rate; the equity level or moneyness; equity
  volatility; the stochastic-volatility state; correlation parameters; and the fee rate.
- *Mortality variables:* the current mortality intensity; the mortality trend; mortality
  volatility; a cohort effect; the financial–mortality correlation; and the mortality-model
  parameters.

**The output.** The response is the single continuous quantity `Y`, the present value of the
guarantee liability. The emulator reports it in two forms: a point prediction `f_θ(x)`, and a
calibrated predictive interval `[L(x), U(x)]` constructed so that the true value is contained with
probability at least `1 − α`.

**Stochastic-mortality setup.** The force of mortality `μ_t` follows a stochastic model — an affine
intensity in the base specification, with the option of a Lee–Carter or Volterra formulation —
calibrated to life-table survival probabilities. The survival factor over `[t, s]` is the expected
exponential of the integrated intensity, and the mortality paths enter the valuation both through
the labels, where they determine the timing of death benefits, and through the state `x`, whose
mortality block records the current intensity and the model parameters. A financial–mortality
correlation parameter allows dependence between the two sources of risk.

**Uncertainty model.** The object of inference is the conditional law of the value given the state.
The predictive uncertainty is decomposed into an aleatoric part — the Monte-Carlo noise of the
labels and any genuine irreducibility in `Y | x` — and an epistemic part, the approximation and
estimation error of the emulator itself. The point head of the network estimates `f_θ(x)`; a
variance head or an ensemble spread estimates the uncertainty; and a conformal calibration step
converts these into intervals with a guaranteed coverage level.

## 6. Methodology

The work proceeds along three interlocking tracks.

**Emulation.** A reproducible pipeline generates nested-Monte-Carlo labels over a designed sample
of states, chosen to cover the policy, financial, and mortality ranges of interest. Features are
engineered from the raw state, including, for path-dependent contracts, signature features of the
simulated trajectories — a technique the candidate has used before and which Li and Lyu (2026) show
to be effective for exactly this class of problem. Feedforward networks serve as the baseline
architecture, with recurrent (GRU) variants where the path state matters. Training minimises a
regression loss against the labels, with regularisation and hyperparameters selected by Bayesian
optimisation. Accuracy is reported against held-out nested-Monte-Carlo values across moneyness,
age, maturity, and mortality regime.

**Uncertainty quantification.** Epistemic uncertainty is estimated by deep ensembles and,
comparatively, by Bayesian last-layer and Monte-Carlo-dropout approximations. Aleatoric
uncertainty is modelled by a learned-variance (Gaussian negative-log-likelihood) head that
captures the state-dependent label noise. These estimates are then wrapped in a split-conformal
layer: residuals on a held-out calibration set, suitably normalised by the predicted scale, yield
a quantile that defines distribution-free intervals. A methodological subtlety, and part of the
contribution, is that the labels are themselves noisy estimates of the true value, so the
calibration must account for Monte-Carlo label noise to certify coverage of the *true* `Y(x)`
rather than merely of the label. Calibration quality is assessed by empirical coverage, mean
interval width, the continuous ranked probability score, and reliability diagrams.

**Validation and application.** The emulator's values and intervals are benchmarked against full
nested Monte Carlo and against the LSMC prices of the 2026 literature on comparable contracts. The
method is then applied to compute reserves and a solvency-capital proxy, with and without the
quantified uncertainty, to show the practical consequence of calibrated intervals. An optional
South African case study uses StatsSA or ASSA mortality in place of the Human Mortality Database
calibration.

## 7. Statistical Contributions

The thesis targets four results, stated here as objectives to be established.

**C1.** A deep neural network emulator for the present value of a VA guarantee liability under
stochastic mortality, with documented accuracy relative to nested Monte Carlo across the state
space.

**C2.** A principled decomposition of the emulator's predictive uncertainty into epistemic and
aleatoric components appropriate to this valuation problem, together with a comparison of ensemble,
Bayesian, and variance-head estimators.

**C3.** The central contribution: a finite-sample coverage guarantee for the emulator's predictive
intervals via split-conformal prediction, including the treatment of Monte-Carlo label noise and
the argument that exchangeability holds by construction because training states are sampled
independently rather than observed as a dependent time series.

**C4.** As a stretch result, error and sample-complexity relations linking the inner-simulation
label budget, the network capacity, and the resulting interval width, giving practical guidance on
how to allocate computational effort for a target accuracy and coverage.

## 8. Computational Plan

The implementation uses PyTorch with GPU acceleration, following the candidate's established
workflow. The nested-Monte-Carlo label generator is a self-contained, seeded module whose inner
path counts and scenario designs are logged for reproducibility, and hyperparameter search uses
Bayesian optimisation. The intended deliverable is an open-source emulator with reproducible
notebooks that regenerate every figure and table, so that the accuracy and coverage claims can be
independently checked.

## 9. Data

Mortality is calibrated to the Human Mortality Database, whose cohort and period life tables
support both the affine and the Volterra intensity specifications. Contract parameters follow a
concrete VA specification — fee rate, guarantee level, surrender-charge schedule, and withdrawal
rate — and the training states are drawn from synthetic portfolios designed to span the input
ranges. Financial scenarios use standard equity and interest-rate models calibrated to market
data. An optional South African case study substitutes StatsSA or ASSA mortality, which also lets
the thesis speak to a data-sparse, structurally distinct mortality environment.

## 10. Significance and Expected Contribution

The project contributes on three levels. Theoretically, it brings a finite-sample coverage
guarantee to a deep surrogate for an actuarial valuation, a setting where uncertainty has so far
been quantified only as estimator error. Methodologically, it delivers a calibrated,
uncertainty-aware VA emulator that values heterogeneous contracts instantly while reporting how far
each value can be trusted. Practically, it gives insurers a fast tool for pricing, hedging, and
capital that is honest about its own error, which is directly relevant to solvency reporting.
Target outlets include the South African Actuarial Journal and the ASSA convention, with
international submissions to the Scandinavian Actuarial Journal, Insurance: Mathematics and
Economics, and the ASTIN Bulletin; the work is also eligible for the SASA student competitions.

## 11. Timeline

The programme is planned over three years, the minimum registration period.

| Phase | Period | Milestones |
|-------|--------|------------|
| Year 1 | Months 1–12 | Literature review; nested-Monte-Carlo label pipeline; baseline emulator and accuracy study (C1); first paper submitted |
| Year 2 | Months 13–24 | Uncertainty-quantification methods and the epistemic/aleatoric decomposition (C2); conformal coverage theory and experiments (C3); full numerical study; second paper submitted |
| Year 3 | Months 25–36 | Sample-complexity relations (C4); capital-impact study; optional South African case study; thesis write-up; third paper submitted; examination |

## 12. Ethical Considerations

The research uses only publicly available, aggregated data — the Human Mortality Database and,
where applicable, published South African mortality tables — together with synthetic contract
portfolios. No individual policyholder records are used. Standard ethics clearance will be obtained
from the University of the Free State prior to data work, in line with the candidate's MSc
practice.

## References

Bacinello, A. R. — see within the nested-valuation and VA literature cited above.

Bauer, D., Kling, A. and Russ, J. (2008). A universal pricing framework for guaranteed minimum
benefits in variable annuities. *ASTIN Bulletin*, 38(2), 621–651.

Biffis, E. (2005). Affine processes for dynamic mortality and actuarial valuations. *Insurance:
Mathematics and Economics*, 37(3), 443–468.

Gal, Y. and Ghahramani, Z. (2016). Dropout as a Bayesian approximation: representing model
uncertainty in deep learning. *ICML*, 1050–1059.

Gan, G. and Lin, X. S. (2015). Valuation of large variable annuity portfolios under nested
simulation: a functional data approach. *Insurance: Mathematics and Economics*, 62, 138–150.

Gan, G. and Lin, X. S. (2018). Efficient Greek calculation of variable annuity portfolios for
dynamic hedging: a two-level metamodeling approach. *North American Actuarial Journal*, 22(2),
161–177.

Hejazi, S. A. and Jackson, K. R. (2016). A neural network approach to efficient valuation of large
portfolios of variable annuities. *Insurance: Mathematics and Economics*, 70, 169–181.

Kendall, A. and Gal, Y. (2017). What uncertainties do we need in Bayesian deep learning for
computer vision? *NeurIPS*, 30.

Lakshminarayanan, B., Pritzel, A. and Blundell, C. (2017). Simple and scalable predictive
uncertainty estimation using deep ensembles. *NeurIPS*, 30.

Langrené, N., Luo, X., Shevchenko, P. V. and Zhang, R. (2026). Deep least squares Monte Carlo
methods for the valuation of variable annuities with guarantees. *arXiv:2605.27182*.

Lee, R. D. and Carter, L. R. (1992). Modeling and forecasting U.S. mortality. *Journal of the
American Statistical Association*, 87(419), 659–671.

Lei, J., G'Sell, M., Rinaldo, A., Tibshirani, R. J. and Wasserman, L. (2018). Distribution-free
predictive inference for regression. *Journal of the American Statistical Association*, 113(523),
1094–1111.

Li, W. and Lyu, H. (2026). Valuation of variable annuities under the Volterra mortality and rough
Heston models. *arXiv:2604.00472*.

Vovk, V., Gammerman, A. and Shafer, G. (2005). *Algorithmic Learning in a Random World*. Springer.
