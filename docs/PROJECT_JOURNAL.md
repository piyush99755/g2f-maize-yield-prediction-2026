# Project Journal — G2F Maize Yield Prediction 2026

This journal records chronological development milestones, technical concepts, implementation details, errors encountered, debugging notes, experiment observations, and lessons learned throughout the 2026 G2F Maize Genotype × Environment Prediction Competition.

---

## Entry Template

```markdown
## YYYY-MM-DD — [Milestone / Topic Title]

### Context & Objective
- What is being worked on and why.

### Technical Concepts & Implementation Details
- Key methods, design choices, or code paths introduced.

### Experiments & Observations
- Concrete, verified results (link to `reports/metrics/` or `configs/`).

### Errors, Debugging & Resolutions
- Issues encountered, root causes, and fixes applied.

### Lessons Learned & Next Steps
- Actionable takeaways and next concrete milestone.
```

---

## 2026-11-26 — Phase 1: Project Initialization

### Context & Objective
- Initialized the `g2f-maize-yield-prediction-2026` repository to establish a clean, reproducible engineering foundation before ingesting or inspecting competition datasets.
- Scope restricted strictly to project scaffolding, Python environment setup, `.gitignore` safeguards, documentation, and initial Git/GitHub configuration.

### Technical Concepts & Implementation Details
- **Directory Architecture:** Created modular separation between immutable raw data (`data/raw/training/`, `data/raw/testing/`), derived artifacts (`data/processed/`), reusable source packages (`src/data/`, `src/features/`, `src/models/`, `src/evaluation/`), configurations (`configs/`), experiments (`experiments/`), reporting (`reports/figures/`, `reports/metrics/`), submissions (`submissions/`), tests (`tests/`), notebooks (`notebooks/`), and project documentation (`docs/`).
- **Python Environment:** Provisioned an isolated `.venv` using **Python 3.11.9** with only the core exploration and baseline ML stack (`numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`, `scikit-learn`, `jupyter`, `ipykernel`, `pytest`, `pyarrow`). Deferred deep learning and advanced modeling libraries until justified by EDA and baseline benchmarks.
- **Jupyter Kernel:** Registered a dedicated project kernel (`g2f-maize-2026`) pointing to `.venv`.
- **Git & Data Hygiene:** Configured `.gitignore` to exclude virtual environments, caches, raw/processed datasets, model binaries, submissions, experiment artifacts, secrets, and temporary files while preserving empty directory placeholders via `.gitkeep`.

### Errors, Debugging & Resolutions
- **Stale Python 3.12 launcher entry:** `py -0p` listed Python 3.12 as the default launcher entry, but invoking `py -3.12 --version` failed because `C:\Users\piyus\AppData\Local\Programs\Python\Python312\python.exe` was missing on disk. Resolved by targeting the verified Python 3.11.9 installation (`py -3.11 -m venv .venv`).

### Lessons Learned & Next Steps
- Always verify interpreter binaries directly before building virtual environments rather than relying solely on `py -0p` registry listings.
- **Next Step:** Await approval to begin Phase 2 (Data Ingestion & Exploratory Data Analysis). No data processing, EDA, feature engineering, or model training has been performed.
