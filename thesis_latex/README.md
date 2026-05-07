# Οδηγίες Overleaf — Διπλωματική Εργασία

## Πώς να ανεβάσεις στο Overleaf

1. Πήγαινε στο https://www.overleaf.com
2. Κλίκ "New Project" → "Upload Project"
3. Ανέβασε το ZIP αρχείο thesis_latex.zip
4. Άνοιξε το Project

## ΚΡΙΣΙΜΟ: Αλλαγή Compiler

Το project χρησιμοποιεί XeLaTeX για τα ελληνικά.

Menu (πάνω αριστερά) → Compiler → **XeLaTeX**

Χωρίς αυτό τα ελληνικά ΔΕΝ θα εμφανιστούν σωστά.

## Δομή Αρχείων

```
main.tex                    ← Κεντρικό αρχείο (ΜΗΝ μετονομάσεις)
chapters/
    titlepage.tex           ← Εξώφυλλο
    abstract.tex            ← Περίληψη (γράφεται τελευταία)
    chapter1.tex            ← Εισαγωγή (γράφεται τελευταία)
    chapter2.tex            ← Θεωρητικό Πλαίσιο ← ΤΩΡΑ ΕΔΩΣ ΔΟΥΛΕΥΟΥΜΕ
    chapter3.tex            ← Δεδομένα & Μεθοδολογία
    chapter4.tex            ← Αποτελέσματα
    chapter5.tex            ← Πρόταση Framework
    chapter6.tex            ← Συμπεράσματα
figures/                    ← Εδώ βάζεις εικόνες/γραφήματα (.png, .pdf)
references/
    bibliography.bib        ← Βιβλιογραφία
```

## Πώς να προσθέσεις νέο κείμενο

Άνοιξε το αντίστοιχο chapter*.tex αρχείο και
αντικατέστησε το \textit{[Εκκρεμεί.]} με το κείμενό σου.

## Πώς να προσθέσεις citation

1. Βρες/προσθέσε την πηγή στο bibliography.bib
2. Στο κείμενο γράψε: \citep{key}
   Παράδειγμα: \citep{basel2006}

## Πώς να προσθέσεις εικόνα

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\textwidth]{figures/my_plot.png}
    \caption{Περιγραφή εικόνας}
    \label{fig:my_plot}
\end{figure}
```

## Τι είναι έτοιμο τώρα

- [x] Εξώφυλλο (συμπλήρωσε όνομα, ΑΜ, πόλη)
- [x] Δομή όλων των κεφαλαίων
- [x] Βιβλιογραφία με βασικές πηγές
- [x] Κεφ. 2.1.1 — Ορισμός Πιστωτικού Κινδύνου (ΓΡΑΜΜΕΝΟ)
- [ ] Υπόλοιπες ενότητες — σε εξέλιξη
