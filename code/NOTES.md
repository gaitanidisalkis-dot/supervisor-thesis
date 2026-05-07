# Σημειώσεις Κώδικα — Διπλωματική

## Notebook 01 — Data Preprocessing
### Dataset
- 10.459 εγγραφές, 24 στήλες
- Target: RiskPerformance (Bad 52.2% / Good 47.8%)
- Missing values κωδικοποιημένα ως -9

### Αποφάσεις Preprocessing
- Οι -9 εγγραφές έχουν target (331 Bad / 267 Good) αλλά μηδενικά χαρακτηριστικά
- Εξήγηση: δανειολήπτες χωρίς πιστωτικό ιστορικό τη στιγμή αίτησης
- Το μοντέλο δεν έχει τίποτα να μάθει από αυτές → αφαιρούνται
- Μετά τον καθαρισμό: 9.861 εγγραφές, 0 missing values
- Class distribution: 52% Bad / 48% Good — παραμένει ισορροπημένο

### Split & Scaling
- Split: 70/15/15 (train/val/test) με stratification
- Train: 6.902 / Val: 1.479 / Test: 1.480
- Bad rate σταθερό 52% και στα τρία sets
- Scaling: StandardScaler fit μόνο στο train set (αποφυγή data leakage)
- Αποθηκεύτηκαν 11 αρχεία στο data/processed/
- feature_names.npy, scaler.pkl, X/y train/val/test (raw + scaled)

## Notebook 02 — Models Performance

### Logistic Regression (Baseline)
- AUC-ROC: 0.7894 (test)
- Gini: 0.5787 (test)
- KS: 0.4554 (test)
- Accuracy: 0.7209 (test)
- Overfitting: καμία (διαφορά train/val = 0.01)

### XGBoost (Κύριο μοντέλο)
- AUC-ROC: 0.7963 (test)
- Gini: 0.5927 (test)
- KS: 0.4577 (test)
- Accuracy: 0.7243 (test)
- Best iteration: 140 (early stopping)
- Overfitting: μέτριο (διαφορά train/val = 0.053)
- Val/Test κοντά → καλή γενίκευση

### FFNN (Complexity Benchmark)
- AUC-ROC: 0.7923 (test)
- Gini: 0.5846 (test)
- KS: 0.4639 (test)
- Accuracy: 0.7270 (test)
- Early stopping: epoch 30, best epoch 15
- Overfitting: καμία (διαφορά 0.014)

### Σύγκριση Μοντέλων
- XGBoost > FFNN > LR σε AUC και Gini
- Διαφορές πολύ μικρές — πολυπλοκότητα δεν αποδίδει ανάλογα

## Notebook 03 — Interpretability

### SHAP Global — XGBoost Top Features
1. ExternalRiskEstimate — συνθετικό score FICO
2. MSinceMostRecentInqexcl7days — πρόσφατες πιστωτικές έρευνες
3. NetFractionRevolvingBurden — χρήση revolving πίστωσης
4. AverageMInFile — ηλικία πιστωτικού ιστορικού

### SHAP Local Analysis
- Bad δανειολήπτης: f(x)=1.059, κυρίως λόγω χαμηλού ExternalRiskEstimate=58
- Good δανειολήπτης: f(x)=-1.678, κυρίως λόγω παλιού ιστορικού και καθαρού record
- Το local SHAP επιτρέπει εξήγηση κάθε απόφασης ξεχωριστά → κανονιστική συμμόρφωση

### PDP — XGBoost Top 4 Features
- ExternalRiskEstimate: απότομη πτώση κινδύνου μεταξύ 60-80
- MSinceMostRecentInqexcl7days: υψηλός κίνδυνος κοντά στο 0, πέφτει μετά
- NetFractionRevolvingBurden: ανοδική τάση — περισσότερο χρέος = υψηλότερος κίνδυνος
- AverageMInFile: καθοδική τάση — παλαιότερο ιστορικό = χαμηλότερος κίνδυνος

