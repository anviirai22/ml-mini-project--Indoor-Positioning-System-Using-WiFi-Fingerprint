# Wi-Fi Indoor Floor Localization Using Machine Learning

## Team

**Team #3**

1. Anvii S Rai (PES2UG24CS076)
2. Ayush Kumar (PES2UG24CS101)

---

## 1. Project Overview

This project investigates indoor floor localization using Wi-Fi RSSI fingerprints.

The project has two main components:

1. A **paper-inspired baseline** based on PCA and multiple machine learning classifiers.
2. An **alternative ensemble approach** using Extra Trees and HistGradientBoosting with additional Wi-Fi summary features.

The approaches are evaluated using Accuracy and Macro-F1.

---

## 2. Dataset

The dataset contains Wi-Fi RSSI fingerprints from **520 Wireless Access Points (WAPs)**.

The main input features are:

```text
WAP001 ... WAP520
```
The target variable is:
```
FLOOR
```
The dataset uses the value 100 to indicate that a WAP was not observed.

The project uses:

- `trainingData.csv`
- `validationData.csv`

## 3. Project Workflow
```
Wi-Fi RSSI Data
      ↓
Data Exploration
      ↓
Paper-Inspired Baseline
      ↓
Exploratory Alternatives
      ↓
Final Alternative Approach
      ↓
Performance Comparison
```
## 4. Paper-Inspired Baseline

Implementation:

`notebooks/02_paper_baseline.ipynb`

The baseline follows the main methodology of the reference paper using:

- Missing-value handling
- Standardization
- PCA
- Multiple classifiers
- 10-fold Stratified Cross-Validation
  
### Preprocessing

The value `100` is converted to a missing value.

Missing values are then replaced with `-105`, followed by standardization and PCA.

### PCA Experiments

The following PCA dimensions were investigated:

- 5 components
- 50 components
- 200 components

The main model comparison uses 50 components.

### Models

Four classifiers were evaluated:

- k-Nearest Neighbors
- Decision Tree
- Gradient Boosting
- Support Vector Machine

### Evaluation

The baseline uses:
- 10-fold Stratified Cross-Validation
- Accuracy
- Macro-F1


## 5. Exploratory Experiments

Implementation:

`notebooks/05_exploratory_alternatives.ipynb`

Five alternative approaches were investigated:

| Approach | Accuracy | Macro-F1 |
|---|---:|---:|
| PCA + Distance-Based kNN | 83.53% | 82.90% |
| SelectKBest + 1-NN | 59.50% | 62.97% |
| PCA + Extra Trees | 84.97% | 83.57% |
| Tuned Extra Trees | 90.73% | 89.86% |
| HistGradientBoosting | 29.25% | 22.57% |

These experiments helped identify suitable feature representations and models for the final alternative approach.


## 6. Final Alternative Approach

Implementation:

`notebooks/03_improved_alternative.ipynb`

Instead of reducing the Wi-Fi fingerprint using PCA, the alternative approach retains all 520 WAP features and adds five summary features:

- Observed WAP count
- Mean RSSI
- RSSI standard deviation
- Maximum RSSI
- Minimum RSSI

This produces 525 features in total.

### Models
Two tree-based models are used:
- Extra Trees
- HistGradientBoosting

Extra Trees uses:
- 300 trees
- Balanced class weights
- `random_state=42`

The final prediction combines model probabilities using:
```
60% Extra Trees
40% HistGradientBoosting
```

## 7. Final Alternative Results

The final ensemble achieved:

| Metric | Score |
|---|---:|
| Accuracy | 91.63% |
| Macro-F1 | 91.31% |

The confusion matrix is saved as:

`figures/ensemble_confusion_matrix.png`

Results are saved in:

`results/improved_alternative_results.csv`

## 8. Baseline Results

The main paper-inspired comparison using PCA(50) achieved the following cross-validation results:

| Model | Accuracy | Macro-F1 |
|---|---:|---:|
| kNN + PCA(50) | 99.16% | 99.23% |
| Decision Tree + PCA(50) | 95.73% | 95.98% |
| Gradient Boosting + PCA(50) | 98.28% | 98.23% |
| SVM + PCA(50) | 94.32% | 94.19% |

The comparison is generated in:

`notebooks/04_results_comparison.ipynb`

and saved as:

`figures/model_comparison.png`

> **Evaluation note:** The baseline uses 10-fold cross-validation on the training data, while the final alternative is evaluated on the held-out validation set. Therefore, the scores should be interpreted according to their respective evaluation procedures.



## 9. Key Findings
- PCA dimensionality has a significant effect on model performance.
- The PCA-based baseline performs strongly with 50 components.
- Exploratory experiments showed that feature representation strongly affects localization performance.
- The final alternative approach retains the original RSSI information and adds Wi-Fi summary features.
- The final alternative ensemble achieved 91.63% Accuracy and 91.31% Macro-F1 on the validation dataset.
  
## 10. Project Structure
```
wifi-indoor-localization/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   └── raw/
│       ├── trainingData.csv
│       └── validationData.csv
│
├── notebooks/
│   ├── 01_dataset_exploration.ipynb
│   ├── 02_paper_baseline.ipynb
│   ├── 03_improved_alternative.ipynb
│   ├── 04_results_comparison.ipynb
│   └── 05_exploratory_alternatives.ipynb
│
├── results/
│   ├── baseline_pca5.csv
│   ├── baseline_pca50.csv
│   ├── baseline_pca200_partial.csv
│   ├── baseline_pca_comparison.csv
│   └── improved_alternative_results.csv
│
├── figures/
│   ├── class_distribution.png
│   ├── observed_wap_count.png
│   ├── pca_comparison.png
│   ├── model_comparison.png
│   ├── ensemble_confusion_matrix.png
│   └── exploratory_alternatives.png
│
└── docs/
    ├── methodology.md
    └── final_report.pdf
```

### 11. Installation

Clone the repository:
```
git clone <repository-url>
cd wifi-indoor-localization
```
Create a virtual environment:
```
python -m venv .venv
```
Activate it on Windows:
```
.venv\Scripts\activate
```
Install dependencies:
```
pip install -r requirements.txt
```
## 12. Requirements

The project uses:
```
pandas
numpy
matplotlib
scikit-learn
jupyter
```
## 13. Running the Project

Run the notebooks in the following order:
```
1. 01_dataset_exploration.ipynb
2. 02_paper_baseline.ipynb
3. 05_exploratory_alternatives.ipynb
4. 03_improved_alternative.ipynb
5. 04_results_comparison.ipynb
```

The notebooks generate the required results and figures in the `results/` and `figures/` directories.

## 14. Reproducibility
- Random state `42` is used where applicable.
- The baseline uses 10-fold Stratified Cross-Validation.
- The alternative approach uses the provided validation dataset.
- Results and figures are saved in the repository.
- Raw dataset files are excluded from Git using `.gitignore`.

## 15. Reference

Dan Li, Le Wang, Shiqi Wu,
#### "Indoor Positioning System Using Wifi Fingerprint", Stanford University.
