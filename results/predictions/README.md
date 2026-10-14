# Predictions

Per-complex predictions for the validation, test, and CASF-2016 sets. Each row is one protein–ligand complex. These files let reviewers recompute any metric in `results/metrics/` and check the figures.

## Conventions

- **pKd:** experimental and predicted binding affinity.
- **z-score:** the same values standardised with training-set mean and SD.
- **Error sign:** `signed_error_pKd = pred_pKd − true_pKd`. Positive means over-prediction.
- **Uncertainty:** standard deviation of predictions across the 5 ensemble seeds and 10 MC-dropout passes per seed.
- **Attention weights (`alpha_1`–`alpha_3`):** the weights the model gives to its 3 retrieved neighbours. They sum to 1.

## Files

| File | Model | Rows |
|---|---|---|
| `predictions_val.csv` | Proposed model (K = 3, learned weights) | 1005 |
| `predictions_test.csv` | Proposed model (K = 3, learned weights) | 1005 |
| `predictions_casf.csv` | Proposed model (K = 3, learned weights) | 107 |
| `predictions_val_uniform.csv` | Same model with uniform weights (1/3 each) | 1005 |
| `predictions_test_uniform.csv` | Same model with uniform weights (1/3 each) | 1005 |
| `member_predictions.npz` | Individual ensemble members and baselines | See below |

## Columns

**`predictions_val.csv`, `predictions_test.csv`, `predictions_casf.csv`**
- `pdb_id`: PDB identifier of the complex
- `true_pKd`: experimental pKd
- `pred_pKd`: ensemble-mean predicted pKd
- `uncertainty_pKd`: standard deviation of predictions (pKd scale)
- `true_z`, `pred_z`, `uncertainty_z`: the same three values on the z-score scale
- `alpha_1`, `alpha_2`, `alpha_3`: attention weights for the three retrieved neighbours (sum to 1)
- `abs_error_pKd`: |pred_pKd − true_pKd|
- `signed_error_pKd`: pred_pKd − true_pKd

**`predictions_val_uniform.csv`, `predictions_test_uniform.csv`**
- `pdb_id`, `true_pKd`, `pred_pKd`, `uncertainty_pKd`, `signed_error_pKd`
- No `alpha` columns, because the weights are fixed at 1/3 each.

## `member_predictions.npz`

A NumPy archive of raw predictions. Each key has the form `<model name>|<dataset>`, for example `Agentic K=3 (main)|test`. Load it with:

```python
import numpy as np
z = np.load("results/predictions/member_predictions.npz", allow_pickle=True)
print(z.files)                       # list of available keys
p = z["Agentic K=3 (main)|test"]     # shape (members, n_samples), pKd scale
```

- **Shape:** `(5, n)` for ensemble models (one row per seed). `(1, n)` for the training-free kNN baselines.
- **Datasets:** `val` (n = 1005), `test` (n = 1005), `casf` (n = 107).
- **Models included:** proposed model (K = 3, 5, 7), uniform-weight variant, no-retrieval model, ligand-only model, protein-only model, and kNN (K = 3, 5, 7).
- **Row order:** should match the row order of the corresponding `predictions_*.csv` file. Check this against the `pdb_id` column before relying on it.

Use `member_predictions.npz` to recompute the seed-level results in `results/metrics/stats_*.csv`.
