# Scalable Inference Design Exploration

Date: 2026-09-23. Superpowers architectural brainstorming, not an approved
implementation spec. No model was fitted or changed during this exploration.
Extends `SCOPE.md` stages 4-5, TASK-007/008, and `PLATFORM_PLAN.md`.

## Intended Outcome

Make California and U.S. uncertainty analysis computationally practical while
preserving credible statements about underserved neighborhoods and interventions.
The method the user recalled is **variational inference (VI)**, already named in
`AGENT.md` and `SCOPE.md`. **ADVI** means automatic differentiation variational
inference. INLA, empirical Bayes, and conjugate approximations were also listed.

Model specification, inference algorithm, and data storage are separate choices.
DuckDB can help prepare tables; it does not solve posterior inference. Millions
of GTFS rows need not become millions of observations in the tract model: measure
routing cost, tract/latent-variable count, graph cost, and inference separately.

## What the Current Code Does

- `notebooks/04_bayesian_model.ipynb`, sampling cell: calls `pm.sample` with
  configured draws, warmup, and chains. It does not automatically keep sampling
  until R-hat passes. Defaults in `configs/defaults.yaml` are 8,000 retained draws
  per chain, 4,000 warmup steps, and four chains; notebook/config overrides apply.
- `init="advi+adapt_diag"` uses ADVI for initialization. The final fit is still
  NUTS MCMC, not a variational posterior.
- `scripts/nb04_export_diagnostics.py::convergence_assessment` checks R-hat <= 1.01
  after fitting. R-hat measures agreement across chains; it is not 99% accuracy
  or proof of model validity. `target_accept: 0.99` is a different NUTS setting.
- Check effective sample sizes, Monte Carlo error, divergences, and predictive
  adequacy too. F025 records an intercept R-hat around 1.011 in a historical run;
  F029 documents predictive/LOO limitations despite improved sampling diagnostics.

