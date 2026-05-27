# Comparative Evaluation of Federated Learning Algorithms in Dirichlet Non-IID Medical Imaging

[![DOI](https://img.shields.io/badge/DOI-10.20885%2Fsnati.v5.i1.44597-blue)](https://doi.org/10.20885/snati.v5.i1.44597)
[![Journal](https://img.shields.io/badge/Journal-SNATI%202026-green)](https://journal.uii.ac.id/jurnalsnati/)
[![Dataset](https://img.shields.io/badge/Dataset-MedMNIST%20v2-orange)](https://medmnist.com)

> **Published in:** *Jurnal Sains, Nalar, dan Aplikasi Teknologi Informasi*, Vol. 5 No. 1 (2026), pp. 32–44
> **Received:** 13 November 2025 | **Revised:** 10 January 2026 | **Accepted:** 15 January 2026 | **Published:** 31 January 2026

---

## Authors

**Michael Angello Qadosy Riyadi**<sup>1*</sup>, **Adinda Mariasti Dewi**<sup>2</sup>, **Zahid Abdullah Nur Mukhlishin**<sup>3</sup>, **Zalsabilah Rezky Amelia Arep**<sup>4</sup>

<sup>1,2</sup> Department of Information Technology, Telkom University, Surabaya Campus, Surabaya, Indonesia
<sup>3,4</sup> Department of Data Science, Telkom University, Surabaya Campus, Surabaya, Indonesia

---

## Abstract

Machine learning has achieved diagnostic performance comparable to clinical experts on medical imaging, yet centralized training paradigms necessitate patient data aggregation, risking violations of privacy regulations such as GDPR and HIPAA. In 2023, 1,853 healthcare data breaches were reported in the United States, compromising over 133 million medical records, rendering raw inter-institutional data exchange increasingly unsustainable. Federated Learning (FL) offers a viable solution by enabling collaborative model training without data transfer. However, prior studies predominantly evaluate single algorithms and often neglect non-IID Dirichlet-distributed conditions and probabilistic calibration metrics like log-loss.

This study rigorously compares **FedAvg**, **FedProx**, **FedSVRG**, and **FedAtt** across three MedMNIST v2 datasets—PneumoniaMNIST (binary), DermaMNIST, and BloodMNIST (multi-class)—using three clients under non-IID Dirichlet partitioning (α = 0.1) over 50 communication rounds. **FedProx demonstrates the most consistent performance and stability**, achieving accuracy of 0.9521 and log-loss of 0.1850 on PneumoniaMNIST; 0.8595 and 0.4066 on BloodMNIST; and 0.5747 and 1.5996 on DermaMNIST.

**Keywords:** Aggregation; healthcare; federated learning; data transfer; privacy-preserving

---

## Research Motivation

Inter-institutional medical data is inherently non-IID: a dermatology oncology center predominantly manages melanoma cases, whereas a general hospital handles a markedly different distribution. Such heterogeneity causes conventional FL algorithms—including FedAvg—to suffer accuracy degradation of up to **25.7%** under extreme non-IID conditions compared to IID settings. Existing literature further suffers from a threefold methodological gap:

1. Evaluation of single algorithms without systematic comparison
2. Neglect of Dirichlet distribution as a standard for non-IID simulation
3. Omission of probabilistic calibration metrics such as log-loss

---

## Datasets

All datasets are sourced from [MedMNIST v2](https://medmnist.com).

| Dataset | Task | Samples | Classes |
|---|---|---|---|
| **PneumoniaMNIST** | Binary | 5,856 | Normal (27.03%), Pneumonia (72.97%) |
| **DermaMNIST** | Multiclass | 10,015 | 7 skin lesion types (akiec, bcc, bkl, df, mel, nv, vasc) |
| **BloodMNIST** | Multiclass | 17,092 | 8 blood cell types (basophil, eosinophil, erythroblast, immature granulocytes, lymphocyte, monocyte, neutrophil, platelet) |

### Non-IID Partitioning via Dirichlet Distribution

Data were partitioned across **3 heterogeneous clients** using a Dirichlet distribution with concentration parameter **α = 0.1** to simulate extreme inter-institutional heterogeneity. Statistical validation confirmed significant distributional differences across clients (Chi-square test, *p* < 0.05; Kolmogorov–Smirnov test on pixel intensity, *p* < 0.05 for all dataset–client pairs).

---

## Methods

### Federated Learning Algorithms

| Algorithm | Core Mechanism |
|---|---|
| **FedAvg** | Weighted averaging of local parameters proportional to dataset size |
| **FedProx** | FedAvg + proximal regularization term (μ) to constrain local-global divergence |
| **FedSVRG** | Gradient variance reduction via control variates (SCAFFOLD framework) |
| **FedAtt** | Cosine similarity-based attention for adaptive client weighting during aggregation |

### Local Model: Convolutional Neural Network (CNN)

Each client trains a shallow CNN comprising two convolutional layers (ReLU activation), one max-pooling layer, dropout (p = 0.25), and two fully connected layers. The output dimension is adjusted for binary (1 unit, BCEWithLogitsLoss) and multiclass (7 or 8 units, CrossEntropyLoss) tasks.

### Experimental Protocol

- **Clients:** 3
- **Communication rounds:** 50
- **Cross-validation:** 5-fold stratified (StratifiedKFold) per client
- **Evaluation metrics:** Accuracy, Precision, Recall, F1-score, ROC-AUC, Log-loss

---

## Results

### PneumoniaMNIST — Final Performance (Round 50)

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | Log-Loss |
|---|---|---|---|---|---|---|
| Local | 0.7642 | 0.5828 | 0.6456 | 0.6068 | 0.7921 | 0.4267 |
| FedAvg | 0.7097 | 0.6998 | 0.9955 | 0.8035 | 0.8367 | 0.6130 |
| **FedProx** | **0.9521** | **0.9439** | **0.9763** | **0.9589** | **0.9852** | **0.1850** |
| FedSVRG | 0.9461 | 0.9360 | 0.9716 | 0.9525 | 0.9772 | 0.2687 |
| FedAtt | 0.9461 | 0.9360 | 0.9716 | 0.9525 | 0.9773 | 0.2687 |

### DermaMNIST — Final Performance (Round 50)

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | Log-Loss |
|---|---|---|---|---|---|---|
| Local | 0.7464 | 0.1066 | 0.1429 | 0.1219 | 0.7556 | 0.7892 |
| FedAvg | 0.5339 | 0.0763 | 0.1429 | 0.0861 | 0.7061 | 1.4642 |
| **FedProx** | **0.5747** | **0.2919** | **0.2652** | **0.2190** | **0.8178** | 1.5996 |
| FedSVRG | 0.5438 | 0.2528 | 0.2700 | 0.2125 | 0.7397 | 2.4578 |
| FedAtt | 0.5439 | 0.2532 | 0.2705 | 0.2133 | 0.7396 | 2.4540 |

> **Note:** All federated algorithms fail to surpass local training on DermaMNIST due to extreme class imbalance (one class dominating >66% of data), where aggregation dilutes minority class signals.

### BloodMNIST — Final Performance (Round 50)

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | Log-Loss |
|---|---|---|---|---|---|---|
| Local | 0.6315 | 0.2114 | 0.2426 | 0.2140 | 0.7194 | 1.0999 |
| FedAvg | 0.1831 | 0.0320 | 0.1439 | 0.0481 | 0.7319 | 1.9868 |
| **FedProx** | **0.8595** | **0.7571** | **0.8619** | **0.7834** | **0.9848** | **0.4066** |
| FedSVRG | 0.7568 | 0.6441 | 0.7501 | 0.6618 | 0.9534 | 1.0513 |
| FedAtt | 0.7569 | 0.6434 | 0.7487 | 0.6607 | 0.9534 | 1.0509 |

---

## Key Findings

- **FedProx** is the most robust algorithm across binary and moderately imbalanced multiclass tasks, attributed to its proximal regularization that constrains local model drift from the global model.
- **FedAvg, FedSVRG, and FedAtt** undergo complete global aggregation failure on DermaMNIST, where a single class dominates over 65% of the data — effectively reducing to majority-class guessing (accuracy ≈ 0.53) from Round 1 through Round 50.
- **Local training outperforms federated collaboration** under extreme class imbalance (DermaMNIST), as global aggregation dilutes rare class diagnostic signals.
- **FedSVRG and FedAtt** show probability calibration degradation in later rounds (log-loss increases from Round 25 to Round 50 on PneumoniaMNIST and BloodMNIST), despite comparable accuracy to FedProx.

---

## Limitations and Future Work

This study acknowledges the following limitations:

1. **Client scale:** Only 3 clients were used; real-world clinical FL systems may involve dozens to hundreds of institutions.
2. **Fixed hyperparameter:** The proximal coefficient μ in FedProx was not subjected to sensitivity analysis for fairness in cross-algorithm comparison.
3. **Shallow backbone:** A simple two-layer CNN was employed; deeper architectures (ResNet-18/50, Vision Transformers) may improve feature extraction and robustness.
4. **Absence of formal privacy mechanisms:** Differential privacy (DP-SGD) and secure aggregation (SecAgg) were not implemented, limiting direct applicability to GDPR/HIPAA-regulated settings.

Recommended directions for future research include: scaling to 5–20 clients; integrating personalized FL approaches (FedPer, pFedMe, Ditto); developing class-balanced aggregation strategies for extreme imbalance; evaluating deeper backbones; and incorporating formal privacy mechanisms.

---

## Citation

If you use this work, please cite the paper and/or this repository:

### Paper (APA)

> Riyadi, M. A. Q., Dewi, A. M., Mukhlishin, Z. A. N., & Arep, Z. R. A. (2026). Comparative Evaluation of Federated Learning Algorithms in Dirichlet Non-IID Medical Imaging. *Jurnal Sains, Nalar, dan Aplikasi Teknologi Informasi*, *5*(1), 32–44. https://doi.org/10.20885/snati.v5.i1.44597

### Paper (BibTeX)

```bibtex
@article{riyadi2026federated,
  title     = {Comparative Evaluation of Federated Learning Algorithms in Dirichlet Non-IID Medical Imaging},
  author    = {Riyadi, Michael Angello Qadosy and Dewi, Adinda Mariasti and Mukhlishin, Zahid Abdullah Nur and Arep, Zalsabilah Rezky Amelia},
  journal   = {Jurnal Sains, Nalar, dan Aplikasi Teknologi Informasi},
  volume    = {5},
  number    = {1},
  pages     = {32--44},
  year      = {2026},
  doi       = {10.20885/snati.v5.i1.44597},
  url       = {https://doi.org/10.20885/snati.v5.i1.44597},
  publisher = {Universitas Islam Indonesia}
}
```

### Repository (BibTeX)

```bibtex
@misc{nnichaelangello2026federated,
  author       = {Riyadi, Michael Angello Qadosy and Dewi, Adinda Mariasti and Mukhlishin, Zahid Abdullah Nur and Arep, Zalsabilah Rezky Amelia},
  title        = {Federated Learning Medical Imaging {SNATI}-44597},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/nnichaelangello/Federated-Learning-Medical-Imaging-SNATI-44597}},
  note         = {Source code for: Comparative Evaluation of Federated Learning Algorithms in Dirichlet Non-IID Medical Imaging. DOI: 10.20885/snati.v5.i1.44597}
}
```

---

## License

This repository is associated with a published academic work. Please refer to the journal's licensing terms at [https://journal.uii.ac.id/jurnalsnati/](https://journal.uii.ac.id/jurnalsnati/) and cite appropriately when using any materials from this work.
