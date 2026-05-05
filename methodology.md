# Multimodal Depression Detection: Final Methodology

This document outlines the exact architecture, models, datasets, and fusion parameters that were evaluated in the final 2,500-combination grid search experiment. You can use this directly in the **Methodology** section of your research paper.

---

## 1. Machine Learning Models Used (10 Total)

The system isolates the data processing streams into two specialized branches before late fusion.

### Tabular (Clinical Questionnaire) Models
We tested 5 distinct algorithms for processing structured, tabular data (like the PHQ-9 questions). We explicitly removed derived features (`total_score`, `severity_label`, `work_interference`) to enforce strict **Ablation & Leakage Prevention**.
1. **XGBoost** (Gradient Boosting Trees, non-linear dependencies)
2. **LightGBM** (High-efficiency gradient boosting)
3. **CatBoost** (Categorical-optimized gradient boosting)
4. **Random Forest** (Ensemble bagging for variance reduction)
5. **Logistic Regression** (Linear baseline for high interpretability)

### Text (Social Media / NLP) Models
We tested 5 distinct algorithms for processing sparse text data and contextual embeddings.
1. **TF-IDF + Logistic Regression** (Sparse vector baseline)
2. **TF-IDF + Support Vector Machine (SVM)** (Maximum margin classification)
3. **TF-IDF + Multinomial Naive Bayes** (Probabilistic baseline)
4. **TF-IDF + XGBoost** (Tree-based sparse matrix learning)
5. **DistilBERT + Logistic Regression** (Deep Learning feature extraction using the `[CLS]` token for dense semantic embeddings).

---

## 2. Datasets Used (9 Total)

To prove true clinical and real-world robustness, we decoupled from a single data source and evaluated across 9 diverse datasets.

### Tabular Datasets (5)
1. **M1**: OSMI Mental Health in Tech Survey 2014
2. **M2**: OSMI Mental Health in Tech Survey 2016
3. **M3**: Student Mental Health Survey
4. **M4**: Depression & Anxiety Questionnaire
5. **M5**: `PHQ9_Noisy` (A synthetically generated, research-grade PHQ-9 dataset infused with Gaussian noise and 5% label flipping to test model robustness).

### Text Datasets (4)
We utilized a multi-platform approach to ensure our models generalized outside of standard Reddit datasets.
* **Disclaimer:** *Emotion datasets are used as proxy signals for affective state modeling and not as direct clinical depression labels.*
1. **S1: Google `go_emotions`** (Sadness/Grief vs. Joy/Optimism)
2. **S2: `DepressionEmo`** (A highly curated multilabel dataset from GitHub; statements containing sadness/hopelessness).
3. **S3: `ShreyaR/DepressionDetection`** (True depression classification labeled natively from Twitter).
4. **S4: `tweet_eval`** (Standardized Twitter emotion baseline dataset).

---

## 3. The 2,500 Evaluation Comparisons

The experiment systematically evaluated every possible combination of Tabular Models, Tabular Datasets, Text Models, and Text Datasets.

**The Math:**
* 25 Tabular Pipelines (5 Models × 5 Datasets)
* 20 Text Pipelines (5 Models × 4 Datasets)
* `25 × 20 = 500` Core Hybrid Combinations

**The 5 Fusion Methods:**
For each of the 500 core combinations, we combined their out-of-fold probability predictions (`Tabular_Prob` and `Text_Prob`) using 5 different strategies:
1. **Static Weight 1:** $\alpha = 0.2$ (Favors text)
2. **Static Weight 2:** $\alpha = 0.3$
3. **Static Weight 3:** $\alpha = 0.4$
4. **Static Weight 4:** $\alpha = 0.5$ (Equal balance)
5. **Learned Fusion (Meta-Model):** A Logistic Regression algorithm trained natively on the outputs of the two base models to dynamically learn the mathematically optimal weighting.

`500 combinations × 5 fusion methods = 2,500 total evaluations.`

### Primary Comparison Metrics
Each of the 2,500 evaluations was ranked against the others using the following clinical metrics:
* **Recall (Primary Metric):** The ability to identify all true positive depression cases (minimizing false negatives, which is crucial for medical screening).
* **AUC-ROC:** The ability to separate the classes across all threshold bands.
* **F1 Score:** The harmonic mean of precision and recall.
* **Expected Calibration Error (ECE):** A measurement proving how mathematically reliable the model's confidence percentages are (e.g., does "80% confidence" actually translate to true positive 80% of the time?).
