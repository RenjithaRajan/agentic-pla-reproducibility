# Metrics

Evaluation metrics, statistical tests, and stratified analyses reported in the manuscript. All values are produced by the notebook `notebooks/Agentic_PLA_reviewer_revision.ipynb`.

## Conventions

- **pKd** is the experimental binding affinity scale. **z-score** is the same quantity standardised with training-set mean and SD.
- **Error sign:** error = predicted − experimental. Positive means over-prediction.
- **Ensemble:** the proposed model and baselines are 5-seed ensembles. "Per-seed" values are the individual runs.
- **Confidence intervals** are 95% bootstrap intervals, in square brackets.
- **Test set:** PDBbind held-out test (n = 1005). **CASF-2016** (n = 107) is an external set. **Val** (n = 1005) is the validation set.

## Files

| File | Used for | Contents |
|---|---|---|
| `metrics_all.csv` | Main results table | Every model on val, test, and CASF, in pKd and z-score scales |
| `comparison_table.csv` / `.tex` | Comparison with other methods | Our runs and reported literature values, with a flag for whether each comparison is like-for-like |
| `stats_val.csv`, `stats_test.csv`, `stats_casf.csv` | Statistical comparison | Proposed model vs each variant, per dataset |
| `stats_val_uniform.csv`, `stats_test_uniform.csv` | Attention-weight ablation | Same as above, with uniform weights (1/3 each) in place of learned weights |
| `stratified_test.csv`, `stratified_casf.csv` | Seen vs unseen analysis | RMSE by protein and ligand seen/unseen in training, with 95% CIs |
| `stratified_test_uniform.csv` | Attention-weight ablation by stratum | Stratified RMSE with uniform weights |
| `truncation_frequency.csv` | Sequence-length limits | Percentage of samples exceeding the limits used (1024 / 256 / 128) and the legacy limits (512 / 128) |
| `truncation_effect.csv` | Sequence-length limits | Error for truncated vs non-truncated samples, with a Mann–Whitney U test |
| `error_by_pkd_bin.csv` | Error by affinity range | MAE, RMSE, and mean signed error in pKd bins |
| `learning_curve_seed42.csv` | Training behaviour | Training and validation RMSE per epoch (z-scored), seed 42 |

## Column definitions

**`metrics_all.csv`**
- `scale`: `pKd` or `z-score`
- `dataset`: `val`, `test`, or `casf`
- `model`: model name
- `MAE`, `MSE`, `RMSE`: error metrics
- `Pearson`, `R2`: correlation and coefficient of determination
- `SD`: standard deviation of residuals after a linear fit of experimental on predicted values (CASF convention)
- `CI`: concordance index

**`comparison_table.csv`**
- `method`: method name
- `train_data`, `test_set`, `n_test`: data used for training and testing
- `label`, `split`: label type and split protocol
- `comparable`: whether the comparison uses the same pipeline and split. Literature values marked `no` should not be compared directly.
- `RMSE`, `Pearson`, `MAE`, `SD`: metrics as above
- `source`: `this work` or the literature reference

**`stats_*.csv`**
- `variant`: model being compared with the proposed model
- `members`: number of ensemble members
- `RMSE per-seed (mean±SD)`, `Pearson per-seed (mean±SD)`: average and spread across individual seeds
- `RMSE ensemble`, `MAE ensemble`, `Pearson ensemble`: ensemble metrics with 95% CIs
- `R2 ensemble`: ensemble coefficient of determination
- `dRMSE vs main`: variant RMSE − proposed RMSE, with 95% CI. **Negative values mean the variant has lower error than the proposed model.**
- `P(main not better)`: probability from a paired test that the proposed model is not better than the variant. Values near 0 mean the proposed model is clearly better.

**`stratified_*.csv`**
- `stratum`: subgroup (for example, "protein seen in train")
- `n`, `% of set`: number and share of samples
- `RMSE ...` columns: RMSE for each model with 95% CI
- `Pearson main`: correlation for the proposed model
- `RMSE gain vs no-retrieval`: no-retrieval RMSE − proposed RMSE, with 95% CI. **Positive values mean the proposed model is better.**

**`stratified_test_uniform.csv`**
- `label`: subgroup
- `n`: number of samples
- `No retrieval`, `kNN K=3`, `Proposed`: RMSE with 95% CI. "Proposed" here uses uniform attention weights.

**`truncation_frequency.csv`**
- `partition`: `train`, `val`, `test`, or `casf`
- `n`: number of samples
- `% protein > 1024 (used)`, `% SMILES > 256 tok (used)`, `% pocket > 128`: share exceeding the limits used in the model
- `% protein > 512 (legacy)`, `% SMILES > 128 tok (legacy)`: share exceeding the earlier limits

**`truncation_effect.csv`**
- `dataset`, `criterion`: dataset and truncation rule
- `group`: `truncated` or `not truncated`
- `n`, `RMSE`, `MAE`, `mean signed err`: metrics for that group
- `MWU p`: Mann–Whitney U p-value comparing absolute error of truncated and non-truncated samples

**`error_by_pkd_bin.csv`**
- `range`: experimental pKd range
- `n`, `MAE`, `RMSE`, `mean_e`: number of samples and error metrics in that range. `mean_e` is the mean signed error.

**`learning_curve_seed42.csv`**
- `epoch`, `train_rmse`, `val_rmse`: RMSE on z-scored pKd for each epoch
