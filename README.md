# PurIST TNBC Basal Workflow

This repository contains a complete workflow for analyzing **Triple-Negative Breast Cancer (TNBC) Basal-like samples** using TCGA data. It includes preprocessing, tumor purity estimation, DESeq2 differential expression analysis, PCA, clustering, and survival analyses.

---

## Table of Contents

- [Overview](#overview)
- [Data](#data)
- [Workflow](#workflow)
  - [1. Expression Data Preprocessing](#1-expression-data-preprocessing)
  - [2. Tumor Purity Estimation](#2-tumor-purity-estimation)
  - [3. Differential Expression Analysis](#3-differential-expression-analysis)
  - [4. PCA and K-means Clustering](#4-pca-and-k-means-clustering)
  - [5. Volcano Plot](#5-volcano-plot)
  - [6. Survival Analysis](#6-survival-analysis)
- [Figures](#figures)
- [Dependencies](#dependencies)
- [References](#references)

---

## Overview

This workflow processes TNBC basal-like samples from TCGA-BRCA and performs:

- Tumor purity estimation using **ESTIMATE**.
- Creation of **Purity Groups** (High vs Low).
- DESeq2 differential expression analysis between purity groups.
- PCA visualization with K-means clustering.
- Volcano plot of differential genes.
- Kaplan-Meier survival analysis by tumor purity and age.

---

## Data

- **Expression data:** `TCGA-BRCA.star_fpkm-uq.tsv.gz`
- **GTF annotation:** `gencode.v22.annotation.gtf.gz`
- **Clinical / PAM50 data:** Retrieved using `TCGAbiolinks::TCGAquery_subtype()`
- **PurIST-ready GCT files:** `tnbc_expr.gct`, `tnbc_filtered.gct`

---

## Workflow

### 1. Expression Data Preprocessing

- Load and clean TCGA expression data.
- Collapse duplicate Ensembl IDs.
- Map Ensembl IDs → HGNC symbols.
- Filter for TNBC Basal-like patients.
- Generate PurIST-ready GCT file: [`tnbc_expr.gct`](tnbc_expr.gct)

---

### 2. Tumor Purity Estimation

- Filter genes with `filterCommonGenes()`.
- Estimate ESTIMATE scores with `estimateScore()`.
- Create **HighPurity vs LowPurity** groups (median split).

---

### 3. Differential Expression Analysis

- Build DESeq2 dataset using purity groups.
- Run DESeq2 and extract results.
- Save DESeq2 results:

[`TNBC_DESeq2_High_vs_LowPurity.csv`](TNBC_DESeq2_High_vs_LowPurity.csv)

---

### 4. PCA and K-means Clustering

- Variance-stabilizing transformation (VST) applied.
- PCA plot colored by **Purity_Group** and K-means clusters.
- Example figures:

[`TNBC_PCA_High_vs_LowPurity.png`](TNBC_PCA_High_vs_LowPurity.png)  
[`TNBC_PCA_Purity_vs_Kmeans.png`](TNBC_PCA_Purity_vs_Kmeans.png)  
[`TNBC_PCA_Kmeans_Purity.png`](TNBC_PCA_Kmeans_Purity.png)

- Adjusted Rand Index (ARI) calculated to compare clusters with purity groups.

---

### 5. Volcano Plot

- Visualize DE genes between High and Low Purity groups.
- Figure:

[`TNBC_Volcano_High_vs_LowPurity.png`](TNBC_Volcano_High_vs_LowPurity.png)

---

### 6. Survival Analysis

- Kaplan-Meier survival curves by:
  - Tumor purity
  - Age at diagnosis
  - Combined purity and age groups
- Cox proportional hazards modeling for HR estimation.
- Example KM plots:

- [`TNBC_Survival_Purity.png`](figures/TNBC_Survival_Purity.png)  
- [`TNBC_Survival_Age.png`](figures/TNBC_Survival_Age.png)  
- [`TNBC_Survival_Combined.png`](figures/TNBC_Survival_Combined.png)

---

## Figures

| Figure | Description |
|--------|-------------|
| PCA High vs Low Purity | [`TNBC_PCA_High_vs_LowPurity.png`](TNBC_PCA_High_vs_LowPurity.png) |
| PCA Purity vs K-means | [`TNBC_PCA_Purity_vs_Kmeans.png`](TNBC_PCA_Purity_vs_Kmeans.png) |
| Volcano Plot | [`TNBC_Volcano_High_vs_LowPurity.png`](TNBC_Volcano_High_vs_LowPurity.png) |
| Survival by Purity | [`TNBC_Survival_Purity.png`](figures/TNBC_Survival_Purity.png) |
| Survival by Age | [`TNBC_Survival_Age.png`](figures/TNBC_Survival_Age.png) |
| Survival by Combined Group | [`TNBC_Survival_Combined.png`](figures/TNBC_Survival_Combined.png) |

---

## Dependencies

- R (>= 4.2)
- Bioconductor packages:
  - TCGAbiolinks
  - DESeq2
  - estimate
  - EnhancedVolcano
- CRAN packages:
  - dplyr, ggplot2, tidyr, tibble, matrixStats
  - survminer, survival, caret, mclust, factoextra, cluster

---

## References

1. Yoshihara K. *et al.*, **Inferring tumor purity and stromal/immune cell admixture from expression data**, Nat Commun, 2013.  
2. TCGA Biolinks: [https://bioconductor.org/packages/release/bioc/html/TCGAbiolinks.html](https://bioconductor.org/packages/release/bioc/html/TCGAbiolinks.html)  

---

*This workflow was developed by Shirsa Udgata for TNBC basal-like analysis with TCGA data and PurIST pipeline integration.*
