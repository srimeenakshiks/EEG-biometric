# EEG User Identification During Gameplay Using a Transformer-Based Biometric Framework

## Overview

This repository contains the implementation, trained checkpoints, evaluation scripts, experimental logs, and reproducibility resources accompanying our work on EEG-based user identification during gameplay.

The proposed framework employs a Transformer-based architecture operating on tokenized EEG representations extracted from gameplay recordings. The system is evaluated using a 5-fold cross-validation protocol and supports both identification and verification tasks through classification and biometric similarity analysis.

---

## Dataset

This work uses the publicly available MOBI (Mobile Brain/Body Imaging) dataset.

Dataset source:

https://ieee-dataport.org/documents/multi-modal-mobile-brain-body-imaging-mobi-dataset-assaying-neural-and-head-movement

Users should obtain the dataset directly from the official source and comply with all licensing and usage requirements specified by the dataset providers.

The training notebook included in this repository contains the preprocessing and training pipeline used in this work.

---

## Repository Structure

```text
EEG-Gameplay-Biometrics/
│
├── README.md
├── LICENSE
├── requirements.txt
│
├── Training/
│   └── train_notebook.ipynb
│
├── Evaluation/
│   ├── evaluation_code.ipynb
│   └── checkpoint_reproducibility_results.json
│
├── Checkpoints/
│   ├── kfold_fold_1_best_eer.pt
│   ├── kfold_fold_1_best_acc.pt
│   ├── kfold_fold_1_best_crr.pt
│   ├── kfold_fold_1_history.json
│   ├── ...
│   └── kfold_fold_5_history.json
│
├── Results/
│   ├── fold_1_training_log.log
│   ├── fold_2_training_log.log
│   ├── fold_3_training_log.log
│   ├── fold_4_training_log.log
│   ├── fold_5_training_log.log
│   └── checkpoint_reproducibility_results.json
│
└── Figures/
    ├── first_patch_heatmap.png
    ├── patch_boundaries.png
    ├── pca_projection.png
    ├── psd.png
    ├── raw_vs_bandpass.png
    ├── kfold_best_fold_4_similarity.png
    ├── kfold_best_fold_4_roc.png
    ├── kfold_best_fold_4_tsne.png
    └── kfold_best_fold_4_heatmap.png
```

---

## Model Architecture

The proposed framework consists of:

* EEG tokenization
* Linear token embedding layer
* Positional encoding
* Transformer encoder
* Attention-based pooling
* Layer normalization
* Classification head
* ArcFace-assisted metric learning objective

The model jointly learns discriminative subject representations for identification and biometric verification.

---

## Training Configuration

| Parameter           | Value                   |
| ------------------- | ----------------------- |
| Optimizer           | AdamW                   |
| Learning Rate       | 1e-4                    |
| Weight Decay        | 1e-2                    |
| Batch Size          | 16                      |
| Transformer Layers  | 3                       |
| Attention Heads     | 6                       |
| Embedding Dimension | 192                     |
| Dropout             | 0.3                     |
| Warmup Epochs       | 10                      |
| Total Epochs        | 150                     |
| Cross Validation    | 5-Fold Stratified       |
| Loss Function       | Cross Entropy + ArcFace |

---

## Evaluation Metrics

The following metrics are used to evaluate model performance.

### Validation Accuracy

Window-level identification accuracy on the validation fold.

### Correct Recognition Rate (CRR)

Subject-level recognition rate computed using a 60% correctness threshold.

### Equal Error Rate (EER)

Biometric verification metric computed using cosine similarity between subject templates and validation embeddings.

Templates are constructed from the training partition of each fold and evaluated on the corresponding validation partition.

---

## Training

To train the model from scratch:

1. Download the MOBI dataset.
2. Configure the dataset path in the training notebook.
3. Open:

```text
Training/train_notebook.ipynb
```

4. Execute all notebook cells.

The notebook performs:

* EEG preprocessing
* Token generation
* 5-fold cross-validation
* Model training
* Checkpoint saving
* Metric logging

---

## Evaluation and Reproducibility

To evaluate the released checkpoints and reproduce the reported results:

1. Open:

```text
Evaluation/evaluation_code.ipynb
```

2. Execute all notebook cells.

The notebook:

* Loads the released checkpoints
* Reconstructs fold partitions
* Computes Accuracy, CRR, and EER
* Generates evaluation figures
* Produces fold-wise summary statistics

---

## Released Checkpoints

For each fold, three checkpoint types are provided.

### Best EER

```text
kfold_fold_X_best_eer.pt
```

Checkpoint achieving the lowest Equal Error Rate.

### Best Accuracy

```text
kfold_fold_X_best_acc.pt
```

Checkpoint achieving the highest validation accuracy.

### Best CRR

```text
kfold_fold_X_best_crr.pt
```

Checkpoint achieving the highest subject-level Correct Recognition Rate.

---

## Training Histories and Logs

Training histories for each fold are provided as:

```text
kfold_fold_X_history.json
```

These files contain:

* Training loss
* Validation loss
* Training accuracy
* Validation accuracy
* Validation CRR
* Validation EER

Training logs generated during experimentation are available in the `Results` directory.

The file `checkpoint_reproducibility_results.json` contains the fold-wise metrics obtained by re-evaluating the released checkpoints.

---

## Figures

The `Figures` directory contains visualizations used for analysis and evaluation.

### Preprocessing and Feature Visualization

* `raw_vs_bandpass.png`
* `psd.png`
* `patch_boundaries.png`
* `first_patch_heatmap.png`
* `pca_projection.png`

These figures illustrate various stages of EEG preprocessing, token generation, and feature representation.

### Best-Fold Evaluation Visualizations

The following figures were generated using the checkpoint achieving the lowest EER:

* `kfold_best_fold_4_similarity.png`
* `kfold_best_fold_4_roc.png`
* `kfold_best_fold_4_tsne.png`
* `kfold_best_fold_4_heatmap.png`

These figures correspond to biometric verification and embedding-space analysis of the best-performing fold.

---

## Reproducibility Statement

The released checkpoints correspond to the models used for the reported experimental results.

The evaluation notebook can be used to verify checkpoint performance directly from the released model files.

Retraining the model may produce slightly different fold-wise metrics due to:

* Random initialization
* Data shuffling
* GPU non-determinism
* Numerical precision differences

Such variations are expected in deep learning workflows and do not affect the validity of the released checkpoints.

---

## Requirements

Install dependencies using:

```bash
pip install -r requirements.txt
```

---

## License

This project is released under the MIT License.

See the LICENSE file for details.

---

## Citation

Citation information will be added following publication.
