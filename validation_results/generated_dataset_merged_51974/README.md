# Final Merged ML Dataset

This directory contains the final merged clean ML dataset used for inverse-model training and validation.

- Dataset file: `synthetic_clean_ml.csv`
- Rows: 51,974
- Columns: 27
- Units: metres unless explicitly stated otherwise

## Coordinate Convention

Raw/Wolfram trap order:

- W1: upper-right
- W2: lower-right
- W3: upper-left
- W4: lower-left

FEM trap order:

- F1: upper-left
- F2: upper-right
- F3: lower-left
- F4: lower-right

Canonical transform:

```text
FEM = -[W3, W1, W4, W2]
```

This dataset directory is intended to contain only the final clean merged ML dataset and this README. It should not contain personal names, internal labels, local absolute paths, debug data, probe data, temporary generated datasets, or trained model artifacts.
