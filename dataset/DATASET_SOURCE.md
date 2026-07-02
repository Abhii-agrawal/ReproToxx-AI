# Dataset Documentation

## Overview

This project uses the **DARTQSAR** reproductive toxicity dataset for model development and an independent external dataset for evaluating model generalization.

---

# Training Dataset

**Dataset Name:** DARTQSAR

**Source Paper:**

Lee et al.

Prediction of reproductive and developmental toxicity using an attention and gate augmented graph convolutional network.

Scientific Reports (2025)

**DOI:**
https://doi.org/10.1038/s41598-025-02590-y

---

## Dataset Statistics

| Property | Value |
|----------|-------|
| Original Compounds | 4,514 |
| Final Compounds After Preprocessing | 4,503 |
| Toxic Compounds | 2,463 |
| Non-toxic Compounds | 2,040 |
| Total Features | 2,068 |

---

# External Validation Dataset

An independent external validation dataset was used to evaluate the model's ability to generalize to unseen compounds.

## External Dataset Statistics

| Property | Value |
|----------|-------|
| Original External Compounds | 2,154 |
| Overlapping Compounds Removed | 739 |
| Final External Validation Set | 1,414 |

The overlapping compounds between the training and external datasets were removed before evaluation to prevent **data leakage** and ensure an unbiased assessment of model performance.

---

# Data Preprocessing

The following preprocessing steps were performed before model training:

- Removed duplicate molecules.
- Removed invalid SMILES.
- Standardized molecular structures.
- Generated Morgan Fingerprints (ECFP4).
- Calculated RDKit molecular descriptors.
- Removed overlapping compounds from the external validation dataset.
- Performed stratified train-test split.
- Evaluated the final model using Scaffold Split, 5-Fold Cross Validation, and External Validation.

---

# Molecular Representation

Each molecule was represented using:

- **2048-bit Morgan Fingerprints (ECFP4)**
- **20 RDKit Molecular Descriptors**

Total Features:

**2068**

---

# Model Validation Strategy

The developed models were evaluated using multiple validation strategies:

- Stratified 80:20 Train-Test Split
- 5-Fold Cross Validation
- Scaffold Split Validation
- Independent External Validation

This multi-level validation provides a comprehensive assessment of both model performance and generalization capability.

---

# Citation

If you use this dataset in your research, please cite:

Lee et al.

Prediction of reproductive and developmental toxicity using an attention and gate augmented graph convolutional network.

Scientific Reports (2025)

DOI:
https://doi.org/10.1038/s41598-025-02590-y

---

# Disclaimer

The datasets used in this repository are derived from publicly available scientific publications. This repository provides the trained machine learning models and documentation for educational and research purposes. Users should refer to the original publications for access to the complete datasets and licensing information.