### SHAP FFNN — Ολοκληρώθηκε
- Έτρεξε με background=500, sample=2000, nsamples=500
- Χρόνος εκτέλεσης: ~2.5 ώρες (CPU: i5-10600K)
- Αποτελέσματα αποθηκευμένα: ../data/processed\shap_ffnn_values_20260325_065734.npy
- Ο κώδικας πλέον έχει if-check και φορτώνει αυτόματα από το αρχείο
- Εάν δεν υπάρχει θα ξαναβρεί τις τιμές

### Σημειώσεις για την αναφορά (Κεφ. 3 & 4)
- Να αναφερθεί ότι για το XGBoost χρησιμοποιήθηκε TreeExplainer (γρήγορος, exact)
  και για το FFNN KernelExplainer (αργός, model-agnostic) — η διαφορά αυτή
  είναι μέρος του επιχειρήματος για το κόστος πολυπλοκότητας
- Το FFNN SHAP τρέχει σε 2000 από τις ~2600 εγγραφές του test set —
  να το αναφέρεις ως "αντιπροσωπευτικό δείγμα" και όχι ως πλήρη ανάλυση
- Ο χρόνος εκτέλεσης (~2.5 ώρες vs δευτερόλεπτα για XGBoost) είναι
  από μόνος του επιχείρημα για την ενότητα "κόστος επικύρωσης FFNN"


  
## Notebook 04 — Bias Detection

### Αποτελέσματα Bias
- Υποομάδες: AverageMInFile < 76 μήνες (Α) vs >= 76 μήνες (Β)
- DI ~0.47 και στα τρία μοντέλα — κάτω από το όριο 0.8
- Έγκριση Ομάδα Α: ~30% / Ομάδα Β: ~63%
- Το bias προέρχεται πιθανώς από τα δεδομένα και όχι μόνο από το μοντέλο
- Και τα τρία μοντέλα παρουσιάζουν παρόμοιο επίπεδο bias

### Fairness Mitigation
- Μέθοδος: ExponentiatedGradient με DemographicParity
- DI: 0.47 → 0.93 (βελτίωση +0.46)
- AUC: 0.789 → 0.680 (κόστος -0.109)
- Κεντρικό εύρημα: fairness-accuracy trade-off
- Συμπέρασμα: δεν υπάρχει δωρεάν βελτίωση bias

## Notebook 05 — Monitoring

### Data Drift Simulation
- Μέθοδος: Gaussian Noise σε όλα τα features
- Κύρια επιδείνωση: ExternalRiskEstimate (-0.5 std), NetFractionRevolvingBurden (+0.5 std), AverageMInFile (-0.4 std)
- PSI: 0.2099 → Παρακολούθηση απαιτείται (0.10-0.25)

### CSI Αποτελέσματα
- MaxDelq2PublicRecLast12M: 0.92 — υψηλή αστάθεια (ευαίσθητο feature)
- MaxDelqEver: 0.91 — υψηλή αστάθεια
- NetFractionRevolvingBurden: 0.80 — υψηλή αστάθεια
- ExternalRiskEstimate: 0.49 — μέτρια αστάθεια
- Τα υπόλοιπα: σταθερά (CSI < 0.10)
- Εύρημα: CSI εντοπίζει αστάθεια σε features που δεν αλλάχθηκαν άμεσα

### FFNN Monitoring
- PSI FFNN: 0.2206 vs PSI XGBoost: 0.2099
- Ελαφρώς υψηλότερο PSI για FFNN — πιο ευαίσθητο στο drift
- Επιπλέον: SHAP αργό, δύσκολος εντοπισμός αιτίας αστάθειας
- Συμπέρασμα: FFNN απαιτεί πιο συχνή και πιο κοστοβόρα παρακολούθηση

### FFNN Monitoring — Πλήρης Ανάλυση
- PSI scenarios: Ήπια 0.036 / Μέτρια 0.221 / Σοβαρή 0.653
- Παρόμοιο με XGBoost σε όλα τα σενάρια
- CSI: ακριβώς ίδιο με XGBoost — η αστάθεια είναι στα δεδομένα
- Κεντρικό μήνυμα: CSI εύκολο να υπολογιστεί αλλά αδύνατο να ερμηνευτεί
  χωρίς TreeExplainer για το FFNN