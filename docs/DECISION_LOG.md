# Decision Log — G2F Maize Yield Prediction 2026

This document tracks significant technical, methodological, and architectural decisions made during the project. Each entry captures the context, decision, rationale, alternatives considered, and validation evidence.

---

## Decision Template

```markdown
## DEC-XXX — [Short Decision Title]

- **Date:** YYYY-MM-DD
- **Status:** Proposed | Accepted | Superseded
- **Context:** What problem or requirement prompted this decision?
- **Decision:** What was chosen?
- **Rationale:** Why was this option selected?
- **Alternatives Considered:** What other approaches were evaluated and why were they deferred or rejected?
- **Validation Evidence:** Empirical results, verification checks, or references supporting this decision.
```

---

## DEC-001 — Modular Project Directory Layout

- **Date:** 2026-11-26
- **Status:** Accepted
- **Context:** Multi-modal G × E crop yield prediction combines phenotypic, genetic, weather, soil, management, and environmental covariate datasets across 2014–2025. Without strict separation of concerns, notebooks and ad-hoc scripts rapidly cause reproducibility issues and data leakage.
- **Decision:** Adopted a structured ML research layout separating immutable raw data (`data/raw/{training,testing}`), derived data (`data/processed`), modular Python source code (`src/{data,features,models,evaluation}`), configurations (`configs`), experiments (`experiments`), reports (`reports/{figures,metrics}`), submissions (`submissions`), tests (`tests`), notebooks (`notebooks`), and documentation (`docs`).
- **Rationale:** Enforces clear boundaries between raw data, reusable pipeline code, exploratory notebooks, and generated outputs; supports automated testing and clean version control.
- **Alternatives Considered:** Flat script directory or notebook-only workflow — rejected due to poor maintainability, weak testability, and high risk of preprocessing leakage.
- **Validation Evidence:** Directory structure verified against Phase 1 specification with `.gitkeep` placeholders and Python package `__init__.py` files.

---

## DEC-002 — Python 3.11.9 Environment and Minimal Initial Dependency Set

- **Date:** 2026-11-26
- **Status:** Accepted
- **Context:** Phase 1 requires a stable, reproducible Python environment for EDA and baseline ML without premature framework commitments.
- **Decision:** Created `.venv` with Python 3.11.9 and installed only the core initial stack (`numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`, `scikit-learn`, `jupyter`, `ipykernel`, `pytest`, `pyarrow`), recording exact installed direct versions in `requirements.txt`.
- **Rationale:** Python 3.11.9 is installed and verified locally (a stale Windows launcher entry for Python 3.12 had no backing executable), and has mature wheel support across the scientific Python ecosystem. Keeping dependencies minimal avoids bloated environments before EDA informs modeling choices.
- **Alternatives Considered:**
  - Python 3.12 via `py -3.12`: unavailable due to missing executable on disk.
  - Pre-installing gradient boosting / deep learning frameworks (e.g., LightGBM, XGBoost, PyTorch): deferred until Phase 3+ after EDA and validation design.
- **Validation Evidence:** Verified `.venv` interpreter version, direct dependency installation, package imports, and dedicated Jupyter kernel registration (`g2f-maize-2026`).

---

## DEC-003 — Strict Git Exclusion Policy for Data and Generated Artifacts

- **Date:** 2026-11-26
- **Status:** Accepted
- **Context:** Competition datasets, intermediate feature tables, trained models, experiment artifacts, and submission files are large, reproducible from code, or subject to competition data handling rules.
- **Decision:** Configured `.gitignore` to exclude `.venv/`, Python/Jupyter caches, everything inside `data/raw/training/`, `data/raw/testing/`, `data/processed/`, `submissions/`, and `experiments/`, common binary/data/model file formats, secrets, and temporary files, while explicitly preserving `.gitkeep` placeholders.
- **Rationale:** Prevents accidental uploads of competition data to GitHub, keeps Git history lightweight, and preserves expected folder structure on fresh clones.
- **Alternatives Considered:** Tracking processed summaries or raw samples in Git — rejected to maintain a zero-risk policy against uploading competition datasets.
- **Validation Evidence:** Verified `.gitignore` behavior across datasets, caches, secrets, artifacts, and `.gitkeep` placeholders using `git check-ignore -v`.
