# Molecular Solubility Prediction

An introductory project in cheminformatics and a Python training exercise. The goal is to predict the aqueous solubility of molecules (the ESOL dataset) from their SMILES, using RDKit descriptors and scikit-learn.

The reasoning behind the main choices is in [`docs/decisions.md`](docs/decisions.md).

## Plan

```
SMILES (ESOL dataset) -> clean -> extract descriptors with RDKit -> train -> compare with the experimental values
```

## Status

- [x] Notebook 01: exploratory analysis and cleaning
- [ ] Notebook 02: feature extraction (RDKit descriptors / fingerprints)
- [ ] Notebook 03: train/test split, models and evaluation

## Setup

Create and activate the conda environment:

```bash
conda env create -f environment.yml
conda activate logs
```

The environment name `logs` comes from "log solubility", the target of the dataset.

## Data

The raw dataset is **not** in the repository (CSV files are ignored by git). Download it into `data/raw/`:

```bash
curl -L -o data/raw/delaney-processed.csv \
  https://raw.githubusercontent.com/deepchem/deepchem/master/datasets/delaney-processed.csv
```

The dataset comes from the DeepChem repository (1128 molecules with experimentally measured solubility).

Then run `notebooks/01_exploratory_analysis.ipynb`. It cleans the data and saves the result to `data/processed/solubility_clean.csv` (canonical SMILES and measured log solubility, 1117 molecules). The next notebooks load this file directly, so the cleaning is not repeated. `data/processed/` is also ignored by git and is recreated by the notebook.

## Project structure

```
data/raw/          original dataset (not versioned)
data/processed/    clean dataset created by notebook 01 (not versioned)
notebooks/         analysis and modeling notebooks
src/               reusable code (empty for now)
results/figures/   generated figures
docs/              project decisions
```

## Cleaning summary (notebook 01)

- No null values and no invalid SMILES.
- 11 duplicated molecules found with canonical SMILES; only the first row of each is kept.
- Very low solubility values are kept: they are real molecules, not outliers to remove.
