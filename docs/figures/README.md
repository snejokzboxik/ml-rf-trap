# Article-ready figures

All article figures use English labels, a white background, consistent typography, and neutral publication styling. PNG files are high-resolution raster previews; PDF and SVG files retain vector text and line work where the underlying artist is vector-based.

## `article_concept_pipeline`

- **Files:** `article_concept_pipeline.png`, `.pdf`, `.svg`
- **Type:** schematic, not a numerical field result
- **Source:** project geometry, coordinate dimensions, forward-FEM workflow, robust three-minimum extraction, and inverse-regression conventions
- **Article note:** the triangular mesh is deliberately regular and schematic. The pseudopotential panel contains exactly three marked quasi-equilibrium points. The schematic is suitable as a guide for later manual vector redrawing.
- **Caption:** *Conceptual pipeline for inverse reconstruction of RF-trap electrode displacements. A displaced four-electrode geometry is propagated through the forward FEM model, three pseudopotential minima are extracted to form supervised samples, and an inverse MLP reconstructs the eight displacement coordinates.*

## `article_fem_connections`

- **Files:** `article_fem_connections.png`, `.pdf`, `.svg`
- **Type:** real numerical output from the existing 2D FEM solver
- **Source:** row index 0, sample ID 1, from the clean merged N=51974 dataset
- **Geometry and mesh:** the same absolute displaced geometry and the same 3,996-node, 7,384-triangle practical mesh are used in all six panels. The central mesh size is 500 µm.
- **Connections:** quadrupole checkerboard `(F1,F2,F3,F4)=(+1,-1,-1,+1) V`; in-phase `(F1,F2,F3,F4)=(+1,+1,+1,+1) V`.
- **Article note:** each connection has a mesh panel, a scalar electric-potential panel, and an effective-potential panel. All six panels use the same moderate ±14 mm field of view, so substantial segments of all four displaced electrodes remain visible while the central structure is readable. Electric-potential panels use restrained central-percentile normalization and softened colormaps to avoid aggressive saturation. Effective-potential panels retain logarithmic scaling of the real `Psi = |E|^2` values; denser contours near low-field wells and larger red markers make the robust minima explicit without changing their coordinates. The quadrupole panel contains 1 detected minimum; the in-phase panel clearly shows exactly 3 detected robust minima.
- **Caption:** *Real two-dimensional FEM comparison for one displaced electrode geometry. For both quadrupole and in-phase electrode connections, the figure shows the practical FEM mesh, the scalar electric potential distribution φ, and the effective-potential proxy Ψ = |E|². All panels use the same ±14 mm field of view, with visible segments of the four displaced electrodes. Display normalization and contours reveal the central field structure without altering the numerical solution. The quadrupole connection yields one robust minimum, whereas the in-phase connection yields exactly three robust minima.*

The associated `article_fem_connections_metadata.json` records the selected displacements, connection definitions, mesh counts, residuals, electric-potential color limits, minima coordinates, and output paths.

## `article_learning_curves`

- **Files:** `article_learning_curves.png`, `.pdf`, `.svg`
- **Type:** quantitative result figure
- **Source:** `validation_results/learning_curve_merged_29995/learning_curve_metrics.csv`
- **Article note:** the three panels show the exact tracked MLP MAE, RMSE, and maximum absolute coordinate error for dataset sizes 1,000, 5,000, 10,000, 20,000, and 29,995. The maximum-error series is non-monotonic and is shown without smoothing.
- **Caption:** *Learning curves of the inverse MLP for increasing numbers of FEM-generated samples. Mean and root-mean-square coordinate errors decrease with dataset size, while the maximum coordinate error remains dominated by individual tail cases.*

## `article_model_comparison`

- **Files:** `article_model_comparison.png`, `.pdf`, `.svg`
- **Type:** quantitative result figure
- **Source:** `validation_results/inverse_model_merged_51974/metrics.csv`
- **Article note:** all bar-chart axes start at zero. The panels compare Ridge, Random Forest, and MLP on the same N=51974 train/test split.
- **Caption:** *Comparison of inverse-regression models trained on the merged N=51974 dataset. The MLP provides the lowest test MAE, RMSE, and maximum absolute coordinate error among the evaluated baseline models.*

## Reproduction

Run from the repository root:

```text
python docs/figures/generate_article_figures.py
```

The script validates the tracked metric tables before plotting and requires the selected in-phase FEM case to reproduce exactly three stored clean minima.

