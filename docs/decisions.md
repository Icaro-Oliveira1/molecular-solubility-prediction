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
