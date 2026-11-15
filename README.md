# E-commerce returns prediction (2021)

A case study where I predicted whether an item bought online will be sent back. I cleaned the order data, built
customer and product history features (past return rates by user, item, brand, size, colour and state), trained a
Random Forest and a small neural network, and chose a probability cut-off using a percentile and lift table. The
Random Forest was the better model; the neural network needed more work.

**When:** built in 2021 (the notebook is dated June / July 2021).

**Modern version:** the maintained version is
[ecommerce-return-prediction](https://github.com/Avinash186200/ecommerce-return-prediction).

## Problem

Returns cost online shops money in shipping and handling. If the business can score the risk of a return at order
time, it can act on it, for example with size advice or different return terms. The task: given an order line and
the customer, predict the probability that it will be returned.

## Approach

- **Data checks:** date parsing, delivery time (negative or missing values set to the mean), customer age and
  account age, share of missing values per column.
- **Features:**
  - running return history per user (cumulative returns up to each order date)
  - recent return rate per item, brand, size, colour and state
  - price, delivery time, user age, account age
  - one-hot encoded categories
- **Exploration:** return rate by each variable, written to a workbook, to see which variables separate returns.
- **Models:**
  - Random Forest with grid search and 5-fold CV on the training set
  - Keras feed-forward network with three hidden layers
- **Validation:** 70/30 train/test split, confusion matrix and lift table at each probability decile, permutation
  importance (`rfpimp`).
- **Scoring:** the same features are rebuilt for the unseen test file using the training history, and the
  Random Forest scores it.

## Notebooks

| Notebook | What it does |
|---|---|
| [`01_return_prediction_2021-06-07.ipynb`](notebooks/01_return_prediction_2021-06-07.ipynb) | Data checks, EDA, history features, Random Forest and NN models, lift tables, scoring the test file |

## How to run

```bash
python -m venv .venv
.venv/Scripts/activate        # on Linux / macOS: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/01_return_prediction_2021-06-07.ipynb
```

The data is not included; see [data/README.md](data/README.md) for the columns and set `path` in the notebook.
More detail is in [docs/methodology.md](docs/methodology.md).

## What I would do differently today

- **Time-based split.** I used a random 70/30 split. Orders are in time, and the history features use past
  returns, so a random split lets the model see the future. I would train on earlier orders and test on later ones.
- **Leakage in history features.** The return-rate features should use only orders whose return outcome was known
  before the order date, not the same day, and should be computed on the training window only.
- **Scaling.** I fitted the scaler for the scoring step on train and test together. It should be fitted on train
  only and reused.
- **Calibration and cut-off.** The cut-offs (0.48 to 0.65) were picked by eye. I would calibrate the probabilities
  and choose the cut-off from the cost of a return vs the cost of acting on a false alarm.
- **Metrics.** Report ROC AUC, PR AUC and the Brier score, not only accuracy and lift.
- **Importance.** I used R-squared as the score for permutation importance on a classifier; I would use log loss or
  AUC on a validation set.
- **Pipeline.** One scikit-learn pipeline for encoding, scaling and the model, so training and scoring cannot drift.
