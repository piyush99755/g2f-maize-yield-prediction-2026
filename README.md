# G2F Maize Yield Prediction 2026

A reproducible machine learning research repository for the **2026 Genomes to Fields (G2F) Maize Genotype × Environment Prediction Competition**.

## Competition Overview

The **Genomes to Fields (G2F) Initiative** organizes a multi-environment maize prediction challenge focused on forecasting hybrid crop performance under real-world Genotype × Environment (G × E) interactions.

- **Competition:** 2026 Genomes to Fields (G2F) Maize Genotype × Environment Prediction Competition
- **Official Challenge Page:** https://eval.ai/web/challenges/challenge-page/2742/overview

## Project Objective

Build a disciplined, leakage-free, and reproducible machine learning pipeline to predict maize grain yield for **2026 hybrid–environment combinations** using multi-year historical data spanning **2014–2025** alongside genetic, weather, soil, management, and environmental covariate data.

## Prediction Target

- **Target Variable:** `Yield_Mg_ha` (maize grain yield in megagrams per hectare, Mg/ha).
- **Evaluation Protocol:** Official competition evaluation metrics and submission formatting rules will be verified directly from the competition documentation and testing template in Phase 2 before any model evaluation is implemented.

## Historical Training Period

- **Historical Training Data:** 2014–2025 field seasons.
- **Forecast Target Season:** 2026 hybrid–environment combinations.

## Expected Data Sources

Competition datasets will be placed locally under `data/raw/` (excluded from version control) and are expected to encompass:

- Historical phenotypic / trait records (2014–2025), including the target `Yield_Mg_ha`
- Genetic / genomic information for maize hybrids and parental lines
- Weather observations across growing seasons and locations
- Soil properties and field-level measurements
- Agronomic management metadata
- Environmental covariates (EC)

> **Note:** Raw and processed competition datasets are never committed to GitHub. Place competition files locally in `data/raw/training/` and `data/raw/testing/`.

## Project Structure

```text
g2f-maize-yield-prediction-2026/
├── data/
│   ├── raw/
│   │   ├── training/       # Immutable raw training datasets (gitignored)
│   │   └── testing/        # Immutable raw testing datasets (gitignored)
│   └── processed/          # Derived datasets and feature tables (gitignored)
├── notebooks/              # Exploratory notebooks and analysis walkthroughs
├── src/                    # Reusable, modular Python package
│   ├── __init__.py
│   ├── data/               # Data ingestion, validation, and preprocessing
│   ├── features/           # Feature engineering modules
│   ├── models/             # Model training and inference pipelines
│   └── evaluation/         # Cross-validation and metric evaluation
├── configs/                # Experiment and pipeline configuration files
├── experiments/            # Generated experiment logs and outputs (gitignored)
├── reports/
│   ├── figures/            # Publication-quality plots and diagnostics
│   └── metrics/            # Structured validation metric summaries
├── submissions/            # Generated competition submission files (gitignored)
├── tests/                  # Automated unit and data-integrity tests
├── docs/
│   ├── PROJECT_JOURNAL.md  # Chronological engineering and research log
│   └── DECISION_LOG.md     # Technical decision records (ADRs)
├── README.md
├── requirements.txt
└── .gitignore
```

## Environment Setup

This project uses an isolated local Python virtual environment (`.venv`) running **Python 3.11** (specifically initialized on Python 3.11.9).

### 1. Create and Activate the Virtual Environment

**Windows (PowerShell):**

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3.11 -m venv .venv
source .venv/bin/activate
```

### 2. Install Dependencies

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Register the Project Jupyter Kernel

```powershell
python -m ipykernel install --user --name g2f-maize-2026 --display-name "Python (g2f-maize-2026)"
```

### 4. Verify the Installation

```powershell
python -c "import numpy, pandas, scipy, matplotlib, seaborn, sklearn, jupyter, ipykernel, pytest, pyarrow; print('All core packages imported successfully.')"
```

## Development Roadmap

1. **Phase 1 — Project Initialization** *(Completed)*
   - Repository structure, `.gitignore`, isolated Python environment, Jupyter kernel, documentation, and initial Git/GitHub setup.
2. **Phase 2 — Data Ingestion & Exploratory Data Analysis** *(Pending approval)*
   - Inspect raw competition schemas, missingness, temporal coverage (2014–2025 vs. 2026), hybrid/environment overlap, target distribution, and official evaluation rules.
3. **Phase 3 — Validation Strategy & Baseline Pipeline** *(Planned)*
   - Design leakage-safe validation splits aligned with 2026 forecasting, implement official metric computation, and build reproducible baseline models.
4. **Phase 4 — Feature Engineering & Multimodal Integration** *(Planned)*
   - Integrate genetic representations, weather summaries, soil variables, management metadata, and environmental covariates inside fold-safe pipelines.
5. **Phase 5 — Iterative Modeling, Ensembling & Error Analysis** *(Planned)*
   - Explore diverse model families, perform systematic ablation studies, diagnose per-environment and per-hybrid errors, and evaluate ensembling.
6. **Phase 6 — Final Verification & Submission Packaging** *(Planned)*
   - End-to-end reproducibility checks, submission integrity validation, and final reporting.

## Reproducibility Principles

- **Environment Isolation:** All dependencies are installed in `.venv` and pinned explicitly in `requirements.txt`.
- **Immutable Raw Data:** Files under `data/raw/` are treated as strictly read-only; all transformations live in versioned scripts under `src/`.
- **Deterministic Execution:** Random seeds, split definitions, and preprocessing steps will be fixed and recorded in `configs/` and `docs/PROJECT_JOURNAL.md`.
- **Modular Code Over Ad-Hoc Notebooks:** Notebooks are reserved for exploration and visualization; production data, feature, training, and evaluation logic belongs in `src/` with tests in `tests/`.
- **Traceable Decisions:** Significant modeling, validation, and engineering choices are documented in `docs/DECISION_LOG.md` with rationale and validation evidence.

## Data Leakage Prevention Principles

- **Temporal Integrity:** Because the task predicts 2026 outcomes from 2014–2025 historical seasons, validation schemes and feature pipelines must respect year boundaries and prohibit future information from leaking into past folds.
- **Strict Train/Test Isolation:** Testing data in `data/raw/testing/` must never be used to fit scalers, imputers, encoders, dimensionality reduction, or feature selection steps.
- **Fold-Local Preprocessing:** Every data-dependent transformation (imputation, normalization, target encoding, PCA/kinship projections, feature selection) must be fit exclusively on each training fold and applied to the corresponding validation fold.
- **Target & Post-Harvest Leakage Audits:** Candidate predictors will be audited during EDA to ensure no post-harvest measurements or target-derived quantities unavailable at prediction time are used as model inputs.
- **Group Awareness:** Validation design will account for shared environments, locations, years, and hybrids to avoid optimistic leakage across correlated plots or replicates.

## Current Project Status

- **Phase:** Phase 1 — Project Initialization (Complete)
- **Data Processing / EDA:** Not started
- **Feature Engineering / Model Training:** Not started
- **Next Milestone:** Awaiting approval to proceed to Phase 2 (Data Ingestion & Exploratory Data Analysis).
