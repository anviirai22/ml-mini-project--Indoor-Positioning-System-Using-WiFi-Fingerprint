# Experiments to run

## Experiment 1 — PCA dimension

Run baseline models with:

- PCA = 5
- PCA = 50
- PCA = 200

Record:
- CV accuracy
- validation accuracy
- macro-F1
- runtime

## Experiment 2 — Model comparison

Compare:
- kNN
- Decision Tree
- Gradient Boosting
- SVM
- Alternative ExtraTrees

## Experiment 3 — Alternative localization

Report:
- building accuracy
- floor accuracy
- building macro-F1
- floor macro-F1
- latitude MAE
- longitude MAE

## Experiment 4 — Sparse WiFi robustness

Artificially hide a fixed percentage of observed RSSI values and compare baseline kNN with the alternative.

Suggested missingness levels:
- 0%
- 10%
- 20%
- 30%

This experiment should be described as a controlled robustness experiment, not as a claim about real-world missingness.
