# Acne Severity Classification

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Justinshen23/acne-severity-classification/blob/main/notebooks/acne_severity_project.ipynb)

Machine learning pipeline that classifies acne images into **three severity levels** — **mild**, **moderate**, and **severe** — using handcrafted image features and convolutional neural networks.

**4,621 images** · **4 models** · **70/15/15 train/val/test split** · Built for UC Berkeley **SAAS**

---

## Results

All metrics are on the **held-out test set (695 images)** after training on the full dataset.

| Model | Method | Test accuracy |
|-------|--------|:-------------:|
| Baseline | Logistic regression (9 engineered features) | **45.84%** |
| CNN v2 | MobileNetV2 transfer learning + augmentation | **66.33%** |
| CNN v3 | EfficientNetB0 transfer learning | **63.74%** |
| **CNN v4** | **Fine-tuned MobileNetV2 (2-phase training)** | **74.68%** |

**Best model: CNN v4** — fine-tuned MobileNetV2, exceeding the 70% target.

### Earlier baseline (smaller ~999-image subset)

| Model | Test accuracy |
|-------|:-------------:|
| Logistic regression | 53.50% |
| CNN v1 (MobileNetV2) | 64.50% |

Scaling to **4,621 images** and stronger CNN training (especially v4) improved performance substantially over these early runs.

### Key findings

- **CNNs outperform handcrafted features** by ~29 points on the full test set (74.68% vs 45.84%).
- **Fine-tuning** (v4) beats frozen-backbone CNNs (v2, v3) on this task.
- **Class imbalance** (severe ≈ 17%) is handled with class weights and augmentation.
- Confusion matrices, per-class metrics, and training curves are in the [notebook](notebooks/acne_severity_project.ipynb).

---

## Quick start (Google Colab)

1. **Open the notebook** → [Open in Colab](https://colab.research.google.com/github/Justinshen23/acne-severity-classification/blob/main/notebooks/acne_severity_project.ipynb)

2. **Enable GPU** → Runtime → Change runtime type → **T4 GPU** (recommended for CNN cells)

3. **Download the dataset** (~5.4 GB)  
   **[datasets_v2.zip on Google Drive](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link)**  
   Or use `gdown` — see [DATASET.md](DATASET.md)

4. **Run all cells** top to bottom  
   - Mount Drive / unzip → EDA → create `unified/` split → train models → evaluate

5. **Try inference** — run the final upload cell with your own image (uses CNN v4)

---

## How to run locally

```bash
git clone https://github.com/Justinshen23/acne-severity-classification.git
cd acne-severity-classification
pip install -r requirements.txt
```

Download and unzip `datasets_v2.zip` ([Drive link](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link)) into `./datasets/` — see [DATASET.md](DATASET.md).

```bash
jupyter notebook notebooks/acne_severity_project.ipynb
```

---

## Pipeline

```
datasets_v2.zip
       ↓
created_dataset/  (Level_0 · Level_1 · Level_2)
       ↓
Feature extraction + EDA
       ↓
unified/  →  train (70%) · val (15%) · test (15%)
       ↓
┌──────────────────────────────────────────────────┐
│  Baseline LR  →  CNN v2  →  CNN v3  →  CNN v4   │
│                              (best: 74.68%)      │
└──────────────────────────────────────────────────┘
```

| Step | Contents |
|------|----------|
| **EDA** | Class distribution, sample images, feature histograms, correlation, pairplot, PCA, outlier detection |
| **Baseline** | Brightness, redness, saturation, contrast, edges, gradient + engineered ratios |
| **CNN v2** | Frozen MobileNetV2, heavy augmentation |
| **CNN v3** | EfficientNetB0, ImageNet preprocessing |
| **CNN v4** | Two-phase MobileNetV2: train head → unfreeze top layers → fine-tune |

---

## Dataset

| Class | Folder | Images | Share |
|-------|--------|--------|-------|
| Mild | `Level_0` | 1,763 | 38.2% |
| Moderate | `Level_1` | 2,083 | 45.1% |
| Severe | `Level_2` | 775 | 16.8% |
| **Total** | | **4,621** | |

**Layout after unzip:**

```
datasets/
├── created_dataset/
│   ├── Level_0/
│   ├── Level_1/
│   └── Level_2/
└── unified/              # created by notebook
    ├── train/
    ├── val/
    └── test/
```

| Resource | Link |
|----------|------|
| **Full dataset** (~5.4 GB) | [Google Drive — datasets_v2.zip](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link) |
| **Demo images** (9 files) | [`data/sample/`](data/sample/) |
| **Setup guide** | [DATASET.md](DATASET.md) |

---

## Project structure

```
acne-severity-classification/
├── README.md
├── DATASET.md
├── requirements.txt
├── .gitignore
├── data/sample/                 # 9 demo images (3 per class)
└── notebooks/
    └── acne_severity_project.ipynb
```

---

## Requirements

- Python 3.8+
- See [`requirements.txt`](requirements.txt): `numpy`, `pandas`, `matplotlib`, `seaborn`, `opencv-python-headless`, `scikit-learn`, `tensorflow`
- **GPU** recommended for CNN training (Colab T4 works well)

---

## Limitations

- **Sampling bias** — ACNE04 / IGA-style labels; not a random global clinical population.
- **Geographic bias** — predominantly East Asian subjects in source datasets.
- **Severity imbalance** — severe cases underrepresented (16.8%).
- **Image conditions** — clinical/controlled photos vs real-world selfies.
- **Label noise** — merged grading scales (Hayashi + IGA).

Reported test accuracy may not fully generalize to deployment settings.

---

## Author

**Justin Shen** · GitHub [@Justinshen23](https://github.com/Justinshen23)
