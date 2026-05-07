# Πλαίσιο Επικύρωσης Μοντέλων Μηχανικής Μάθησης στον Πιστωτικό Κίνδυνο: Ερμηνευσιμότητα, Απόδοση, Μεροληψία και Παρακολούθηση

<!-- thesis PDF θα προστεθεί μετά την κατάθεση -->

**Φοιτητής:** Νεόκλητος Γαϊτανίδης, ΑΜ: 10031
**Επιβλέπων:** κ. Παπαλάμπρου
**Ίδρυμα:** Αριστοτέλειο Πανεπιστήμιο Θεσσαλονίκης (AUTH)
**Έτος:** 2026

---

## Περιγραφή

Το αποθετήριο αυτό περιέχει τον κώδικα της διπλωματικής εργασίας, η οποία πραγματεύεται την υλοποίηση και επικύρωση τριών μοντέλων μηχανικής μάθησης για τον πιστωτικό κίνδυνο: Logistic Regression, XGBoost και Feed-Forward Neural Network (FFNN). Τα μοντέλα εκπαιδεύτηκαν στο FICO HELOC dataset.

Κύρια έμφαση δίνεται στο XGBoost ως κύριο μοντέλο, με πλήρη ανάλυση ερμηνευσιμότητας μέσω SHAP, ανίχνευση μεροληψίας μέσω Fairlearn, και παρακολούθηση μέσω δεικτών PSI και CSI.

Βασικό συμπέρασμα της εργασίας είναι ότι τα πιο σύνθετα μοντέλα δεν αποδίδουν απαραίτητα καλύτερα, αλλά απαιτούν πάντα εκτενέστερη επικύρωση.

---

## Δομή

```
thesis-credit-risk/
├── code/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_models_performance.ipynb
│   ├── 03_interpretability.ipynb
│   ├── 04_bias_detection.ipynb
│   └── 05_monitoring.ipynb
├── data/
│   ├── heloc_dataset_v1.csv        # Original FICO HELOC dataset
│   ├── processed/                  # Παράγεται από το NB01
│   └── models/                     # Παράγεται από το NB02
├── figures/                        # Παράγεται κατά την εκτέλεση
├── thesis_latex/                   # LaTeX source
├── requirements.txt
└── README.md
```

---

## Δεδομένα

**Dataset:** FICO HELOC Dataset

**Πηγή:** [Kaggle — Home Equity Line of Credit (HELOC)](https://www.kaggle.com/datasets/averkiyoliabev/home-equity-line-of-creditheloc)

Το dataset προέρχεται από το FICO Explainable ML Challenge 2018. Το αρχείο `heloc_dataset_v1.csv` συμπεριλαμβάνεται στο αποθετήριο στον φάκελο `data/`.

Τα δεδομένα χρησιμοποιούνται αποκλειστικά για ακαδημαϊκούς σκοπούς.

---

## Τεχνικές Απαιτήσεις

- Python 3.11.9
- Δοκιμασμένο σε Windows 10/11

Βασικά πακέτα: `scikit-learn`, `xgboost`, `tensorflow`, `shap`, `fairlearn`, `pandas`, `numpy`, `matplotlib`, `seaborn`

Εγκατάσταση εξαρτήσεων:

```bash
pip install -r requirements.txt
```

**Σημείωση για Windows:** Το TensorFlow τρέχει μόνο σε CPU. Η εκπαίδευση του FFNN θα είναι αισθητά πιο αργή σε σχέση με Linux/Mac όπου υπάρχει υποστήριξη GPU.

---

## Εκτέλεση

Τα notebooks πρέπει να εκτελεστούν με τη σειρά (01 → 05), καθώς κάθε ένα εξαρτάται από τα αποτελέσματα του προηγούμενου.

| Notebook | Περιεχόμενο |
|---|---|
| `01_data_preprocessing.ipynb` | Φόρτωση και προεπεξεργασία δεδομένων |
| `02_models_performance.ipynb` | Εκπαίδευση μοντέλων και αξιολόγηση απόδοσης |
| `03_interpretability.ipynb` | Ερμηνευσιμότητα (SHAP, PDP, feature importance) |
| `04_bias_detection.ipynb` | Ανίχνευση μεροληψίας και mitigation (Fairlearn) |
| `05_monitoring.ipynb` | Παρακολούθηση μοντέλου (PSI, CSI, drift) |

---

## Άδεια

Πρόκειται για αποθετήριο διπλωματικής εργασίας. Ο κώδικας διατίθεται για ακαδημαϊκή αναφορά και δεν προορίζεται για εμπορική χρήση.
