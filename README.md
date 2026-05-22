# Acne Severity Classification

Classify facial acne images into **three severity levels** (mild, moderate, severe) using handcrafted features and deep learning. Built for a SAAS data science project with **4,621 labeled images** and a full pipeline from EDA → baselines → CNNs.

**Course / context:** UC Berkeley SAAS — acne severity classification with ACNE04-derived labels.

---

## What this README is for

Your **README** is the front page of the repo. Someone (a grader, recruiter, or future you) should understand:

1. **What** the project does  
2. **How well** it performed (results)  
3. **How to run** it step by step  
4. **Where** the data and code live  

You do **not** need every plot in the README — keep it concise and link to the notebook for details. **Do** include headline metrics, setup steps, and the dataset link.

---

## Results

Test-set performance on the held-out split (**695 images**, 15% of 4,621). *Update the CNN rows with your exact numbers after a full notebook run.*

| Model | Method | Test accuracy |
|-------|--------|---------------|
| **Baseline** | Logistic regression (9 engineered features) | **~53.5%** |
| **CNN v2** | MobileNetV2 + augmentation (frozen backbone) | *run notebook* |
| **CNN v3** | EfficientNetB0 transfer learning | **~63.7%** |
| **CNN v4** | Fine-tuned MobileNetV2 (2-phase train) | **best model** — *run notebook* |

Earlier experiments on a smaller subset (~999 images) reached **~64.5%** (CNN v1) vs **~53.5%** (LR), motivating the full 4,621-image pipeline.

**Takeaways**

- Handcrafted features alone are a weak baseline (~53%); CNNs capture visual patterns better.  
- Class imbalance (severe ≈ 17%) — models use class weights and augmentation.  
- See the notebook for confusion matrices, training curves, and classification reports.

---

## Quick start (Google Colab)

Recommended — GPU + easy data download.

1. **Open the notebook**  
   [Open in Colab](https://colab.research.google.com/github/Justinshen23/acne-severity-classification/blob/main/notebooks/acne_severity_project.ipynb)  
   or: File → Open notebook → GitHub → `Justinshen23/acne-severity-classification`

2. **Get the data** (~5.4 GB)  
   - Download [`datasets_v2.zip`](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link) from Google Drive, **or**  
   - Use the `gdown` cell in [DATASET.md](DATASET.md)

3. **Run top to bottom**  
   - Setup & imports → unzip data → EDA → train/val/test split → models  
   - CNN training needs a **GPU** runtime (Runtime → Change runtime type → GPU)

4. **Optional:** upload a test image in the final cell to try the fine-tuned model.

---

## Pipeline overview

```
datasets_v2.zip
       ↓
  created_dataset/     (Level_0, Level_1, Level_2)
       ↓
  Feature extraction → EDA (distributions, PCA, outliers)
       ↓
  unified/ train · val · test   (70% / 15% / 15%)
       ↓
  ┌─────────────────────────────────────────────┐
  │ Baseline LR  →  CNN v2  →  v3  →  v4 (best) │
  └─────────────────────────────────────────────┘
```

| Stage | What happens |
|-------|----------------|
| **EDA** | Class balance, sample images, feature histograms, correlation, pairplot, PCA, outliers |
| **Baseline** | 9 features (brightness, redness, ratios, log-edges, …) + logistic regression |
| **CNN v2** | MobileNetV2, frozen backbone, heavy augmentation |
| **CNN v3** | EfficientNetB0, different preprocessing scale |
| **CNN v4** | MobileNetV2 fine-tuned in two phases (higher capacity head + unfreeze) |

---

## Dataset

| Class | Folder | Images |
|-------|--------|--------|
| Mild | `Level_0` | 1,763 |
| Moderate | `Level_1` | 2,083 |
| Severe | `Level_2` | 775 |
| **Total** | | **4,621** |

After unzip:

```
datasets/
├── created_dataset/
│   ├── Level_0/
│   ├── Level_1/
│   └── Level_2/
└── unified/          # created by the notebook
    ├── train/
    ├── val/
    └── test/
```

- **Full zip:** [Google Drive](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link) (~5.4 GB)  
- **Demo images in repo:** [`data/sample/`](data/sample/) (9 images)  
- **Details:** [DATASET.md](DATASET.md)

---

## Models (summary)

| Model | Approach |
|-------|----------|
| **Baseline** | Logistic regression on 9 engineered features |
| **CNN v2** | MobileNetV2 transfer learning |
| **CNN v3** | EfficientNetB0 transfer learning |
| **CNN v4** | Fine-tuned MobileNetV2 (best performance) |

---

## Local setup (optional)

```bash
git clone https://github.com/Justinshen23/acne-severity-classification.git
cd acne-severity-classification
pip install -r requirements.txt
# Download datasets_v2.zip (see DATASET.md) and unzip into ./datasets/
jupyter notebook notebooks/acne_severity_project.ipynb
```

---

## Project structure

```
acne-severity-classification/
├── README.md              # you are here
├── DATASET.md             # download & unzip instructions
├── requirements.txt
├── .gitignore
├── data/sample/           # 9 demo images
└── notebooks/
    └── acne_severity_project.ipynb
```

---

## Limitations & bias

Documented in the notebook — summary:

- Not a random clinical sample (ACNE04 / IGA-style labels, East Asian–heavy sources).  
- Severe class underrepresented (~17%).  
- Studio/clinical lighting ≠ real-world selfies.  
- Merged grading scales may add label noise.

Test accuracy may **overstate** real-world performance.

---

## Author

**Justin Shen** — [@Justinshen23](https://github.com/Justinshen23)
