# Feasibility Mapping of Latent Representations for Electrochemical Energy Materials

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)  
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-red.svg)](https://pytorch.org/)  
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5.2-yellow.svg)](https://scikit-learn.org/stable/)  
[![UMAP](https://img.shields.io/badge/UMAP-learn-orange.svg)](https://umap-learn.readthedocs.io/)  
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green.svg)](LICENSE)

---

This repository accompanies the manuscript:

**“Feasibility Mapping of Latent Representations Reveals Transferability Limits in Machine-Learning Models for Electrochemical Energy Materials”**  
(submitted to *ACS Energy Letters*)

It provides a fully reproducible pipeline for training multitask machine-learning models on heterogeneous electrochemical materials datasets and for interpreting the learned latent representations as maps of **data-supported feasibility**, rather than purely predictive intermediates.

---

## 📌 Scope and Philosophy

This project is built around the following principles:

- No new machine-learning architecture is proposed
- No domain labels are used during model training
- Model evaluation extends beyond accuracy to assess feasibility, extrapolation risk, and transferability
- Latent representations are treated as objects of scientific interpretation, not black boxes

The framework addresses the question:

> Which regions of predicted electrochemical performance are genuinely supported by data, and which arise from extrapolation across heterogeneous material domains?

---

## 🧠 Conceptual Overview

A multitask artificial neural network (ANN) is trained jointly on multiple electrochemical targets using heterogeneous datasets spanning:

- Experimentally measured porous carbons
- Experimentally measured metal–organic frameworks (MOFs)
- Simulated MOFs

The shared latent representation learned by the encoder is subsequently analyzed post hoc to:

- Map smooth, data-supported electrochemical property manifolds
- Quantify domain divergence and overlap
- Identify sparsely supported regions associated with elevated screening risk
- Diagnose transferability limits across material classes

---

## 📁 Repository Structure

```
├── Feature contract & preprocessing
│   ├── feature selection and leakage control
│   ├── global scaling and missingness masking
│   └── preprocessing artifacts (saved)
│
├── Multitask ANN training
│   ├── shared encoder with deterministic latent space
│   ├── multiple supervised electrochemical heads
│   └── trained model checkpoint
│
├── Latent feasibility analysis
│   ├── latent extraction (encoder only)
│   ├── PCA and UMAP geometry
│   ├── property-colored embeddings
│   ├── domain divergence (centroid distance, MMD²)
│   ├── latent–target coupling analysis
│   └── property-sorted latent trajectories
│
├── Domain separability diagnostics
│   ├── linear probe with stratified cross-validation
│   ├── random-guess baseline comparison
│   └── latent dimension importance ranking
│
├── models_disentangled/
│   ├── feature_cols.pkl
│   ├── feature_scaler.pkl
│   ├── target_cols.pkl
│   ├── models/
│   │   └── multitask_ann_PAPER_A.pt
│   └── diagnostics/
│       └── paperA_latent_transfer/
│
├── README.md
└── LICENSE
```

---

## 🧪 Datasets

The pipeline expects the following Excel files in the project root:

- `carbon_dataset_experimental.xlsx`
- `mof_dataset_experimental.xlsx`
- `mof_dataset_simulation.xlsx`

These correspond to experimentally measured porous carbon electrodes, experimentally measured MOF electrodes, and simulated MOF electrodes (ionic liquid EMIMBF₄), respectively.

Datasets are concatenated internally with a `domain` label used **only for analysis and visualization**, never during model training.

---

## ⚙️ Installation

**Recommended environment**

- Python ≥ 3.9

**Required packages**

```
numpy
pandas
scikit-learn
torch
umap-learn
plotly
joblib
openpyxl
kaleido
```

Install via:

```bash
pip install -r requirements.txt
```

---

## 🚀 Usage Workflow

1. **Feature contract and scaling**  
   Defines the allowed input space, excludes targets and domain labels, fits a global scaler, and saves preprocessing artifacts.

2. **Multitask ANN training**  
   Trains a shared encoder with multiple electrochemical targets using masked losses. The trained model is saved and remains fixed for all subsequent analyses.

3. **Latent feasibility analysis**  
   Extracts frozen latent representations and performs geometric, statistical, and property-coupling analyses to identify feasible and extrapolative regimes.

4. **Domain separability diagnostics**  
   Uses a post hoc linear probe to quantify emergent domain encoding, compare against a random-guess baseline, and identify domain-informative latent dimensions.

---

## 📊 Interpretation Guide

- Smooth latent gradients indicate data-supported feasibility regimes
- Sparse or isolated latent regions indicate elevated extrapolation risk
- Strong domain divergence signals limited cross-domain transferability
- Concentrated domain importance across latent dimensions indicates partial disentanglement rather than representational collapse

---

## 🔁 Reproducibility

- Global random seeds fixed (NumPy, PyTorch)
- Preprocessing artifacts persisted and reused
- No retraining during analysis stages
- All figures exported as publication-quality PNGs and interactive HTML

---

## 📜 License

Creative Commons Attribution Share Alike 4.0 International
Permits almost any use subject to providing credit and license notice. Frequently used for media assets and educational materials. The most common license for Open Access scientific publications. Not recommended for software.

---

## 📬 Author

For questions or clarifications related to the methodology or scripts, please contact the corresponding author listed in the manuscript.

Henry Reynolds Nana Benyin Enninful  
Email: hrnbenninful@gmail.com  
GitHub: https://github.com/HRNBEnninful  
LinkedIn: https://www.linkedin.com/in/henryrnbenninful/

