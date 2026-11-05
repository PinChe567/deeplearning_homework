# HW3 — Hyperparameter Tuning

**Course:** Deep Learning in Biomedical Optical Imaging (11510IPT 553000)  
**Student:** 黃品喆 (115080548)

For this assignment, I used a chest X-ray dataset to compare different training settings for binary classification (NORMAL vs. PNEUMONIA). I started from the instructor's example notebook, kept its original cell order, and added experiments at the end.

## Files

- [Jupyter Notebook — code and experiment outputs](115080548_黃品喆_HW3.ipynb)
- [Analysis report (PDF)](115080548_黃品喆_HW3.pdf)

## What I compared

The notebook contains **16 runs in total**, including the baseline:

| Part | Experiments |
|---|---|
| Baseline | MLP, Adam, learning rate 0.001, batch size 32 |
| Optimization | Learning rate, batch size, SGD with momentum, AdamW |
| Schedule and regularization | Number of epochs, cosine LR scheduler, weight decay |
| Model architecture | MLP width, dropout, and a small CNN |
| Data augmentation | No augmentation, mild transforms, horizontal flip, strong transforms |

The original instructor notebook cells are kept in the same order (cells 0–25); the additional comparisons follow in cells 26–33. The PDF explains the results and the changes made to the example.

## Dataset and evaluation

The provided dataset is heavily imbalanced: **26 NORMAL** and **540 PNEUMONIA** images. Its original validation folder has only NORMAL images, and the test folders are empty. I therefore combined the provided images and created new train/validation/test subsets, while grouping PNEUMONIA filenames with the same `personNNN` identifier.

| Split | NORMAL | PNEUMONIA |
|---|---:|---:|
| Train | 18 | 383 |
| Validation | 4 | 62 |
| Test | 4 | 95 |

Training uses a weighted sampler to reduce the impact of class imbalance. Validation results were used to select the final configuration; the held-out test split was evaluated only after model selection.

**Selected model: baseline MLP.** Its test accuracy was **97.98%**, AUROC **0.9921**, sensitivity **100%**, and specificity **50%**.

The high accuracy should be interpreted carefully: the test split contains only four NORMAL images, and two were classified as PNEUMONIA. The filename-based grouping also cannot establish full patient independence. These results are for this assignment dataset, not a clinical diagnostic evaluation.

## How to run

1. Install Python and Jupyter, then the required packages:

   ```bash
   pip install notebook torch torchvision numpy matplotlib scikit-learn Pillow
   ```

2. Obtain the course-provided chest X-ray dataset. Put the original `chest_xray_HW3*.zip` file beside the notebook, or extract it into a `chest_xray_HW3/` directory in the working folder. The images are not included in this repository.
3. Open `115080548_黃品喆_HW3.ipynb` and run all cells in order. The notebook automatically uses CUDA if available, otherwise CPU.
4. The experiment tables, plots, metrics, split manifest, and model checkpoints are generated in `results_annotated/`.

Results can vary across software and hardware environments. The notebook fixes a random seed, but that does not guarantee bit-for-bit identical results on every machine.
