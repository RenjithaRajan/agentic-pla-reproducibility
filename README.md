# Agentic Retrieval-Augmented Framework for Protein–Ligand Binding Affinity Prediction

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RenjithaRajan/agentic-pla-reproducibility/blob/main/notebooks/Agentic_PLA_reviewer_revision.ipynb)

Reproducible implementation, processed data, and evaluation code for an agentic retrieval-augmented
framework for protein–ligand binding affinity (pKd) prediction. The model uses frozen ProtBERT and
MolT5 embeddings, FAISS top-K retrieval, agentic evidence weighting, pocket-aware attention fusion,
and MC-dropout uncertainty.

## Repository contents

| Folder | What is inside |
|---|---|
| `notebooks/` | Main end-to-end pipeline: embeddings, retrieval, training, evaluation, baselines, statistics |
| `data/` | Processed PDBbind v2020 (Kd subset) and CASF-2016 tables. See `data/README.md` |
| `results/` | Metrics, predictions, and figures reported in the paper. See `results/README.md` |
| `models/` | Small trained weights (large weights are archived on Zenodo, see below) |

## Quick start (Google Colab, no installation needed)

1. Click the **Open In Colab** badge above.
2. Select **Runtime → Change runtime type → T4 GPU**.
3. Select **Runtime → Run all**.

The first cell clones this repository into Colab. The first run downloads the pretrained models
`Rostlab/prot_bert` (ProtBERT) and `laituan245/molt5-base` (MolT5) from Hugging Face. Embeddings are
cached in `cache/`, so later runs are faster.

## Quick start (local machine with a GPU)

```bash
git clone https://github.com/RenjithaRajan/agentic-pla-reproducibility.git
cd agentic-pla-reproducibility
pip install torch==2.11.0 --index-url https://download.pytorch.org/whl/cu130   # match your CUDA version
pip install -r requirements.txt
jupyter notebook notebooks/Agentic_PLA_reviewer_revision.ipynb
```

The notebook expects to be run from the repository root so that the relative paths `data/`, `cache/`,
and `results/` resolve correctly.

## Reproducing the paper

- Software versions: Python 3.12, torch 2.11.0, transformers 5.18.0, faiss 1.15.1, scikit-learn 1.6.1,
  numpy 2.1.3, pandas 2.2.3, scipy 1.16.3, matplotlib 3.10.0 (see `requirements.txt`).
- Hardware: NVIDIA T4 GPU (Google Colab).
- Random seed: 42. Ensemble seeds are derived from it inside the notebook.
- Data split: Kd-only, `contact_unique` chain policy, random split with seed 42.

## Data

The processed tables are derived from PDBbind v2020 and CASF-2016. The original data are subject to
their providers' licenses. See `data/README.md` for provenance and instructions.

## Archived release

Version 1.0.0 (paper submission version) is archived on Zenodo: `https://doi.org/10.5281/zenodo.XXXXXXX`
*(replace XXXXXXX with your DOI after step 11 of the setup guide)*.

## Citation

If you use this code, please cite the paper. Citation metadata is in [`CITATION.cff`](CITATION.cff).

## License

Code: MIT License (see [`LICENSE`](LICENSE)).
Data: subject to the original PDBbind and CASF-2016 terms.

## Contact

Renjitha Rajan: `<your email>`
