# 🌾 AgroSmart AI

![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Last Updated](https://img.shields.io/badge/last%20updated-2026--05--13-gold.svg)

**AgroSmart AI** is a production-style machine learning platform for **crop recommendation** (multiclass classification) and **yield prediction** (regression) from soil chemistry and climate inputs. It ships with a **Flask** web UI, a **Streamlit** analytics dashboard, centralized **YAML** configuration, structured **logging**, and a full **pytest** suite.

---

## Features

- Five crop classifiers benchmarked (Random Forest, XGBoost, SVM, k-NN, Gradient Boosting) with stratified CV and hyperparameter search on RF / XGBoost.
- Five yield regressors benchmarked (Random Forest, XGBoost, Gradient Boosting, Ridge, SVR) with K-fold CV and search on RF / XGBoost.
- Persisted **best** models under `artifacts/models/` plus rich `*_metadata.json` for explainability and the `/model-info` API.
- **Flask** app factory, validated `/predict`, `/get_defaults`, `/model-info`, `/health`.
- **Streamlit** dashboard with Predict, Model Comparison, Data Explorer, and About tabs.
- **Professional web UI** (forest green / cream / gold palette, DM Sans + Playfair Display).

---

## Model performance (after training)

Training writes metrics for **all** candidate models. The **best** crop model is selected by **macro F1** on the hold-out set; the best yield model by **R²**.

| Crop classifiers (example schema) | Accuracy | F1 macro | ROC-AUC macro |
|------------------------------------|----------|----------|---------------|
| RandomForest | from `crop_metadata.json` | … | … |
| XGBoost | … | … | … |
| SVM | … | … | … |
| KNN | … | … | … |
| GradientBoosting | … | … | … |

| Yield regressors | R² | MAE (hg/ha) | RMSE |
|------------------|----|-------------|------|
| RandomForest | … | … | … |
| XGBoost | … | … | … |
| … | … | … | … |

> Run `python src/training/train_crop.py` and `python src/training/train_yield.py`, then open `artifacts/models/crop_metadata.json` and `yield_metadata.json` for the exact numbers used in your README tables and UI.

---

## Tech stack

| Layer | Technology |
|-------|------------|
| Language | Python 3.10+ |
| ML | scikit-learn, XGBoost |
| API | Flask 3 |
| UI | HTML/CSS/JS, Streamlit |
| Config | PyYAML |
| Logging | loguru |
| Tests | pytest |

---

## Quick start

```bash
git clone <your-repo-url>
cd agrosmart-ai
python -m venv .venv
.venv\Scripts\activate          # Windows
pip install -r requirements.txt
python src/training/train_crop.py
python src/training/train_yield.py
python app/main.py
```

- Web UI: **http://127.0.0.1:5000**
- Streamlit: `streamlit run dashboard/streamlit_app.py`

Copy `.env.example` → `.env` if you need local overrides.

---

## Project structure (abbreviated)

```
agrosmart-ai/
├── app/                 # Flask UI + predictor
├── artifacts/models/    # crop_model.pkl, yield_model.pkl, *_metadata.json
├── artifacts/reports/   # comparison charts, confusion matrix, feature importance
├── config/config.yaml   # paths, validation ranges, country defaults
├── dashboard/           # Streamlit app
├── data/raw/            # CSV datasets (do not delete)
├── notebooks/           # EDA + model comparison notebooks
├── src/                 # preprocessing, models, training, evaluation, utils
├── tests/               # pytest
├── Makefile
└── requirements.txt
```

---

## API reference

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Web UI |
| `/health` | GET | `{"status":"ok"}` |
| `/model-info` | GET | Crop + yield training metadata JSON |
| `/get_defaults` | POST | JSON `{country}` → rainfall/temperature/pesticides |
| `/predict` | POST | Full validated prediction JSON |

---

## Running tests

```bash
pytest tests/ -v
```

---

## Contributing

Issues and pull requests are welcome. Please keep changes focused, add/adjust tests, and run `pytest` before submitting.

---

## License

MIT — suitable for academic submission; retain attribution in coursework.

---

## Legacy project

The previous `crop-recommendation-system/` folder remains alongside this rebuild for reference. **This** directory (`agrosmart-ai/`) is the canonical codebase going forward.
