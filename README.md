# Development of a novel aggregated deep learning framework for small biological datasets using overlapping subsequences

A hybrid deep learning framework for the classification of short sequences from small biological datasets.

This repository contains the primary Python scripts, training datasets, and pre-computed embeddings associated with the study.

---

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Datasets](#datasets)
- [Embeddings](#embeddings)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Model Architecture](#model-architecture)
- [Citation](#citation)
- [License](#license)

---

## Overview

This framework addresses a core challenge in computational genomics: classifying full-length sequences (e.g., 200 bp) when only a small number of labeled examples are available (e.g., 50–100 sequences per class).

The approach is based on a data augmentation strategy developed by Abbasi-Vineh et al. (2025):[OpenAI] (DOI: 10.1038/s41598-025-12796-9)

### Key Features

- **Overlapping subsequence augmentation**
  - Each 200 bp sequence is padded and decomposed into **240 overlapping 40-nt subsequences**.

- **Hybrid CNN–LSTM–Attention–Residual model**
  - CNN layers extract local motifs.
  - Bidirectional LSTM captures positional dependencies.
  - Attention mechanisms emphasize informative regions.

- **Coverage-aware feature aggregation**
  - Subsequence features are aggregated back to the original sequence using masking and attention-weighted averaging.

- **Dual hybrid loss function**
  - Joint optimization of subsequence-level and sequence-level cross-entropy losses.
  - Weighting factor: **α = 0.7**.

The framework was evaluated on three independent genomic datasets covering divergent organisms, chloroplast genomes, and Shine–Dalgarno motif classification.

---

## Repository Structure

```text
Aggregated-DL/
│
├── Primary_Scripts_for_Aggregated_DL.ipynb
├── Supplementary Data.rar
│
├── datasets/
│   ├── dataset1_divergent_organisms/
│   ├── dataset2_chloroplast_genomes/
│   └── dataset3_SD_motif/
│
├── embeddings/
│   ├── dataset1_embeddings.npy
│   ├── dataset2_embeddings.npy
│   └── dataset3_embeddings.npy
│
├── README.md
└── LICENSE
```

> Update folder names and paths as needed to match your repository.

---

## Datasets

### Dataset 1 — Divergent Organisms

- 6 classes
- 100 sequences total
- Regulatory sequences from evolutionarily divergent organisms

| Class | NCBI Accession |
|---------|---------|
| Archaeon | NZ_CP145900 |
| Bacterium | CP178715 |
| Fungus | NC_018946 |
| Plant (*Arabidopsis thaliana*) | CP002687 |
| Protist | JAPFFF010000003 |
| Virus | PV416404 |

### Dataset 2 — Chloroplast Genomes

- 6 classes
- 50 sequences total

| Organism | Accession |
|----------|----------|
| Arabidopsis thaliana | NC_000932.1 |
| Chlamydomonas reinhardtii | NC_005353.1 |
| Chlorella vulgaris | NC_001865.1 |
| Nicotiana tabacum | MZ707522.1 |
| Porphyridium purpureum | NC_023133 |
| Triticum aestivum | NC_002762.1 |

### Dataset 3 — Shine-Dalgarno Motifs

- 2 classes
- 100 sequences total
- With and without canonical Shine-Dalgarno motifs

---

## Embeddings

Pre-computed CNN embeddings are provided in the `embeddings/` directory.

```python
import numpy as np

embeddings = np.load(
    "embeddings/dataset1_embeddings.npy",
    allow_pickle=True
).item()
```

Embeddings can be used for:

- Similarity analysis
- Clustering
- Transfer learning
- Downstream classification

---

## Requirements

### Python

```text
Python >= 3.10
```

### Packages

```bash
pip install torch numpy pandas scikit-learn matplotlib seaborn scipy biopython
```

Required libraries:

- torch >= 2.0
- numpy
- pandas
- scikit-learn
- matplotlib
- seaborn
- scipy
- biopython

---

## Installation

### Option 1 — Clone Repository

```bash
git clone https://github.com/parkingvarsson/Aggregated-DL.git
cd Aggregated-DL
```

### Option 2 — Google Colab

Open the notebook directly in Google Colab and enable a **T4 GPU** runtime.

---

## Usage

All pipeline steps are implemented in:

```text
Primary_Scripts_for_Aggregated_DL.ipynb
```

### Step 1 — Data Loading

Load FASTA files and assign class labels.

### Step 2 — Augmentation

Generate 240 overlapping 40-nt subsequences from each 200 bp sequence.

### Step 3 — One-Hot Encoding

Encode sequences into 5-channel matrices:

```text
A, T, C, G, N
```

### Step 4 — Model Training

| Parameter | Value |
|------------|--------|
| Epochs | 100 |
| Batch Size | 256 |
| Optimizer | AdamW |
| Scheduler | OneCycleLR |
| α | 0.7 |
| Validation | Stratified 3-fold |

### Step 5 — Subsequence Evaluation

Generate:

- Classification reports
- Confusion matrices
- ROC curves
- PR curves

### Step 6 — Sequence-Level Evaluation

Aggregate subsequence features using attention-weighted averaging.

### Step 7 — Saliency Mapping

Identify informative nucleotide positions using gradient-based saliency maps.

### Step 8 — Save Results

Automatically save:

- Trained model
- Metadata
- Sequence mappings
- Embeddings

---

## Model Architecture

```text
Input (40-nt one-hot, 5 channels)
    ↓
CNN Block 1
    ↓
CNN Block 2
    ↓
CNN Block 3
    ↓
Positional Encoding
    ↓
Bidirectional LSTM
    ↓
LSTM Attention
    ↓
FC1 → FC2 → FC3
    ↓
FC4 → Softmax
```

### Feature Extraction

Embeddings are extracted after:

```text
FC3
```

before the final classification layer.

---

## Citation

If you use this repository in your research, please cite:

```text
[Citation will be added upon publication]
```

---

## License

This project is licensed under the MIT License.

## License

See the `LICENSE` file for details.

## Recommendation

Optimized versions of the current framework, informed by ongoing functional and experimental studies, are currently under development. If you plan to apply this framework to your own datasets, we encourage you to contact the authors to obtain information about the latest updates, improvements, and recommended implementation practices.

This may help ensure that you benefit from the most recent optimizations and performance enhancements that are not yet reflected in the current public release.

## Contact
For technical questions or additional details regarding data or analysis, please contact:
....
