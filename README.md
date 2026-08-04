# ML RF Trap

This repository contains a clean publication-ready version of a reproducible
2D RF-trap inverse-modelling workflow:

```text
FEM synthetic data -> inverse ML model -> prediction interface -> article figures
```

The forward model builds a four-electrode 2D geometry, solves Laplace's
equation with Gmsh and scikit-fem, extracts three pseudopotential minima, and
uses those six coordinates as inputs to an inverse regression model that
predicts eight electrode-displacement coordinates.

## Contents

- `src/rf_trap_forward/`: forward FEM utilities, inverse-model training/export
  helpers, prediction CLI, and row-level FEM verification helpers.
- `app_inverse_model_tk.py`: Tkinter GUI for saved inverse-model predictions.
- `docs/USING_INVERSE_MODEL.md`: detailed CLI, CSV, GUI, units, and electrode
  ordering reference.
- `docs/ARTICLE_ML_METHODS_RU.md` and `docs/article/ml_methods_ru.docx`: article
  methods text.
- `docs/figures/`: final article figures and the script used to regenerate
  them.
- `validation_results/inverse_model_merged_51974/`: final saved MLP model and
  model metrics.
- `validation_results/prediction_export_merged_51974/`: compact prediction
  export used by downstream examples.
- `validation_results/fem_order_prediction_dataset_100/` and
  `validation_results/fem_order_prediction_dataset_2000/`: compact FEM-order
  datasets for direct comparison or substitution.
- `validation_results/article_fem_sample_51974/`: one compact source row used to
  reproduce the numerical FEM article figure.
- `validation_results/learning_curve_merged_29995/learning_curve_metrics.csv`:
  metrics used by the learning-curve figure.

Large raw generated datasets, obsolete experiments, local caches, and non-final
model artifacts are intentionally not included.

## Installation

Use Python 3.11 or newer:

```powershell
python -m venv .venv
.venv\Scripts\python -m pip install -e ".[test]"
```

On Unix-like systems, replace `.venv\Scripts\python` with
`.venv/bin/python`.

## CLI Prediction

The default prediction model is
`validation_results/inverse_model_merged_51974/mlp.joblib`.

Run one direct prediction from three minima in millimetres:

```powershell
python -m rf_trap_forward.predict_inverse --minima "-1.596,3.869;-1.836,-3.034;4.218,-1.076" --units mm
```

Run batch prediction from a CSV:

```powershell
python -m rf_trap_forward.predict_inverse --input-csv examples/example_minima_input.csv --output-csv validation_results/manual_predictions/predicted_displacements.csv --units m
```

Inputs are sorted into the training pipeline's canonical polar-angle order by
default. See [docs/USING_INVERSE_MODEL.md](docs/USING_INVERSE_MODEL.md) for
the full interface contract and output column definitions.

## GUI Prediction

Launch the Tkinter interface from the repository root:

```powershell
python app_inverse_model_tk.py
```

The GUI loads the final MLP by default, accepts direct minima or CSV input,
shows Wolfram-order and FEM-order displacement vectors, and can export
predictions to CSV.

## Article Figures

Final article figures are stored in `docs/figures/` as PNG, PDF, and SVG files:

- `article_concept_pipeline.*`
- `article_fem_connections.*`
- `article_learning_curves.*`
- `article_model_comparison.*`

Regenerate them from the tracked compact artifacts with:

```powershell
python docs/figures/generate_article_figures.py
```

The generator validates tracked metric tables and uses
`validation_results/article_fem_sample_51974/synthetic_clean_ml_row0.csv` for
the numerical FEM connection comparison.

## Included Datasets And Artifacts

- `prediction_export_merged_51974`: 300-row prediction export with true
  displacements, minima, saved-model predictions, and error summaries.
- `fem_order_prediction_dataset_100`: first 100 rows converted to FEM order.
- `fem_order_prediction_dataset_2000`: first 2,000 rows converted to FEM order.
- `learning_curve_merged_29995/learning_curve_metrics.csv`: compact metrics for
  figure regeneration.
- `article_fem_sample_51974/synthetic_clean_ml_row0.csv`: representative clean
  FEM sample for the article connection figure.

All metre-valued columns end in `_m`; micrometre-valued columns end in `_um`.
The documented Wolfram-to-FEM transform is `F1,F2,F3,F4 = -[W3,W1,W4,W2]`.

## Validation

Run the focused publication tests:

```powershell
python -m pytest -q tests/test_predict_inverse.py tests/test_export_prediction_dataset.py tests/test_verify_export_row_fem.py
```

Additional smoke checks:

```powershell
python -c "import rf_trap_forward; print(rf_trap_forward.__name__)"
python -m rf_trap_forward.predict_inverse --minima "-1.596,3.869;-1.836,-3.034;4.218,-1.076" --units mm
python docs/figures/generate_article_figures.py
```
