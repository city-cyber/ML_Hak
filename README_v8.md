# V8 reproducibility

1. Put these files in one directory:
   - PROJECT_phishing_v8_full5fold.ipynb
   - train.csv
   - test.csv
   - sample_submission.csv

2. Recommended environment:
   - Python 3.14.0
   - pandas 2.3.3

3. Install:
   pip install -r requirements_v8_py314.txt

4. Open the notebook and run all cells from top to bottom.

The notebook does not read intermediate CSV files. It creates only final artifacts:
- submission_v8_stack.csv
- submission_v8_textcat_stable.csv
- best_phishing_model_v8_stack.pkl
- phishing_model_v8_textcat_stable.pkl

Validation design:
- 5-fold Stratified OOF.
- TF-IDF IDF is fit only inside each train fold.
- CatBoost early stopping uses only that fold's validation.
- Similarity/reputation features are computed only from the train part of each fold.
- Thresholds are selected from pooled OOF predictions.
- test.csv is prediction-only.
- exact train/test URL lookup is applied only after model inference.

Private selection:
Use two diverse stable solutions rather than two files selected only by public leaderboard position.
