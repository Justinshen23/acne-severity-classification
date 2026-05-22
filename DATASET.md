# Dataset guide

Your data lives on your Mac at:

```
/Users/justinshen/Downloads/SAAS Project - Acne - Justin Shen/
```

## What to use

| File / folder | Size | Use in this project? |
|---------------|------|---------------------|
| **`datasets_v2.zip`** | ~5.4 GB | **Yes** — this is what the Colab notebook expects |
| `datasets/created_dataset/` | ~2 GB | Same images, already extracted (4,621 total) |
| `datasets_v2` full `datasets/` tree | ~5.5 GB | Includes ACNE04, grading CSVs, etc. |
| `datasets.zip` | ~2.4 GB | Older bundle — use `datasets_v2.zip` instead |

### `created_dataset` layout (4,621 images)

```
datasets/created_dataset/
├── Level_0/   # 1,763 mild
├── Level_1/   # 2,083 moderate
└── Level_2/   # 775 severe
```

The notebook builds `datasets/unified/train|val|test` from `created_dataset` automatically.

---

## Do not upload the full dataset to GitHub

GitHub is for **code**, not multi‑GB image folders.

| Limit | Why it matters |
|-------|----------------|
| **100 MB** max per file | `datasets_v2.zip` is ~5,400 MB |
| Repos should stay **&lt; 1 GB** | Your folder is ~14 GB |
| Git is slow on images | Every clone would download everything |

The repo `.gitignore` already blocks `*.zip`, `data/`, and `datasets/` so you don’t accidentally commit them.

---

## Download full dataset (Google Drive)

**`datasets_v2.zip`** (~5.4 GB, 4,621 images):

**[Download datasets_v2.zip on Google Drive](https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link)**

Share setting: anyone with the link can view.

### Option A — Colab (mount Drive)

If the file is in your Drive, mount and unzip as in the notebook:

```python
from google.colab import drive
drive.mount('/content/drive')
# unzip from MyDrive/datasets_v2.zip → /content/datasets/
```

### Option B — Colab or local (`gdown`)

```python
!pip install -q gdown
!gdown --fuzzy "https://drive.google.com/file/d/1QlSPUP3Hg1298mErNxYm435nQl-F3G1A/view?usp=drive_link"
!mkdir -p /content/datasets && unzip -q datasets_v2.zip -d /content/datasets/
```

File ID (for scripts): `1QlSPUP3Hg1298mErNxYm435nQl-F3G1A`

---

## Optional: tiny sample inside the repo

For a **demo only** (a few images so others can see folder structure):

```bash
cd /Users/justinshen/acne-severity-classification
mkdir -p data/sample/Level_{0,1,2}

SRC="/Users/justinshen/Downloads/SAAS Project - Acne - Justin Shen/datasets/created_dataset"
for level in Level_0 Level_1 Level_2; do
  ls "$SRC/$level" | head -3 | while read f; do
    cp "$SRC/$level/$f" "data/sample/$level/"
  done
done

git add data/sample/
git commit -m "Add small dataset sample for demo"
git push
```

Keep this to **~10–20 images total** (a few MB), not the full 4,621.

---

## Other hosting options (later)

| Method | Good for |
|--------|----------|
| [Kaggle Dataset](https://www.kaggle.com/datasets) | Public ML datasets, free hosting |
| [Zenodo](https://zenodo.org/) | Academic projects, DOI |
| GitHub Release | Files **&lt; 2 GB** each only |
| Git LFS | Large files in git (paid quota, slow) |

---

## Quick checklist

- [ ] Full zip on **Google Drive** (or Kaggle), not in git  
- [ ] Download link in **README.md**  
- [ ] Optional: **9-image** sample under `data/sample/`  
- [ ] Never `git add datasets_v2.zip` or `datasets/`  