The [rank-normalized R-hat paper](https://arxiv.org/abs/1903.08008) explains the
diagnostic and why chain agreement alone is insufficient. MCMC is a numerical
reference with finite-sampling error, not an infallible scientific ground truth.

## Three Design Paths

| Path | Mechanism and attraction | Main tradeoff | Proposed role |
| --- | --- | --- | --- |
| A. Improve MCMC | Sparse spatial setup, better parameterization, and a measured comparison of compatible NUTS backends such as nutpie/NumPyro. | Still MCMC; runtime gains depend on model, hardware, and compilation. | Preserve the current scientific reference and improve regional validation runs. |
| B. VI / ADVI | Optimize a tractable distribution to approximate the posterior, then obtain draws or summaries from that approximation. | Mean-field independence can miss correlations and underestimate uncertainty; convergence of the optimizer does not prove accuracy. | Fast candidate for broad screening, promoted only after uncertainty QA. |
| C. INLA / spatial Laplace inference | Exploit a latent Gaussian spatial field and sparse precision structure to approximate posterior marginals. | Restricted model class; likelihood, priors, constraints, and scaling must match. An R-INLA pilot adds a separate offline toolchain. | Serious candidate for routine spatial releases, benchmarked against MCMC. |

VI replaces posterior exploration with optimization; generating more draws from
a fitted approximation does not remove its approximation bias. See the
[VI review](https://arxiv.org/abs/1601.00670) and
[PyMC ADVI definition](https://www.pymc.io/projects/docs/en/stable/api/generated/pymc.ADVI.html).
Full-rank VI can represent correlations but needs quadratic covariance storage;
it is not an automatic national-scale solution. Structured/sparse or low-rank
approximations are follow-ups if mean-field fails, not assumed existing features.

INLA integrates over hyperparameter uncertainty as well as approximating the
latent field. It is more than a single Gaussian approximation at the joint mode.
Its suitability here is a hypothesis based on the spatial model structure,
not a measured result. See the [INLA overview](https://www.r-inla.org/whatisinla/)
and [BYM2 model definition](https://www.inla.r-inla-download.org/r-inla.org/doc/latent/bym2.pdf).

Secondary candidates, after the first comparison:

- **Pathfinder:** an optimization-based variational approximation useful for
  initialization or screening; assess importance-weight diagnostics before using
  its draws for conclusions ([Stan documentation](https://mc-stan.org/docs/reference-manual/pathfinder.html)).
- **Empirical Bayes:** fit hyperparameters and condition on those estimates for
  cheaper shrinkage. Quantify uncertainty lost by treating those values as known.
- **Conjugate/conditionally Gaussian methods:** analytic conditional calculations
  can reduce sampling, but changing the Student-t likelihood or fixing learned
  variance parameters changes the model. The current model is not fully conjugate.

Recommendation for a first experiment: A as the reference, B and C as competing
approximations. Select by probability/interval fidelity and cost, not familiarity.

## Improvements Beyond Replacing the Sampler

1. **Make spatial computation sparse.** `src/modeling/spatial.py` allocates dense
   float64 adjacency in `adjacency_from_queen` and calls `np.linalg.inv` in
   `scaling_factor_sp`. One dense matrix for an illustrative 80,000 tracts costs
   51.2 GB before copies or factorization (80,000 squared times 8 bytes); this is
   not an estimate of the actual national tract count. Preserve sparse edges and
   use validated constrained sparse calculations rather than a full inverse.
   This bottleneck exists before the sampler choice.
2. **Verify the spatial prior before comparing engines.** `_build_icar_prior`
   assigns Normal priors to free coordinates, sets the last to their negative
   sum, and adds an ICAR penalty. Its density includes that Normal prior, so
   stock BYM2/ICAR is not automatically equivalent. Audit covariance, ordering
   invariance, component constraints, island handling, and the ridge-based
   scaling calculation on small graphs. A correction becomes a new model
   version; do not attribute resulting posterior changes to inference alone.
3. **Evaluate regional models with shared information.** A national hierarchy
   with regional spatial fields may distribute computation, but independent
   county fits change pooling and boundary behavior. Benchmark cross-border
   corridors and rural/island cases; retain uncertainty in shared parameters.
   Refitting only individually flagged tracts is not equivalent to refitting
   their coupled spatial model.
4. **Compute once, explore many thresholds.** Precompute marginal distributions
   or CDF grids for `P(access < threshold)` and supply intervals/summaries to the
   app. Moving an accessibility cutoff within a fixed fitted outcome need not
   rerun inference. A different travel-time budget or route design can change
   the outcome and require routing/refitting. Keep numerical interpolation error
   bounded and expose only supported controls.
5. **Retain dependence for interventions.** Marginals suffice for individual
   tract probabilities. Aggregate interval estimates, simultaneous spatial
   events, and paired intervention differences can require joint or coupled
   draws. Independently sampling tract marginals would discard dependence.
   Validate joint outputs before replacing existing scenario draw processing.
6. **Distinguish kinds of uncertainty.** Posterior uncertainty, numerical error,
   input/service variability, and data-quality gaps are different. More MCMC
   draws only reduce numerical error. Future departure-window or input ensembles
   could propagate additional uncertainty, but require an explicit observation
   model; scheduled GTFS alone does not measure actual operational reliability.

## Small Benchmark Before a Migration

1. Freeze one reproducible San Diego bundle: source versions, tract order,
   adjacency, transforms, likelihood, priors, model version, and policy thresholds.
   Check the spatial-prior issues above before calling two engines equivalent.
2. Reuse a compatible MCMC fit only if diagnostics and required quantities are
   adequate; otherwise budget a new reference fit. Compare all candidates on
   the same target. Treat a revised model as a separate experiment.
3. Compare at least VI and an INLA-compatible candidate with the reference on
   San Diego, then a contrasting pilot. Predeclare tolerances before inspecting
   results; preserve an independent validation region. Do not select only easy
   dense urban tracts or remove neighbors to make a favorable comparison.
4. Log cold compilation and repeated-fit time separately, peak RAM, artifact
   size, and seed/restart stability. For MCMC record R-hat, bulk/tail ESS, MCSE,
   and divergences; for VI inspect optimization/restart stability and output
   agreement. R-hat from independent draws of a biased approximation cannot
   certify that approximation.
5. Compare probability errors near policy thresholds, interval widths, tails,
   tract/group summaries, and intervention-priority/crossing agreement. Match
   interval definitions and include MCMC numerical error. Mean agreement alone
   cannot qualify a method for an uncertainty dashboard.
6. Separate approximation fidelity from statistical calibration: use simulated
   truth for interval/probability calibration and spatially blocked predictive
   checks for generalization. Account for shared spatial effects when holding
   out observations. F029's failed PSIS-LOO estimates are not a valid scorecard.

Keep benchmark artifacts and acceptance/rejection rationale in project-local
MLflow and portable run bundles. No speedup, accuracy, or cloud cost has yet
been measured for these alternatives in this repository.

## Product and Architecture Consequences

Use the same export boundary for whichever method passes validation. Record
model version, inference method, numerical diagnostics, and validation coverage
with each release. A compact fit-to-summary contract is sufficient initially;
implement a second backend before designing a general backend registry.

Routine regional releases could use the validated approximation, with MCMC
audits on representative and difficult regions. Use disagreement to prioritize
additional regional analysis, without silently combining incompatible fits.
The public map should explain what its uncertainty represents; an optional
research view can expose approximation differences. Keep inference offline.

Next design decision: choose the first benchmark's model target, representative
pilot, uncertainty tolerances, and time/memory budget. No backend is selected yet.

Documentation QA: inspected the model, spatial helpers, sampling cell, config,
and diagnostics; verified local links and context pointers; `git diff --check`
passed. No numerical benchmark or application tests were run for this document.
