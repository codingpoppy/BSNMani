# BSNMani

### Bayesian Scalar-on-Network Regression with Manifold Learning

**Learning shared network structure and its association with individual clinical outcomes.**

[Paper](https://doi.org/10.1214/26-AOAS2140) · [Preprint](https://arxiv.org/abs/2410.02965) · [Research website](https://codingpoppy.github.io/research.html)

BSNMani is a Bayesian framework for relating subject-specific networks to scalar outcomes. It learns shared connectivity patterns and individual network features, then links those features to clinical phenotypes while adjusting for covariates. The paper demonstrates the approach with simulations and resting-state fMRI data from a study of major depressive disorder.

This repository contains the R and C++ source code submitted as supplementary material for the paper in *The Annals of Applied Statistics*.

## Method at a glance

- **Network representation:** manifold learning extracts shared connectivity structure and subject-specific network features.
- **Outcome modeling:** a joint predictive model relates those features to a clinical outcome with covariate adjustment.
- **Bayesian computation:** a two-stage algorithm combines the Metropolis-adjusted Langevin algorithm (MALA) with Gibbs sampling, followed by a Metropolis–Hastings adjustment.
- **Posterior assessment:** helper functions support posterior summaries and convergence diagnostics.

## Start here

The analysis entry point is [`two_stage_single_chain_train()`](BSNMani_two_stage_pipeline.R), defined in `BSNMani_two_stage_pipeline.R`. It coordinates initialization, C++ compilation, sampling, and optional saving or loading of stage-specific results.

From an R session with the repository root as the working directory, inspect the interface with:

```r
source("BSNMani_two_stage_pipeline.R")
args(two_stage_single_chain_train)
```

This loads the function definition; it does not fit a model. A model run requires data and an explicit analysis configuration, including priors, latent dimension, initialization, sampler settings, and output paths. See the paper and supplementary material for the statistical specification.

The main data arguments are:

| Argument | Expected content |
| --- | --- |
| `FC_dat` | An array of subject-specific network matrices, with dimensions `n_roi × n_roi × n_pol`. |
| `clinical_df` | The scalar outcome for each subject. |
| `cov_df` | Subject-level covariates, ordered to match the networks and outcomes; the pipeline adds an intercept. |
| `idx_ls` | Groups of node indices used by the pipeline to select representative regions. |

When saving is enabled, the pipeline writes RDS results under `g1/`, `g2/`, and `MH/` within the configured output locations. Output paths and filenames must also satisfy the supplied `dir_tags` and `basename_tags` checks.

## Source files

| File | Purpose |
| --- | --- |
| [`BSNMani_two_stage_pipeline.R`](BSNMani_two_stage_pipeline.R) | Main analysis function and stage orchestration. |
| [`BSNMani_BFGS_initialization.R`](BSNMani_BFGS_initialization.R) | Objective and gradient functions for BFGS initialization. |
| [`BSNMani_diagnostics_helpers.R`](BSNMani_diagnostics_helpers.R) | Posterior processing and convergence-diagnostic helpers. |
| [`hybrid_MALA_Gibbs_stage1.cpp`](hybrid_MALA_Gibbs_stage1.cpp) | First-stage network-model sampling. |
| [`hybrid_MALA_Gibbs_stage2.cpp`](hybrid_MALA_Gibbs_stage2.cpp) | Second-stage outcome-model sampling. |
| [`hybrid_MALA_Gibbs_A_lambda.cpp`](hybrid_MALA_Gibbs_A_lambda.cpp) | Metropolis–Hastings adjustment involving the network-feature parameters. |

## Requirements and reproducibility

The supplementary code lists **R 4.2.0** as its reference version. The pipeline uses `Rcpp`, `RcppArmadillo`, `SparseGrid`, `matrixStats`, `data.table`, `pracma`, `dplyr`, `tidyr`, `coda`, `abind`, and `fs`. A working C++ compilation toolchain is needed for `Rcpp::sourceCpp()`.

**Data and a complete runnable example are not bundled in this repository.** Users must supply compatible data, configure the analysis arguments, and adapt output paths to their environment. Package versions are not pinned. Keep the R and C++ files together, since the pipeline sources helper files using relative paths.

## Citation

Li, Y., Choi, K. S., Dunlop, B. W., Craighead, W. E., Mayberg, H. S., Garmire, L., Guo, Y., and Kang, J. (2026). **BSNMani: Bayesian scalar-on-network regression with manifold learning.** *The Annals of Applied Statistics*, **20**(2), 1496–1515. [doi:10.1214/26-AOAS2140](https://doi.org/10.1214/26-AOAS2140).

## Related work

[SPIN](https://github.com/lanagarmire/SPIN) applies BSNMani to spatial molecular networks for population-scale clinical prediction and biological interpretation.

For research context and contact information, visit [Yijun Li's website](https://codingpoppy.github.io/). For questions about this code, [open an issue](https://github.com/codingpoppy/BSNMani/issues).
