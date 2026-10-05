# PhD Research Proposal (Skeleton)

**Working title:** Reflected Backward Stochastic Differential Equations and Optimal
Stopping for Variable-Annuity Surrender Guarantees under Stochastic Mortality in
Incomplete Markets

**Candidate:** Thabang Baloyi · **Supervisor:** Dr. Jan Blomerous (FASSA)
**Degree:** PhD (Mathematical / Applied Statistics), University of the Free State
**Proposed start:** 2027 · **Date:** _____

> Target length **10–12 pages**. Page budget per section is shown in〔brackets〕.
> Keep it high-level (research landscape, problem, questions, methods, data, timeline)
> but with more depth and lit than the master's proposal. Statements of theorems, not proofs.

---

## Abstract 〔~0.5 pg〕
- 150–250 words. One paragraph that names: the product (variable-annuity surrender
  guarantees), the mathematical object (reflected BSDE / Snell envelope), the two
  complications (stochastic mortality + market incompleteness), the method (optional
  decomposition + deep-BSDE solver), and the headline contribution (existence/uniqueness
  + super-hedging duality + a convergence-guaranteed deep scheme, validated on real data).

## 1. Introduction and Background 〔~1.5 pg〕
- 1.1 Variable annuities and their embedded guarantees (GMAB/GMDB/GMWB) — scale of the
  market, why insurers carry the risk.
- 1.2 The **surrender (lapse) option** as an American-style feature → "exercise anytime."
- 1.3 Why valuation/hedging is hard: random horizon (mortality), unhedgeable mortality
  risk, high dimensionality.
- 1.4 Positioning vs. the candidate's MSc (super-hedging of American options via optional
  decomposition + deep learning) — this PhD extends that machinery from *financial*
  American options to *insurance* American-style guarantees.
- 1.5 One short paragraph stating the aim in plain words.

## 2. Problem Statement and Research Gap 〔~1 pg〕
- 2.1 What is solved today: RBSDE theory for American options (El Karoui et al. 1997);
  deep BSDE / deep optimal stopping (Han–Jentzen–E; Becker–Cheridito–Jentzen);
  VA valuation under deterministic or simple mortality.
- 2.2 **The gap:** RBSDEs with a *stochastic mortality intensity* (random horizon,
  doubly-stochastic structure) in an *incomplete* market are under-developed; no
  optional-decomposition / super-hedging duality has been established for this setting;
  deep solvers for it lack convergence guarantees.
- 2.3 One-sentence statement of the problem this thesis closes.

## 3. Research Questions and Objectives 〔~0.75 pg〕
- **RQ1.** How is the value/super-hedge of a VA surrender guarantee characterised as a
  reflected BSDE when the death time has a stochastic intensity and the market is incomplete?
- **RQ2.** Does this RBSDE admit a unique solution, and does a super-hedging (optional-
  decomposition) duality hold?
- **RQ3.** Can a deep-learning scheme approximate the solution with provable convergence /
  error bounds?
- **RQ4.** What do calibrated numerics (HMD + a concrete VA contract) reveal about hedge
  cost, surrender behaviour, and model risk?
- **Objectives** (bulleted, mapped 1:1 to the RQs and to the theorems in §7).

## 4. Literature Landscape 〔~2 pg — the "more lit" section Blomerous wants〕
Group into themes, ~1 paragraph each; cite accurately.
- 4.1 Reflected BSDEs & optimal stopping: El Karoui, Kapoudjian, Pardoux, Peng, Quenez (1997);
  Snell envelopes.
- 4.2 Optional decomposition & super-hedging in incomplete markets: El Karoui–Quenez (1995),
  Kramkov (1996), Föllmer–Kabanov. (Bridge to MSc.)
- 4.3 Stochastic mortality models: Lee–Carter (1992); Cairns–Blake–Dowd; affine intensity
  models — Biffis (2005), Dahl, Luciano–Vigna.
- 4.4 Variable annuities / surrender behaviour: Bauer–Kling–Russ, Bacinello, Milevsky.
- 4.5 Deep learning for BSDEs/PDEs & optimal stopping: Han–Jentzen–E (2017/18);
  Becker–Cheridito–Jentzen (2019/20); Buehler et al. "Deep Hedging" (2019).
- 4.6 (Optional) approximation theory / curse of dimensionality: Grohs et al.; Hutzenthaler et al.
- 4.7 Short "synthesis" paragraph: where the gaps intersect = this thesis.

## 5. Mathematical Formulation and Preliminaries 〔~1.5 pg〕
- 5.1 Market & filtration: Brownian financial factors $W$, traded assets $S$, short rate $r$;
  state incompleteness ($m$ risk sources $>$ $d$ traded assets).
