# SIT720 HD Task 11.1 
## Option 3: Heart Disease Classification

Reproduction and stress test of Bhagat, Sharma & Agarwal (2025), An efficient stacking-base ensemble technique for early heart attack prediction, Multimedia Tools and Applications 84:36351–36375.

- **Part 1** reconstructs the paper's six classifiers (LR, DT, RF, XGBoost, NB, KNN) and its 5-fold stacking ensemble on the same 1025-row dataset.
- **Part 2** proposes a leakage-controlled evaluation protocol (deduplication, preprocessing inside the training folds, nested cross-validation), then tests whether it estimates performance on unseen records more accurately than the paper's protocol.

## Files

| File | What it is |
|------|------------|
| `11.1 HD v6.ipynb` | Main notebook |
| `heart_1025.csv` | Dataset used by the paper |
| `cleveland_303_reference.csv` | 303-row Cleveland file, used only to check where the 1025-row file comes from |
| `requirements.txt` | Pinned package versions |
| `results_v6.json` | Every result the report uses |
| `environment.json` | Python and package versions of the run |
| `oof_predictions.csv` | Out-of-fold predictions and fold numbers for the 241 development records |
| `holdout_predictions.csv` | Predictions for the 61 holdout records, every model |
| `perfold_metrics.csv` | Metrics for each model in each outer fold |
| `selected_hyperparameters.csv` | Tuned settings chosen in each outer fold and in the final fit |
| `estimate_validity.csv` | Each protocol's estimate vs accuracy on unseen records |
| `bootstrap_comparisons.csv` | Paired bootstrap comparisons between models |
| `paper_comparison_all_metrics.csv` | Differences from the paper's Table 11, every model and metric |
| `stack_across_settings.csv` | The stack under every setting, all five metrics |
| `v6_fig1_reproduction.png` … `v6_fig6_paper_comparison.png` | Figures 1–6 of the report |

## Dataset provenance

- `heart_1025.csv` : the Kaggle Heart Disease Dataset by user johnsmith88 (https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset), the link given in the paper. 1025 rows, 13 predictors and a binary `target`. It contains 723 exact duplicate rows (302 distinct records), and the codes `ca = 4` / `thal = 0` look like missing-value placeholders. Both are handled in the notebook.
- 
- `cleveland_303_reference.csv` : the 303-row, Cleveland-only version of the same file, as mirrored at https://github.com/kb22/Heart-Disease-Prediction/blob/master/dataset.csv. The original data is the UCI Heart Disease dataset (Janosi et al., 1988, doi:10.24432/C52P4X). All 302 distinct records in `heart_1025.csv` match this file exactly.

## Setup

Python 3.11 or 3.12


## Reproducibility

The notebook was run with Python 3.11.15 and again with Python 3.12.3, both using the pinned versions, and every exported value was identical. On other platforms (for example Apple-silicon Macs), probability-based values such as AUC can differ in the fourth decimal place; the outputs saved in the submitted notebook are the ones reported.

## Main findings

- In the paper-style 80/20 row split, 202 of 205 test rows have an identical copy in the training set, and the tree models and the stack reach 100% accuracy.
- Trained on the same 241 development records, the paper's protocol overestimates accuracy on 61 unseen records by 20.3 percentage points on average. The proposed nested protocol is off by 5.5.
- Under the nested protocol, random forest (0.851 pooled accuracy) beats both stacks (0.805 untuned, 0.813 tuned), and tuning gives no clear gain.
- Across all five metrics (accuracy, precision, recall, F1, AUC), every model under the nested protocol sits below the paper's Table 11, by 7.5 points on average.

## Generative AI 

I used GenAI to support planning, brainstorming, and language editing of this submission. Any suggestions were critically evaluated, modified, and integrated into my own work. I remain responsible for the accuracy, integrity, and quality of the final submission. 

