# TrustNewsAI — Fake News Classification

TrustNewsAI is a Python machine-learning project for classifying a news article's text as **REAL** or **FAKE**. It includes dataset preparation, a TF-IDF and Logistic Regression training workflow, command-line prediction, sample prediction tests, and a Gradio interface.

> **Project status:** The repository contains a local ML workflow. A trained model artifact is required before prediction or the Gradio interface can run. No deployed service is documented here.

## Features

- Explore the Kaggle Fake and Real News dataset.
- Clean and combine article titles and body text.
- Train a text-classification pipeline using TF-IDF and Logistic Regression.
- Save and load the trained pipeline with Joblib.
- Predict from the command line.
- Launch a Gradio interface for interactive local predictions.
- Run sample prediction checks.

## Project structure

```text
TrustNews_AI/
├── ml_services/
│   ├── scripts/
│   │   ├── explore_dataset.py
│   │   ├── prepare_dataset.py
│   │   ├── train_model.py
│   │   ├── predict.py
│   │   ├── test_predict.py
│   │   └── gradio_app.py
│   ├── src/trustnews_ml/     Reusable text preprocessing
│   ├── data/raw/             Input CSV files
│   ├── data/processed/       Prepared dataset
│   ├── artifacts/            Saved model pipeline
│   └── requirements.txt
├── docs/
├── setup_guidelines.md
├── done.md
└── main.py                   Separate algorithm practice file
```

## Requirements

- Python 3.10 or later (use a version compatible with the pinned dependencies)
- Windows PowerShell is used in the setup examples

## 1. Create the environment

From the repository root:

```powershell
cd ml_services
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## 2. Add the dataset

Download the Kaggle Fake and Real News dataset and put these files in:

```text
ml_services/data/raw/True.csv
ml_services/data/raw/Fake.csv
```

The scripts expect these exact filenames.

## 3. Explore and prepare the data

Run from `ml_services/`:

```powershell
python scripts/explore_dataset.py
python scripts/prepare_dataset.py
```

The preparation workflow cleans text, combines title and body, assigns REAL/FAKE labels, filters short or duplicate content, and performs a stratified 80/20 split. Check the script output and the ML service notes for the actual output files.

## 4. Train the model

```powershell
python scripts/train_model.py
```

The training script builds a TF-IDF and Logistic Regression pipeline and saves the model under `ml_services/artifacts/fake_news_pipeline.joblib`.

## 5. Make a command-line prediction

```powershell
python scripts/predict.py
```

Enter an article title and article text when prompted.

## 6. Launch the Gradio interface

```powershell
python scripts/gradio_app.py
```

Open the local URL printed by Gradio. The saved model must exist before launching the interface.

## 7. Run sample prediction checks

```powershell
python scripts/test_predict.py
```

These checks use example article text and exercise the saved model's prediction path. They are not a substitute for a held-out evaluation.

## Data and model limitations

- The workflow depends on the external Kaggle dataset files, which are not included in the repository.
- Model confidence is not a guarantee of factual accuracy.
- Dataset patterns may not generalize to current events, other languages, or unseen publishers.
- Verify important claims with reliable independent sources; do not use this classifier as the sole basis for judging a story.

## Further improvements

- Record reproducible evaluation metrics on a held-out test set.
- Add cross-validation and error analysis.
- Test for data leakage and source-specific bias.
- Add automated unit tests and input validation.
- Keep datasets and generated model artifacts out of Git unless there is a deliberate release plan.
