# Mandrake Bio: Reverse Transcriptase Phase 2 Submission
**Author:** Gaurav Parkhedkar | AI & Data Science Engineering 

## Abstract & Generalization Strategy
This repository contains 5 orthogonal modeling pipelines engineered for the Phase 2 wet-lab validation of the Retroviral Wall Challenge. 

Recognizing that Phase 2 introduces a severe out-of-distribution (OOD) domain shift with ~40 unseen, highly efficient, and potentially miniaturized RT candidates, this architecture abandons linear metric gaming in favor of **biophysical generalization**. 

To guarantee survival against wet-lab domain shifts, this pipeline enforces three strict rules:
1. **Zero Data Leakage:** All `foldseek` phylogenetic columns are explicitly dropped. The models are blinded to evolutionary family labels to prevent memorization and force the learning of universal physics.
2. **Scale-Invariant 3D Physics:** Instead of absolute spatial measurements, the extraction engine computes *Miniaturization-Invariant* features (e.g., Dipole Density, Cleft Ratio). This ensures the model correctly evaluates highly truncated Phase 2 enzymes without penalizing them for their absolute physical size.
3. **Bounded Extrapolation:** The primary architectures utilize Tree Ensembles to strictly bind predictions to known thermodynamic limits, preventing the mathematical collapse of the CLS (Weighted Spearman) metric during OOD ranking.

## Repository Structure & Model Diversity
To cover every possible Phase 2 biological scenario, 5 independent pipelines are provided. All pipelines share the same core extraction engine but utilize different mathematical routing:

* `notebook_01_tree_ensemble.ipynb`: **The Bounded Shield** (Voting Regressor of RF/ExtraTrees). *Primary submission. Prevents linear extrapolation on structurally alien enzymes.*
* `notebook_02_linear_ridge.ipynb`: **The Thermodynamic Anchor** (Ridge with L2=15.0). *Heavily regularized linear mapping with MinMax fold calibration to prevent global scale collapse.*
* `notebook_03_spatial_svr.ipynb`: **The Geometric Manifold** (RBF SVR). *Maps the non-linear spatial distance between enzymes in a 15-dimensional ESM-2 PCA space.*
* `notebook_04_sequence_only_hedge.ipynb`: **The 1D Insurance Policy** (ElasticNet). *A fallback pipeline that completely drops 3D features, relying purely on 1D sequence thermodynamics in the event of AlphaFold PDB hallucination failures in Phase 2.*
* `notebook_05_meta_stacking.ipynb`: **The Meta-Learner Synthesis** (Stacking Regressor). *An ensemble router that dynamically allocates predictions across linear, tree, and spatial algorithms.*

## Extraction Protocol for Mandrake Engineers
The code has been strictly modularized per Phase 2 rules. To extract the modeling pipeline and apply it to the 40 new wet-lab sequences:

1. Copy the three core functions located in `PART 1` of any notebook:
   - `engineer_thermodynamics(df)`
   - `extract_3d_physics(row)`
   - `build_mandrake_pipeline(physics_cols, esm_cols)`
2. Ensure the Kaggle environment (or local server) has the biological parsers installed: `pip install bio biopython`.
3. Pass your new Phase 2 `.csv` and `.pdb` files through the extraction functions. The resulting dataframe can be passed directly into the `build_mandrake_pipeline` without altering a single line of the underlying architecture.
