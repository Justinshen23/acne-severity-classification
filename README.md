# Acne Severity Classification

Machine learning project for classifying acne severity into three levels (mild, moderate, severe) using handcrafted image features and convolutional neural networks.

## Models

| Model | Approach |
|-------|----------|
| **Baseline** | Logistic regression on 9 engineered features |
| **CNN v2** | MobileNetV2 transfer learning |
| **CNN v3** | EfficientNetB0 transfer learning |
| **CNN v4** | Fine-tuned MobileNetV2 (best performance) |

## Dataset

**4,621 images** (mild / moderate / severe) — stored separately from this repo (~5.4 GB as `datasets_v2.zip`).

The notebook expects this layout after unzip:

```
datasets/
├── created_dataset/
│   ├── Level_0/      # 1,763 images
│   ├── Level_1/      # 2,083 images
│   └── Level_2/      # 775 images
└── unified/          # created by the notebook (train/val/test split)
```

### What’s in this repo

- **`data/sample/`** — 9 demo images (3 per class), safe to browse on GitHub  
- **Full data (~5.4 GB)** — host on Google Drive (not in git); see below

### Download full dataset

**[datasets_v2.zip on Google Drive](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link)** (~5.4 GB)

See [DATASET.md](DATASET.md) for Colab (`gdown`) and unzip steps.

**Do not commit** the full zip or `datasets/` folder — blocked by `.gitignore` (GitHub 100 MB/file limit).

## Run in Google Colab

1. Open [`notebooks/acne_severity_project.ipynb`](notebooks/acne_severity_project.ipynb) in [Google Colab](https://colab.research.google.com/).
2. Upload the notebook or open from this repo.
3. Run cells top to bottom.
4. Mount Google Drive when prompted and place `datasets_v2.zip` in your Drive.

## Local setup (optional)

```bash
pip install -r requirements.txt
jupyter notebook notebooks/acne_severity_project.ipynb
```

## Project structure

```
acne-severity-classification/
├── README.md
├── DATASET.md
├── requirements.txt
├── .gitignore
├── data/sample/          # 9 demo images (full zip on Drive)
└── notebooks/
    └── acne_severity_project.ipynb
```

## Author

Justin Shen — [Justinshen23](https://github.com/Justinshen23)
