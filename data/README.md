# Data

## Files
- `processed_pdbbind.csv`: PDBbind v2020 complexes with Kd labels, after the preprocessing described in the paper. Training, validation, and test rows are marked in the `split` column.
- `processed_casf2016.csv`: CASF-2016 benchmark complexes (external test set).

## Source
- PDBbind v2020: http://www.pdbbind.org.cn (requires registration; subject to the PDBbind license)
- CASF-2016: http://www.pdbbind.org.cn (same source; subject to its terms)

## Processing
The CSVs were produced with the preprocessing script `preprocess_pdbbind.py` (Kd-only, `contact_unique` chain policy, random split with seed 42). Replace this line with the final version of the script name if it changes.

## Columns
Check the header row of your CSV and keep this list in sync with it. The notebook uses these columns:

| Column | Meaning |
|---|---|
| `pdb_id` | PDB identifier of the complex |
| `split` | `train`, `val`, or `test` (PDBbind rows) |
| `binding_affinity` | Experimental pKd |
| `protein_seq` | Protein amino-acid sequence |
| `ligand_smiles` | Ligand SMILES string |
| `pocket_residues` | Residues within 5 Å of the ligand |
| `pocket_composition` | JSON list of pocket amino-acid composition values |

## Licensing
These files are derived from PDBbind and CASF-2016. Check the original license before redistributing them. If redistribution is not allowed, delete the CSVs from this folder and rebuild them from the original download with the preprocessing script.
