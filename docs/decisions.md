# Project Decisions Log

This document records the decisions made throughout the project.

## Project Overview

This is an introductory project in cheminformatics and a Python training exercise. I will try not to use AI to write large chunks of code for me — instead, I want to use it as a mentor and helper.

The goal is to predict solubility based on the ESOL dataset. The plan is:

```
SMILES (ESOL dataset) -> clean/etc -> extract descriptors with RDKit -> train -> compare results with the experimental values in the ESOL dataset
```

## Decisions

### 1. Dataset source

I chose the ESOL dataset from the DeepChem repository. This dataset is widely used in solubility prediction projects, comes from a well-respected researcher, and was measured experimentally.

### 2. Column selection

The dataset has many columns, and using most of them would cause data leakage. Because of this, I'm going to use just two columns: the SMILES column, for obvious reasons, and the measured solubility column, to use in the machine learning part.

It isn't a large dataset, but that's fine — if I used a huge dataset from the start, every part of the notebook would take minutes to run, and right now that would slow down this training/introduction project.

### 3. Duplicates are found with canonical SMILES

The same molecule can be written as different SMILES strings, so comparing the raw strings can miss duplicates. I convert each SMILES to its canonical form with RDKit and look for duplicates there. This found 11 duplicated molecules. I keep only the first row of each one. Only one pair had a real difference in solubility (about 1 log unit), so keeping the first row is a safe choice.

### 4. Invalid SMILES check

Before using RDKit on the molecules, I check that every SMILES can be parsed. In this dataset, all of them are valid, but the check is cheap and protects the next steps.

### 5. Very low solubility values are kept

The most insoluble molecules (down to -11.6) look like outliers in the boxplot, but I checked them and they are real molecules with very low solubility. I keep them in the dataset. Later, I need to be careful with how the train and test sets are split, and I expect the model to be less accurate for highly soluble molecules, because there are fewer of them.

### 6. The clean dataset is saved once

The cleaning steps are done only in the first notebook. It saves the result to `data/processed/solubility_clean.csv` (canonical SMILES and measured solubility, 1117 rows), so the next notebooks can load it without repeating the cleaning. This folder is ignored by git, so the file must be recreated by running the first notebook.
