# Methodology

## A. Paper-inspired baseline

The reference paper uses UJIIndoorLoc, PCA for dimensionality reduction, and four classifiers: kNN, decision tree, gradient boosting and SVM. It uses cross-validation and studies different feature/sample sizes.

Our implementation reproduces this core experimental idea in Python/scikit-learn rather than copying the original R implementation.

### Pipeline

WiFi RSSI
→ missing-value handling
→ standardization
→ PCA
→ classifier
→ floor prediction

## B. Alternative

The alternative treats indoor positioning as a hierarchical problem:

WiFi fingerprint
→ Building prediction
→ Building-specific floor prediction
→ coordinate regression

It also adds:
- number of detected APs
- mean RSSI
- RSSI standard deviation
- strongest RSSI
- weakest RSSI

The intent is to expose signal availability and signal-distribution information that may be diluted by PCA.

## Limitations addressed

The reference paper identifies missing-value handling for kNN and future work around moving users, phone types and minimum WiFi sources. Our alternative explicitly models missingness/signal availability and expands evaluation from only floor classification to building, floor and coordinates.

This does not prove robustness to every phone or moving-user scenario; those require dedicated experiments.
