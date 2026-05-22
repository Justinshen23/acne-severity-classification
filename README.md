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

The notebook expects a dataset mounted from Google Drive (`datasets_v2.zip`) with this structure:

```
datasets/
├── created_dataset/
│   ├── Level_0/
│   ├── Level_1/
│   └── Level_2/
└── unified/          # created by the notebook (train/val/test split)
```

**Do not commit raw images or zip files** — they are excluded via `.gitignore`.

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
├── requirements.txt
├── .gitignore
└── notebooks/
    └── acne_severity_project.ipynb
```

## Author

Justin Shen — [Justinshen23](https://github.com/Justinshen23)
