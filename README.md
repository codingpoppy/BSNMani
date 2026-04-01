# BSNMani supplementary code

This folder contains source code submitted as supplementary material for the paper
“BSNMani: Bayesian Scalar-on-network Regression with Manifold Learning.”

## Main pipeline
- `BSNMani_two_stage_pipeline.R`: main script for the two-stage BSNMani analysis pipeline.

## Additional files
- `BFGS_init.R`: initialization functions used in model fitting.
- `diagnostics_helper_funcs.R`: helper functions for posterior diagnostics and convergence checks.
- `hybrid_MALA_Gibbs_Alambda.cpp`: C++ source file for updating `A(lambda)`.
- `hybrid_MALA_Gibbs_p1.cpp`: C++ source file for step 1 of the hybrid MALA-Gibbs sampler.
- `hybrid_MALA_Gibbs_p2.cpp`: C++ source file for step 2 of the hybrid MALA-Gibbs sampler.

## Requirements
- R (4.2.0)
- Rcpp
- RcppArmadillo
- SparseGrid
- matrixStats
- data.table
- pracma
- dplyr
- tidyr
- coda
- abind

## Notes
- Data are not included in this folder.
- Users may need to update file paths depending on their local environment.
- The files included here correspond to the code submitted as supplementary material for the paper.
- Future updates to BSNMani may be made available through the project GitHub repository: `github.com/codingpoppy/BSNMani`