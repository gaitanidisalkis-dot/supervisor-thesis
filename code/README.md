# Κώδικας Διπλωματικής — Περιβάλλον Ανάπτυξης

## Python Environment

- Python 3.11.9
- Virtual environment: `venv/` (δεν ανεβαίνει στο GitHub)

## Εγκατεστημένες Βιβλιοθήκες

| Βιβλιοθήκη | Χρήση |
|---|---|
| pandas, numpy | Επεξεργασία δεδομένων |
| matplotlib, seaborn | Οπτικοποίηση |
| scikit-learn | Logistic Regression, preprocessing |
| xgboost | Κύριο μοντέλο ML |
| shap | Ερμηνευσιμότητα (SHAP values) |
| fairlearn | Bias detection |
| tensorflow / keras | Feed-Forward Neural Network |
| ipykernel | Jupyter notebooks στο VS Code |

Όλες οι εκδόσεις αποθηκευμένες στο `requirements.txt`.

## Δομή Φακέλων
```
code/
├── data/                  # Dataset (δεν ανεβαίνει στο GitHub)
├── notebooks/             # Jupyter notebooks ανάλυσης
├── figures/               # Γραφήματα για το LaTeX
├── requirements.txt       # Βιβλιοθήκες
└── README.md
```

## Εκτέλεση
```bash
venv\Scripts\activate
```
```
