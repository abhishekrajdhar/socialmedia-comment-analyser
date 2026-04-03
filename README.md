# Social Media Comment Analyser

A reproducible end-to-end project for analysing sentiment in YouTube / social media comments. This repository contains data ingestion and preprocessing code, experiments and notebooks, model training and evaluation, an MLflow-backed model registry, a DVC pipeline for reproducibility, a small Flask API, and a Chrome extension frontend for quick demos.

Author: Abhishek Rajdhar Dubey

## Table of contents

- Project overview
- Features
- Quick start
- Project structure
- Usage
    - Run the pipeline (DVC)
    - Train & evaluate locally
    - Run the Flask API
    - Load the Chrome extension (frontend)
- Experiments & notebooks
- Results
- Development notes
- Contributing
- License & contact

## Project overview

This project demonstrates a complete ML workflow for comment sentiment analysis:

- Data ingestion and preprocessing (scripts under `src/data`).
- Model experiments and training (notebooks in `notebook/` and scripts under `src/model`).
- Model evaluation and reporting (including a confusion matrix image in the repo).
- Experiment tracking and model registry using MLflow.
- Reproducible pipeline orchestration using DVC.
- A simple Flask API to serve predictions (`flask_app/app.py`).
- A Chrome extension frontend to interact with the model (`flask_app/frontend`).

The codebase is intended to be reproducible and easy to extend for new experiments or model improvements.

## Features

- End-to-end pipeline: raw data -> preprocessing -> training -> evaluation -> deployable artifact
- DVC pipeline and (optionally) remote storage for data and model versioning
- MLflow for experiment tracking and model registry
- Minimal Flask REST API for serving predictions
- Chrome extension UI for quick demos

## Quick start

1. Clone the repository (if not already):

     git clone <this-repo-url>

2. Create and activate a Python virtual environment (macOS / zsh):

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

3. If you use DVC remotes, pull data and model artifacts (optional, only if a remote is configured):

```bash
dvc pull
```

4. Reproduce the pipeline (this will run the stages defined in `dvc.yaml`):

```bash
dvc repro
```

5. Run the full project orchestration (if `main.py` is the entry point):

```bash
python main.py
```

Notes
- If your environment doesn't have DVC or MLflow configured, steps that touch remote storage or the MLflow server may be skipped or run locally.

## Project structure

Top-level files/folders you will use most:

- `main.py` — Project entrypoint / orchestration script.
- `dvc.yaml`, `dvc.lock` — DVC pipeline definition and lockfile.
- `params.yaml` — Tunable parameters for experiments/pipeline stages.
- `requirements.txt` / `pyproject.toml` / `setup.py` — Dependencies and packaging metadata.
- `src/` — Python package with data, model, and evaluation modules.
    - `src/data` — `data_ingestion.py`, `data_preprocessing.py`.
    - `src/model` — `model_building.py`, `model_evaluation.py`, `register_model.py`.
- `flask_app/` — Small Flask app and frontend (Chrome extension files).
- `notebook/` — Jupyter notebooks used for experiments and exploration.
- `confusion_matrix_Test Data.png` — Example evaluation output.

## Usage

Run only the parts you need — common workflows below.

### 1) Run the DVC pipeline

If you want to reproduce the full data → model pipeline with DVC:

```bash
dvc repro
```

This executes stages defined in `dvc.yaml` in order. Make sure any DVC remotes (if used) are configured and available.

### 2) Train & evaluate locally

You can run the training and evaluation scripts directly. Example (from repository root):

```bash
python src/model/model_building.py
python src/model/model_evaluation.py
```

If the scripts expose CLI args or rely on `params.yaml`, adjust them accordingly.

To view MLflow experiments (if MLflow is used during runs):

```bash
mlflow ui --port 5001
```

### 3) Run the Flask API

Start the small Flask app to serve predictions (default port 5000):

```bash
python flask_app/app.py
```

The API exposes endpoints for sending comment text and receiving a sentiment prediction. Check `flask_app/app.py` for exact routes and payload formats.

### 4) Load the Chrome extension (frontend)

The frontend is a simple Chrome extension under `flask_app/frontend`:

1. Open Chrome and go to `chrome://extensions`.
2. Enable "Developer mode".
3. Click "Load unpacked" and choose `flask_app/frontend`.
4. The extension popup will call the Flask API to get sentiment predictions (ensure the Flask app is running).

## Experiments & notebooks

Notebooks used during exploration and model selection are in the `notebook/` directory. They document experiments, feature engineering ideas, and model comparisons. Use them as a starting point if you want to try different models or preprocessing.

## Results

- See `confusion_matrix_Test Data.png` for an example confusion matrix from evaluation. Additional metrics and MLflow run data (if used) are available in the MLflow UI.

## Development notes

- The project uses DVC to make experiments reproducible. Stages, inputs, outputs, and parameters are in `dvc.yaml` and `params.yaml`.
- MLflow is used for tracking experiments and may require a running MLflow server if you want a central registry.
- The Flask app is intentionally small and synchronous — appropriate for demos but not optimized for production traffic. Consider using a WSGI server (gunicorn) and containerization for production.

Edge cases and assumptions
- The repository assumes you have a working Python 3.8+ environment.
- DVC remotes and MLflow server endpoints are not hard-coded — configure them in your environment or `dvc`/`mlflow` config files if needed.

## Contributing

Contributions are welcome. Suggested small improvements:

- Add unit tests for critical data transforms and model I/O.
- Improve the extension UI and error handling.
- Add CI to run linting, tests, and a minimal pipeline run.

When contributing, open issues and PRs describing the motivation and tests.

## License

This repository uses the MIT License. See the `LICENSE` file (or add one) for details.

## Contact

Author: Abhishek Rajdhar Dubey

If you want help reproducing results or extending the project, open an issue or contact the author directly.

---

