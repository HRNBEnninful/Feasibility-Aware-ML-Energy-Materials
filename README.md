# Feasibility Mapping of Latent Representations for Electrochemical Energy Materials

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)  
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-red.svg)](https://pytorch.org/)  
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5.2-yellow.svg)](https://scikit-learn.org/stable/)
[![Plotly & Dash](https://img.shields.io/badge/Plotly-Dash-orange.svg)](https://plotly.com/dash/)  
[![UMAP](https://img.shields.io/badge/UMAP-learn-orange.svg)](https://umap-learn.readthedocs.io/)  
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC%20BY--SA%204.0-green.svg)](LICENSE)
[![DOI](https://zenodo.org/badge/18486311.svg)](https://doi.org/10.5281/zenodo.18486311)

---
This repository accompanies the manuscript:

**“Feasibility Mapping in Latent Space Reveals Transferability Limits in Cross-Domain Machine Learning for Electrochemical Materials”**
(submitted to *ACS Applied Energy Materials*)

It provides a fully reproducible, three-stage deterministic pipeline for training multitask neural encoders on heterogeneous electrochemical materials datasets and interpreting the learned latent space as a geometry-defined feasibility manifold governing cross-domain transfer.

---

## 📌 Scope and Philosophy

This project is built around the following principles:

- No new machine-learning architecture is proposed
- Domain labels are never used as model inputs
- All preprocessing is train-only to prevent leakage
- Model evaluation extends beyond predictive accuracy to assess feasibility, extrapolation exposure, and transferability limits
- Latent representations are treated as objects of scientific interpretation, not black-box intermediates

The framework addresses the question:

> Which regions of predicted electrochemical performance are structurally supported by training data, and which correspond to geometric extrapolation across heterogeneous domains?

---

## 🧠 Conceptual Overview

A multitask artificial neural network (ANN) is trained jointly on multiple electrochemical targets using heterogeneous datasets spanning:

- Experimentally measured porous carbons
- Experimentally measured metal–organic frameworks (MOFs)
- Simulated MOFs

The shared encoder produces a frozen latent representation, which is subsequently analyzed to:

- Map smooth, data-supported electrochemical property manifolds
- Quantify domain divergence (centroid distance, MMD²)
- Identify sparse support regions (distance-to-support metrics)
- Define geometry-based feasibility frontiers (r*)
- Detect negative transfer zones inside nominal support

---

## 📁 Repository Structure

├── Stage 0 — Feature/Target Contract
│   ├── builds frozen feature list (feature_cols.pkl)
│   ├── defines canonical target names (target_cols.pkl)
│   └── generates domain label coverage summary
│
├── Stage 1 — Multitask ANN Training
│   ├── domain-stratified train/validation split
│   ├── train-only imputer and scaler persistence
│   ├── optional missingness mask concatenation
│   ├── masked multitask loss (sparse supervision)
│   ├── predictive validation metrics (RMSE, MAE, R²)
│   └── frozen checkpoint (encoder + heads)
│
├── Stage 2 — PAPER_MASTER_PIPELINE_v2.py
│   ├── loads frozen encoder
│   ├── extracts latent space (Z)
│   ├── PCA / UMAP visualization
│   ├── domain divergence (centroid, MMD²)
│   ├── support imbalance diagnostics (kNN distance)
│   ├── feasibility frontier (r*) computation
│   ├── coverage ratio analysis
│   ├── negative transfer zone detection
│   ├── latent–target coupling heatmaps
│   └── property-sorted latent trajectories
│
├── models_disentangled/
│   ├── feature_cols.pkl
│   ├── target_cols.pkl
│   ├── feature_imputer_train_only.pkl
│   ├── feature_scaler_train_only.pkl
│   ├── target_scaler_train_only.pkl
│   ├── models/
│   │   └── multitask_ann_PAPER_A_MASTER.pt
│   ├── final_figures/
│   └── final_tables/
│
├── carbon_dataset_experimental.xlsx
├── mof_dataset_experimental.xlsx
├── mof_dataset_simulation.xlsx
├── PAPER_MASTER_PIPELINE_v2.py
├── STAGE_1_TRAIN_MULTITASK.py
└── README.md

---

## 🧪 Datasets

The pipeline expects the following Excel files in the project root:

- `carbon_dataset_experimental.xlsx`
- `mof_dataset_experimental.xlsx`
- `mof_dataset_simulation.xlsx`

These correspond to experimentally measured porous carbon electrodes, experimentally measured MOF electrodes, and simulated MOF electrodes (ionic liquid EMIMBF₄), respectively.

Datasets are concatenated internally with a domain label used **only for analysis, visualization, and diagnostic evaluation — never for training**.

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

**Stage 0 — Build Feature & Target Contract**

- Defines canonical feature columns
- Harmonizes target aliases
- Saves frozen feature and target artifacts

**Stage 1 — Train Multitask Encoder**

- Domain-stratified split
- Train-only imputation and scaling
- Masked multitask regression
- Saves deterministic checkpoint
- Outputs validation RMSE, MAE, and R²

**Stage 2 — Latent Feasibility Mapping**

- Loads frozen encoder
- Extracts latent space once (no retraining)
- Computes geometry-based diagnostics
- Generates all manuscript figures
- Exports publication-ready PNG + HTML

---

## 📊 Interpretation Guide

- Smooth latent gradients indicate structured electrochemical response regimes
- Large kNN distance-to-support indicates geometric extrapolation exposure
- Feasibility frontier (r*) defines intrinsic training-supported boundary
- 100% coverage does not imply chemical equivalence or guaranteed transfer accuracy
- Negative transfer zones identify boundary-fragile regions within nominal support

---

## 🔁 Reproducibility

- Global random seeds fixed (NumPy, PyTorch, CUDA)
- Deterministic minibatch ordering
- Train-only preprocessing artifacts persisted
- Encoder frozen before all geometry analyses
- No retraining during Stage 2
- All figures reproducibly generated from frozen checkpoint

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