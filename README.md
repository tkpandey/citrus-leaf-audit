# Lightweight CNNs for citrus leaf health screening: benchmarking accuracy and auditing dataset bias

Code accompanying the manuscript:

> Pandey TK, Dheer P, Dwivedi M, Patel SK. *Lightweight CNNs for citrus leaf health screening: benchmarking accuracy and auditing dataset bias.* Submitted to *Signal, Image and Video Processing*.

The notebooks reproduce every result reported in the paper: the five-model benchmark, Grad-CAM maps, the dataset-bias audit, the stress test and the statistical analysis.

## Dataset

The images are **not** included in this repository. Download the **Citrus Plant Leaves Dataset** from IndiaAI AIKosh:

https://aikosh.indiaai.gov.in/home/datasets/details/citrus_plant_leaves_dataset.html

Unzip it to a folder (for example `MyDrive/Citrus_Plant_Leaves` on Google Drive). The folder should contain one sub-folder per class (healthy / unhealthy).

## Notebooks (run in this order)

| # | Notebook | What it does | Main outputs |
|---|----------|--------------|--------------|
| 1 | `notebooks/01_benchmark_gradcam.ipynb` | Near-duplicate removal (pHash, Hamming ≤ 4); stratified 5-fold CV of MobileNetV2, EfficientNet-B0, ShuffleNetV2 1.0×, ResNet-18 and SqueezeNet 1.1; Grad-CAM | `duplicates_removed.csv`, per-fold metrics, confusion matrix, training curves, Grad-CAM figures, `resnet18_fold1.pt` |
| 2 | `notebooks/02_reliability_checks.ipynb` | Format/size/background audit, 8-pixel background classifier, embedding-based hidden-copy detection with group-wise CV | audit tables |
| 3 | `notebooks/03_stress_test_source_matched.ipynb` | JPEG (q90), resize (256), grey-background and harmonised stress test; source-matched (256×256 PNG) subset | stress-test tables |
| 4 | `notebooks/04_statistical_analysis.ipynb` | Retrains 5 models × 5 folds and saves per-image predictions; Clopper–Pearson and bootstrap CIs, McNemar, Cochran's Q, Holm correction; forest plot | `stats/tableA–D*.csv`, forest plot |

Notebooks 2–4 read files written by notebook 1, so keep the same `OUT_DIR` for all four.

## Results included

The `results/` folder holds the output files behind every table and figure in the paper, so the numbers can be checked without re-running anything. Image paths are relative to the dataset folder.

- `run_info.json`, `table1_dataset_counts.csv`, `duplicates_removed.csv`: dataset after near-duplicate removal (991 images)
- `per_fold_results.csv`, `table2_model_comparison.csv`: five-fold benchmark
- `check1_*`, `check2_*`, `check3*`, `check4_*`: reliability audit
- `final_A_stress_test.csv`, `final_B_*`: stress test and source-matched subset
- `per_image_predictions.csv`: per-image probabilities for every model, fold and image variant (original, JPEG q90, resized 256, grey background, harmonised)
- `tableA–D_*.csv`: confidence intervals, McNemar tests, Cochran's Q

## How to run (Google Colab)

1. Open a notebook in Colab (File → Open notebook → GitHub, paste this repository's URL).
2. Runtime → Change runtime type → **GPU**.
3. In the settings cell, set `DATA_DIR` (dataset folder) and `OUT_DIR` (results folder) if yours differ from the defaults.
4. Run all cells. Notebook 4 is resumable: if Colab disconnects, run it again and it continues from the last finished fold.

Random seed: 42 throughout.

## Requirements

Colab already has most packages. Locally: `pip install -r requirements.txt` (Python ≥ 3.10).

## Licence

Code: MIT licence (see `LICENSE`). The dataset is subject to the terms set by its provider on AIKosh.

## How to cite

Please cite the paper (citation will be updated on publication) and this archived code release (see `CITATION.cff` or the Zenodo DOI badge).

## Contact

Tushar Kumar Pandey, Dr. Rajendra Prasad Central Agricultural University, Pusa, Bihar, India — tushar.pandey@rpcau.ac.in — ORCID 0009-0008-5106-5994