- 5.2 Mortality: death time $\tau_x$ with stochastic intensity $\mu_t$ (affine), enlarged
  filtration $\mathbb{G}=\mathbb{F}\vee\mathbb{H}$; random horizon $\tau = \min(T,\tau_x)$.
- 5.3 The guarantee payoff $P_t$ (surrender value / benefit) as the **obstacle**.
- 5.4 Value as a Snell envelope:
  $V_t=\operatorname*{ess\,sup}_{\theta\ge t}\mathbb{E}[e^{-\int_t^\theta r}P_\theta\mid\mathcal{G}_t]$.
- 5.5 The **reflected BSDE** with obstacle $L$ and reflecting process $K$:
  $Y_t=\xi+\int_t^T f(s,Y_s,Z_s)\,ds+(K_T-K_t)-\int_t^T Z_s\,dW_s$,
  $Y_t\ge L_t$, $\int_0^T(Y_t-L_t)\,dK_t=0$.
- 5.6 Super-hedging / optional-decomposition statement (to be generalised in §7).
- Keep this to *definitions + the equation*; defer proofs.

## 6. Methodology 〔~1 pg〕
- 6.1 Theory track: derive the RBSDE, prove well-posedness and duality (feeds §7).
- 6.2 Numerical track: deep-BSDE / deep optimal-stopping discretisation; neural nets for
  $(Y,Z)$ and the stopping rule; architecture choices from MSc experience (FNN/GRU);
  loss = discretised RBSDE residual + reflection penalty.
- 6.3 Validation track: benchmark vs. known closed forms / tree methods in simplified cases;
  convergence experiments; calibration to data.

## 7. Theoretical Contributions (Theorems to be Established) 〔~0.75 pg〕
State as propositions "to be proved":
- **T1.** Existence & uniqueness of the solution $(Y,Z,K)$ of the mortality-driven RBSDE.
- **T2.** Super-hedging duality / optional decomposition of the mortality-intensity
  supermartingale (generalising the MSc result).
- **T3.** Convergence and a priori / a posteriori error bounds for the deep scheme.
- (Stretch) **T4.** Robust extension under model uncertainty (G-expectation / 2BSDE) — flag
  as a later chapter.

## 8. Computational / Experimental Plan 〔~0.5 pg〕
- Framework: PyTorch; GPU (vast.ai, as in MSc). Seeds, reproducibility, hyperparameter
  search (Optuna). Deliverable: an open-source solver + reproducible notebooks.

## 9. Data 〔~0.5 pg〕
- Human Mortality Database (HMD) for mortality/intensity calibration; a concrete VA contract
  specification (fees, guarantee level, surrender schedule); market factors (rates, fund
  index). Note South African data option (StatsSA / ASSA) for a local case study.

## 10. Significance and Expected Contribution 〔~0.5 pg〕
- Theoretical (new well-posedness + duality), methodological (guaranteed deep solver),
  practical (insurer hedging of surrender/longevity risk; model-risk insight). Note
  publication targets (SAAJ/ASSA; Scandinavian Actuarial Journal; Insurance: Math & Econ)
  and competition eligibility (SASA).

## 11. Timeline 〔~0.5 pg — Blomerous requires this〕
3-year plan (adjust to UFS 3-yr minimum):
- **Year 1:** lit review; formulate RBSDE; prove T1; set up deep solver; first paper (survey/framework).
- **Year 2:** prove T2 (duality) + T3 (convergence); full numerics on HMD + VA; second paper.
- **Year 3:** robust/model-uncertainty extension (T4); SA case study; write-up; third paper; submit.
- Include a simple Gantt table.

## 12. Ethical Considerations 〔~0.25 pg〕
- Publicly available / aggregated data only (HMD); no personal policyholder data; standard
  UFS ethics clearance statement.

## References 〔~1 pg〕
- Alphabetical, (author, year) style to match UFS guideline; ensure accuracy (Blomerous
  flagged referencing heavily in the MSc). Core: El Karoui et al. (1997); Kramkov (1996);
  El Karoui–Quenez (1995); Biffis (2005); Lee–Carter (1992); Cairns–Blake–Dowd;
  Han–Jentzen–E (2018); Becker–Cheridito–Jentzen (2019); Buehler et al. (2019);
  Bauer–Kling–Russ.

---
### Page-budget check
Abstract 0.5 + Intro 1.5 + Problem 1 + RQs 0.75 + Lit 2 + Maths 1.5 + Method 1 +
Theorems 0.75 + Compute 0.5 + Data 0.5 + Significance 0.5 + Timeline 0.5 + Ethics 0.25 +
Refs 1 ≈ **12.25 pages** (trim Lit/Maths to land at 10–12).
