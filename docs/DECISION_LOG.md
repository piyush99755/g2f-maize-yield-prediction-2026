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

---

## DEC-004 — Provisional Exclusion of Potentially Outcome-Derived Trait Columns from Direct Inputs

- **Date:** 2026-11-26
- **Status:** Proposed *(Provisional — pending 2026 test-schema verification)*
- **Context:** Phase 2.2 EDA of `1_Training_Trait_Data_2014_2025.csv` identified 10 agronomic/harvest trait columns (`Date_Harvested`, `Grain_Moisture`, `Twt_kg_m3`, `Stand_Count_plants`, `Pollen_DAP_days`, `Silk_DAP_days`, `Plant_Height_cm`, `Ear_Height_cm`, `Root_Lodging_plants`, `Stalk_Lodging_plants`) that are measured during crop growth or at harvest alongside `Yield_Mg_ha`.
- **Decision:** Provisionally exclude all 10 columns from direct prediction inputs until the 2026 testing files and permitted prediction-time inputs are audited. Do not approve any column for modeling solely due to correlation with `Yield_Mg_ha`.
- **Rationale:** Using post-harvest measurements (e.g., `Grain_Moisture`, `Twt_kg_m3`, `Date_Harvested`) or in-season plot phenotypes unavailable in 2026 test inputs introduces severe target/temporal leakage and invalidates local cross-validation.
- **Alternatives Considered:**
  - Permanently discarding these columns prior to Phase 3 — deferred because some traits may later be tested strictly as **auxiliary multi-task training targets** (never as test-time inputs), and exact 2026 test-input availability has not yet been verified.
- **Validation Evidence:** Verified schema of `1_Training_Trait_Data_2014_2025.csv` (`203,185` rows × `26` columns) and EvalAI challenge evaluation details (`https://eval.ai/api/challenges/challenge/2742/`).

---

## DEC-005 — Conservative Data-Sharing & Notebook Output Sanitization Policy

- **Date:** 2026-11-26
- **Status:** Accepted
- **Context:** Official EvalAI and `maizegxeprediction.org/about/` rules require winners to open-source their models and treat submitted predictions as public, but do **not** include an explicit dataset redistribution license authorizing broad third-party redistribution of raw competition files or every derived artifact.
- **Decision:** Keep all raw and processed competition datasets strictly outside Git; clear outputs from any notebook cell displaying raw competition records before committing; commit only non-sensitive aggregate statistics and aggregate visualizations containing zero reconstructable plot records; mark broader dataset redistribution terms as requiring confirmation before external dataset sharing.
- **Rationale:** Avoids assuming that open-access model publication automatically authorizes dataset redistribution while preserving full code and aggregate-analysis reproducibility.
- **Alternatives Considered:**
  - Committing full executed notebook outputs including `df.head(5)` previews — rejected to prevent leaking raw competition records into Git history.
  - Clearing all notebook outputs entirely — rejected where non-sensitive aggregate summaries and aggregate plots aid code review without exposing raw rows.
- **Validation Evidence:** Verified zero raw competition files in `git ls-files`, explicit `.gitignore` coverage, and automated raw-record token scan on committed `notebooks/01_trait_data_eda.ipynb`.

---

## DEC-006 — Retention of Unmodified Raw Observations & Statistical Tail Values Through EDA

- **Date:** 2026-11-26
- **Status:** Accepted *(for Phase 2 EDA; Phase 3 modeling policy pending)*
- **Context:** `1_Training_Trait_Data_2014_2025.csv` contains `56,233` replicated `(Env, Hybrid)` pairs (`132,635` unique pairs; `128,597` with valid yield; `4,038` without valid yield), `10,211` missing plot targets, and `982` IQR-flagged tail yield observations (`0.51%` of `192,974` finite valid yields).
- **Decision:** Retain all original observations untouched throughout Phase 2 without dropping IQR-flagged yields, imputing missing values, or aggregating experimental replicates. Defer plot-level vs. `(Env, Hybrid)`-level modeling and high-missingness environment handling (`SCH1_2024`, `2017`) to Phase 3 after auditing metadata, weather, soil, and evaluation edge cases.
- **Rationale:** Zero exact duplicate rows exist (`(Env, Experiment, Replicate, Block, Plot)` is a unique primary key), and extreme yields in field trials frequently represent legitimate biological/weather stress rather than recording errors.
- **Alternatives Considered:** Immediately filtering IQR outliers or averaging replicates during EDA — rejected as premature preprocessing before inspecting metadata and weather context.
- **Validation Evidence:** Composite key uniqueness audit (`0` collisions), finite numerical check (`0` `+inf`, `0` `-inf`, `0` non-numeric), and full distribution analysis in [docs/TRAIT_DATA_EDA.md](TRAIT_DATA_EDA.md).

