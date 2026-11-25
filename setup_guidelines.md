# TrustNewsAI Setup Guidelines

This guide explains how to run the current machine-learning workflow on Windows with PowerShell. The commands use paths relative to the repository, so they work regardless of where you clone it.

## 1. Clone or open the repository

If you have not cloned it yet:

```powershell
git clone https://github.com/kura458/TrustNews_AI.git
cd TrustNews_AI
```

If you already have the repository, open its root folder in VS Code or PowerShell.

## 2. Create and activate the virtual environment

From the repository root:

```powershell
cd ml_services
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks script activation, you can use the environment's Python executable directly for the remaining commands:

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

## 3. Install dependencies

With the environment activated:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

The requirements include the data-processing and ML packages, Jupyter tooling, and Gradio for the local web interface.

## 4. Add the dataset

Download the Kaggle Fake and Real News Dataset. Place the two CSV files here:

```text
ml_services\data\raw\True.csv
ml_services\data\raw\Fake.csv
```

The filenames must match exactly. The dataset is not included in this repository.

## 5. Explore and prepare the data

Run commands from the `ml_services` directory.

Explore the raw dataset:

```powershell
python scripts/explore_dataset.py
```

Prepare the cleaned data and stratified train/test split:

```powershell
python scripts/prepare_dataset.py
```

The preparation script creates:

```text
data/processed/news_clean.csv
data/processed/train.csv
data/processed/test.csv
```

## 6. Train and evaluate the model

After the train/test files have been created:

```powershell
python scripts/train_model.py
```

The script trains a TF-IDF and Logistic Regression pipeline, prints evaluation metrics, and saves the model to:

```text
artifacts/fake_news_pipeline.joblib
```

The reported metrics depend on the dataset and split produced by your run. Do not assume a fixed accuracy before running the evaluation.

## 7. Make predictions

**Command-line interface**

```powershell
python scripts/predict.py
```

Enter an article title and body when prompted. Press Enter at the title prompt to exit.

**Local Gradio interface**

```powershell
python scripts/gradio_app.py
```

Open `http://127.0.0.1:7860` in your browser. Enter an article title and body, then select **Analyze Article**. Stop the server with `Ctrl+C` in the terminal.

Both prediction options require the trained model artifact. Run `train_model.py` first if the model file is missing.

## 8. Troubleshooting

**Python is not recognized:** Install Python and enable the PATH option during installation, then reopen PowerShell.

**A package is missing:** Confirm that the virtual environment is active, then run `python -m pip install -r requirements.txt` from `ml_services/`.

**Dataset files are not found:** Check that `True.csv` and `Fake.csv` are in `ml_services/data/raw/`.

**Train/test files are missing:** Run `prepare_dataset.py` before training.

**Model artifact is missing:** Run `train_model.py` before using the CLI or Gradio interface.

**Port 7860 is already in use:** Stop the other process using that port, or change the port in the Gradio launch configuration.

## Important limitation

This project produces a text-classification estimate based on its training data. A prediction is not proof that a news article is true or false. Verify important claims using reliable independent sources.
