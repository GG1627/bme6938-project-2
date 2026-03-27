# BloodMNIST Blood Cell Classification

> Deep learning pipeline for automated blood cell classification using the BloodMNIST dataset. Compares a custom CNN trained from scratch against fine-tuned ResNet50 and EfficientNet-B0 models.

**GitHub:** [bme6938-project-2](https://github.com/GG1627/bme6938-project-2)  
**Course:** Medical AI (BME6938) — University of Florida

---

## Clinical Context

Accurate blood cell classification is critical in hematology for diagnosing conditions such as leukemia, anemia, and infections. Manual microscopic analysis is time-consuming and subject to inter-observer variability. This project develops an automated CNN-based pipeline to classify 8 blood cell types from peripheral blood smear images, with the goal of supporting clinical decision-making in hematological diagnosis.

---

## Results Summary

| Model | Test Accuracy | Macro F1 | ROC-AUC |
|-------|:---:|:---:|:---:|
| Scratch CNN | 98.51% | 0.9853 | 0.9992 |
| ResNet50 | 98.71% | 0.9883 | 0.9993 |
| EfficientNet-B0 | 98.83% | 0.9894 | 0.9995 |

---

## Repository Structure
```
bme6938-project-2/
├── data/                        # dataset stored here (not tracked by git)
├── figures/                     # generated plots and visualizations
│   ├── all_models_training_curves.png
│   ├── confusion_matrix_*.png
│   └── gradcam.png
├── notebooks/
│   ├── download_data.ipynb   # run once to download dataset
│   ├── eda.ipynb             # exploratory data analysis
│   ├── train.ipynb           # full training pipeline
│   └── demo.ipynb            # load trained model and run inference
├── models/                      # saved model weights (not tracked by git)
├── requirements.txt
└── README.md
```

---

## Quick Start

### Option A — Local

**Requirements:** Python 3.10+, CUDA-compatible GPU recommended
```bash
# clone the repo
git clone https://github.com/GG1627/bme6938-project-2.git
cd bme6938-project-2

# create and activate virtual environment
python -m venv venv
source venv/bin/activate        # on Windows: venv\Scripts\activate

# install dependencies
pip install -r requirements.txt
```

Then open and run the notebooks in order:
1. `notebooks/download_data.ipynb`
2. `notebooks/eda.ipynb`
3. `notebooks/train.ipynb`
4. `notebooks/demo.ipynb`

### Option B — HiPerGator (UF)

When launching a Jupyter session on [ood.rc.ufl.edu](https://ood.rc.ufl.edu), use these settings:

| Setting | Value |
|---|---|
| Cluster Partition | `default` |
| Number of CPUs | `4` |
| Memory (GB) | `16` |
| GRES | `gpu:1` |
| Environment Modules | `pytorch/2.8.0` |

After the session launches, select the **PyTorch-2.8.0** kernel from the Jupyter kernel selector in the top right of the notebook.

> Note: the `pytorch/2.8.0` module ensures CUDA compatibility with HiPerGator's L4/A100 GPUs. Without it, PyTorch will fall back to CPU and training will be significantly slower.

Then run the notebooks in order as above.

---

## Data

- **Source:** [MedMNIST](https://medmnist.com/) — BloodMNIST dataset
- **Size:** 17,092 images (11,959 train / 1,712 val / 3,421 test)
- **Image size:** 64×64 RGB (224×224 also supported)
- **Classes:** 8 blood cell types
  - Basophil, Eosinophil, Erythroblast, Immature Granulocytes (IG), Lymphocyte, Monocyte, Neutrophil, Platelet
- **License:** CC BY 4.0
- **Citation:** Yang et al., MedMNIST v2, Scientific Data, 2023

> The dataset is not included in this repository. Run `00_download_data.ipynb` to download it automatically via the `medmnist` package.

---

## Methods Overview

- **Data augmentation:** random horizontal/vertical flips, rotation (±25°), color jitter, ImageNet normalization
- **Models:** Custom 4-block CNN, ResNet50 (ImageNet pretrained), EfficientNet-B0 (ImageNet pretrained)
- **Training:** Adam optimizer (lr=1e-3), ReduceLROnPlateau scheduler, early stopping (patience=5), CrossEntropyLoss
- **Evaluation:** Accuracy, Precision, Recall, F1 (per-class + macro), ROC-AUC (one-vs-rest), Confusion Matrix, Grad-CAM

---

## Environment
```
Python        3.10
PyTorch       2.8.0
torchvision   0.26.0
medmnist      3.0.2
```

Full list of dependencies in `requirements.txt`.

---

## Authors & Contributions

| Name | Role |
|------|------|
| Gael Garcia | Project lead, model architecture, training pipeline, GitHub setup |
| Gopal Viraj Koundinya Vutukuru | Data preprocessing, augmentation strategy, evaluation metrics |
| James Boyd | Literature review, EDA notebook, results analysis and visualizations |

---

## AI Disclosure

Claude was used as a coding assistants during development. All code was reviewed, tested, and understood by the team. AI assistance is documented per course guidelines.