# RR Diner Coffee: Decision Tree Case Study

Completed Springboard Tier 3 assignment: data cleaning, EDA, four decision trees, training-only cross-validation, random forest tuning, and a business conclusion. All 48 nonempty code cells executed without errors, with plots and outputs saved.

## Files

- RR_Diner_Coffee_Tier_3.ipynb — completed notebook with written interpretations.
- RRDinerCoffeeData.csv — original data, unchanged.
- Hidden_Farm_Predictions.csv — predictions for 228 unknown responses from the CV-selected model; source_csv_row counts the header as row 1 and is not a customer ID.
- Model_Comparison.csv — evaluation metrics for all five models.
- requirements.txt — computation versions and JupyterLab.

## Run

Use Python 3.12. From this directory:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab
```

Open the notebook, then Restart Kernel and Run All Cells. On Windows use `.venv\Scripts\activate`. Keep the CSV beside the notebook. Graphviz and pydotplus are not required. Code was executed sequentially using a Python harness with captured plots; local Jupyter launch was not tested.

## Findings

The CSV contains 702 customers, not the 710 described in the scenario: 474 known decisions (303 YES, 171 NO) and 228 unknown. There are no feature missing values or exact duplicate rows.

| Model | Test accuracy | Training CV balanced accuracy |
|---|---:|---:|
| Entropy, unlimited | 100.00% | 97.84% |
| Gini, unlimited | 99.16% | 97.47% |
| Entropy, depth 3 | 91.60% | 93.73% |
| Gini, depth 3 | 98.32% | 97.63% |
| Tuned random forest | 96.64% | 95.85% |

The test set has 119 rows; the training set has 355. The perfect entropy-tree test result is specific to this split and is not a guarantee of future accuracy. Model selection uses training CV balanced accuracy, not the test score or predicted buyer count.

The selected unlimited entropy tree estimates 303 known + 181 predicted buyers = **484/702 (68.95%)**. The forest estimates **486/702 (69.23%)**. Strictly greater than 70% requires at least **492 buyers**, so both imply **NO-GO under the assignment rule**. All five estimates range from 68.38% to 69.23%.

The random forest does not improve accuracy here; that outcome is reported rather than assumed. The tuned forest uses 200 trees, depth 5 and minimum leaf size 1.

## Method and limitations

- Split seed 246 and test fraction 25%, with stratification added.
- Fit one-hot encoding only on training data, and within CV through pipelines; unknown categories are handled consistently.
- Five stratified CV folds select the tree; a small forest grid searches depth and leaf size on training data only.
- After evaluation, refit selected models on all known decisions for unknown-response predictions.
- Survey intent is not confirmed purchasing or quantity. Missing-response bias and sample representativeness remain unresolved. The 70% cutoff does not itself establish profitability. Validate with a pilot or preorders before a major commitment.
- The original full-labeled-data EDA is retained for the assignment; all eight supplied features remain fixed. Model settings are selected through training-only CV.

## Submit to GitHub

Suggested repository name: **rr-diner-coffee-decision-trees**.

1. Extract this ZIP and review the notebook.
2. Create a public GitHub repository or use an existing public repository.
3. Upload the extracted files, preserving their relative paths. Do not upload .venv.
4. Commit, open the notebook page on GitHub, and copy its URL.
5. Verify that URL in a signed-out/private browser window and submit it to Springboard.

No GitHub repository was created or uploaded as part of this deliverable.

Original scenario and data: Springboard RR Diner Coffee case study. Solution prepared with ChatGPT assistance; review and understand the answers before submission.

Reference: https://scikit-learn.org/stable/modules/tree.html
