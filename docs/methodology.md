# Methodology

## Cleaning
Dates are parsed; delivery time is order to delivery in days, with negative or missing values set to the mean.
Customer age and account age come from date of birth and registration date.

## History features
Orders are sorted by date. For each user I compute the running count of past returns. For item, brand, size,
colour and state I compute the return rate per day and carry it forward as a recent trend feature. For the test
file the same features are looked up from the training history.

## Models
Categories are one-hot encoded and all features standardised.
- Random Forest: grid search with 5-fold CV (500 trees, sqrt features, depth 15, min leaf 2 in the final grid).
- Neural network: three ReLU hidden layers (8, 4 and 2 units) and a sigmoid output, Adam with binary cross-entropy.

## Evaluation
70/30 random split. For each decile of the predicted probability I build a confusion matrix and a lift table, and
pick a cut-off from it. Permutation importance shows which features matter.

## Known limits
Random split on time-ordered data, history features not strictly point-in-time, scaler refit on train plus test at
scoring, no calibration. The README lists what I would change.
