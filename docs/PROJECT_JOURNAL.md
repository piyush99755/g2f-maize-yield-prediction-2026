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
- **Next Step:** Complete remaining Phase 1 verification checks (Jupyter kernel execution, dependency verification, Git safety audit, and hardware assessment) before moving to Phase 2.

---

## 2026-11-26 — Phase 1: Environment Verification & Hardware Assessment

### Context & Objective
- Completed the remaining Phase 1 verification checks to confirm end-to-end Jupyter kernel execution, exact dependency alignment with `requirements.txt`, Git repository safety, and local hardware capacity prior to Phase 2.

### Verification Results

#### 1. Manual VS Code Verification (User-Confirmed)
- Virtual environment activation (`.venv`): **PASSED**
- Python `3.11.9`: **PASSED**
- NumPy, Pandas, Scikit-learn, and SciPy imports: **PASSED**
- Git status: clean and synchronized with `origin/main`
- `pip check`: No broken requirements found

#### 2. Jupyter Execution Smoke Test (`g2f-maize-2026` Kernel)
- Created a temporary synthetic notebook (`notebooks/_tmp_phase1_smoke_test.ipynb`) and executed all 4 cells programmatically via `nbclient` using the registered `g2f-maize-2026` kernel.
- **Active Kernel Interpreter:** `C:\Users\piyus\Downloads\g2f-maize-yield-prediction-2026\.venv\Scripts\python.exe` (exact match with project `.venv`).
- **Python Version:** `3.11.9` (`MSC v.1938 64 bit (AMD64)`).
- **Verified Imports Inside Kernel:** `numpy`, `pandas`, `scipy`, `sklearn`, `matplotlib`, `pyarrow`.
- **Synthetic Computation:** Built a 5-element NumPy array (`[10.5, 11.2, 12.8, 9.5, 11.0]`) and Pandas DataFrame; computed verified mean `11.00`.
- **Plot Rendering:** Generated and rendered a Matplotlib figure to an in-memory PNG buffer (`10,233 bytes`, valid PNG header).
- **Cleanup:** Deleted the temporary notebook immediately after verification; `notebooks/` contains only `.gitkeep`.

#### 3. Dependency Verification (`requirements.txt` vs. `.venv`)
- Verified via `importlib.metadata` that all 10 direct dependencies installed in `.venv` match `requirements.txt` exactly:
  - `numpy==2.4.6`, `pandas==3.1.1`, `scipy==1.17.1`, `pyarrow==26.0.1`
  - `matplotlib==3.11.2`, `seaborn==0.13.2`, `scikit-learn==1.9.1`
  - `jupyter==1.1.1`, `ipykernel==7.4.0`, `pytest==9.2.0`

#### 4. Git Safety Audit
- Audited all 20 tracked files via `git ls-files`: confirmed zero raw datasets, processed datasets, credentials, virtual environment files, or model binaries are tracked.
- Ran `git fetch origin`: confirmed local `HEAD` (`31c489baed6406bb432c86ec4e80337db7895c3c`) matched `origin/main` (`31c489baed6406bb432c86ec4e80337db7895c3c`).
- Audited `.gitignore` via `git check-ignore -v --no-index`: confirmed protection for `data/raw/training/*`, `data/raw/testing/*`, `data/processed/*`, `.venv/`, model binaries (`*.pkl`), `submissions/*`, `experiments/*`, `.env`, and `secrets.*`.

#### 5. Hardware Specifications
- **Operating System:** Microsoft Windows 11 Home, Version `10.0.26200` (Build `26200`), 64-bit
- **CPU:** Intel(R) Core(TM) i7-9750H CPU @ 2.60GHz — **6 physical cores / 12 logical processors**
- **System RAM:** **15.85 GB** total (`2.63 GB` available at measurement time)
- **GPU (Dedicated):** **NVIDIA GeForce GTX 1660 Ti** — **6,144 MiB (6 GB) dedicated VRAM** (`5,956 MiB` free via `nvidia-smi`; driver `616.92`)
- **GPU (Integrated):** Intel(R) UHD Graphics 630 (driver `30.0.101.1660`)
- **NVIDIA CUDA Availability:** NVIDIA driver (`616.92`) reports **CUDA UMD Version 13.4** via `nvidia-smi`. No standalone local CUDA Toolkit (`nvcc` not found on `PATH`; `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA` not present).
- **Project Drive (`C:`, NTFS):** **464.04 GB** total, **~21.07–21.90 GB** available free space at measurement time.

### Errors, Debugging & Resolutions
- **PowerShell inline quoting in smoke-test script:** First inline `-c` invocation failed because PowerShell interpreted an escaped single quote inside a double-quoted string (`PNG : The term 'PNG' is not recognized...`). Resolved cleanly by passing the smoke-test script via a single-quoted PowerShell here-string (`@' ... '@ | .\.venv\Scripts\python.exe -`) with zero shell interpolation.
- **Benign Windows ZMQ/IPKernel warnings during programmatic `nbclient` execution:** `zmq` emitted a `RuntimeWarning` regarding Windows `ProactorEventLoop` (handled automatically via Tornado's selector thread) and `IPKernelApp` logged a local TCP transport warning. All 4 notebook cells executed without errors and passed all assertions.
- **WMI 32-bit VRAM cap:** `Win32_VideoController.AdapterRAM` reported `4 GB` for the GTX 1660 Ti due to Windows WMI's 32-bit unsigned integer limit; verified true dedicated VRAM (`6,144 MiB`) directly with `nvidia-smi`.

### Lessons Learned & Next Steps
- **Resource Planning Note:** With `15.85 GB` system RAM (`2.63 GB` currently free), `6 GB` GPU VRAM, and `~21 GB` free disk space on `C:`, future Phase 2+ workflows should use memory-efficient formats (Parquet/PyArrow, compact numeric dtypes, chunked genomic loading), monitor disk usage carefully before caching large datasets or pretrained checkpoints, and close unnecessary background applications prior to heavy runs.
- **Next Step:** Phase 1 verification is complete. Proceed to Phase 2.1 (Training Dataset Inventory) upon approval.

---

## 2026-11-26 — Phase 2.1: Training Dataset Inventory

### Context & Objective
- Performed a strictly read-only filesystem metadata inventory of `data/raw/training/` after the raw competition training folder was copied locally.
- Verified file counts, sizes, nested paths, compression states, likely dataset categories (distinguishing verified metadata from filename assumptions), hardware/memory/storage constraints, recommended loading strategies, and Git ignore safety.

### Key Findings (Verified Filesystem Metadata)
- **Location:** All 9 dataset files reside inside `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/`.
- **Total Files & Storage:** **9 files** totaling **571.07 MB (MiB)** / **0.5577 GB (GiB)** (`598,808,324 bytes`; `598.81 MB` SI).
  - Tabular `.csv` files (6 files): `53.96 MB` combined — Trait (`35.54 MB`), Weather full-year (`11.64 MB`), Weather seasons-only (`6.10 MB`), EC (`0.54 MB`), Metadata (`0.11 MB`), Soil (`0.03 MB`).
  - Genotype files (3 files): `517.11 MB` combined (~90.6% of total) — `2kSites.vcf` (`58.28 MB`), `2kSites_numeric.txt` (`41.38 MB`), and gzip-compressed `400kSites.vcf.gz` (`417.45 MB`).
- **Documentation Files:** None present inside `data/raw/training/`.
- **Updated Free Disk Space (`C:`):** `20.49 GB` available (`464.04 GB` total).
- **Full Inventory Document:** Recorded in [docs/DATASET_INVENTORY.md](DATASET_INVENTORY.md).

### Hardware Risks & Notable Filename Observations
- **Primary Memory/Storage Risk:** `5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` (`417.45 MB` compressed) must **not** be extracted to uncompressed `.vcf` on disk (given only `20.49 GB` free on `C:`) and must **not** be loaded as a dense `float64`/string DataFrame into RAM (`15.85 GB` total; `2.63 GB` free during Phase 1 check). It requires chunked streaming directly from `.vcf.gz` with `int8`/`uint8` encoding.
- **Filename Observations to Verify in Phase 2.2:**
  - Soil filename covers `2015_2025` (omitting `2014`).
  - Two weather datasets exist (`full_year` vs. `seasons_only`).
  - Genotype filenames specify `All_2014_2027_hybrids`, requiring strict leakage safeguards when fitting any genotype preprocessing or dimensionality reduction.

### Git Safety Verification
- Verified all 9 dataset files individually via `git check-ignore -q --no-index` (`9/9` ignored).
- Verified `git status -uall -s` reports zero untracked or staged training data files.

### Errors, Debugging & Resolutions
- None encountered during Phase 2.1.

### Lessons Learned & Next Steps
- Separating the 2k-site numeric genotype panel (`41.38 MB`) from the 400k-site `.vcf.gz` panel (`417.45 MB`) gives a practical two-tier path: rapid low-memory baselines on `2kSites_numeric.txt`, followed by chunked streaming representations on `400kSites.vcf.gz` once baseline CV is established.
- **Next Step:** Proceed to Phase 2.2 (Phenotype and Trait Data EDA) upon approval.

---

## 2026-11-26 — Phase 2.2: Phenotype & Trait Data EDA

### Context & Objective
- Performed read-only exploratory data analysis exclusively on `1_Training_Trait_Data_2014_2025.csv` via [notebooks/01_trait_data_eda.ipynb](../notebooks/01_trait_data_eda.ipynb) using the `g2f-maize-2026` kernel.
- Documented complete aggregate findings, leakage risks, and open questions in [docs/TRAIT_DATA_EDA.md](TRAIT_DATA_EDA.md) and saved 5 non-sensitive aggregate plots under `reports/figures/`.

### Key Empirical Findings
- **Scale & Loading:** `203,185` observations × `26` columns; loaded with Pandas C engine (`low_memory=False`) using `58.65 MB` deep RAM.
- **Official EvalAI Target & Metric Verification:** Verified from `https://eval.ai/api/challenges/challenge/2742/` and `https://www.maizegxeprediction.org/about/` that `Yield_Mg_ha` is absolute grain yield in `Mg/ha` adjusted to `15.5%` moisture, and the competition metric is **average within-environment Pearson correlation ($r$)** (missing submission values disallowed; organizers may drop failed/unusual test environments or hybrids). Unspecified metric edge cases (environment weighting, constant predictions, minimum hybrids per environment, undefined Pearson $r$ handling) are explicitly marked **unresolved**.
- **Target (`Yield_Mg_ha`):** `192,974` valid finite numerical labels (`10,211` / `5.03%` `NaN`, `0` non-numeric, `0` `+inf`, `0` `-inf`, `0` zero, `0` negative), range `0.4282–23.2680 Mg/ha`, mean `9.7504`, median `9.9179`, SD `3.1278`, IQR `4.2209` (`7.7062–11.9271`), skewness `-0.2164`, `982` IQR-flagged tail observations (`0.51%`, retained untouched).
- **Missingness Patterns:** Highest missingness in `Root_Lodging_plants` (`35.13%`), `Stalk_Lodging_plants` (`28.14%`), `Silk_DAP_days` (`23.47%`), `Pollen_DAP_days` (`22.26%`), and `Twt_kg_m3` (`19.44%`). Missing target rate peaked in `2017` (`17.41%`) and environment `SCH1_2024` (`78.40%`), with **`0` environments** having entirely missing yield. Raw CSV missing tokens are strictly `"NA"` or `""`; `Plot == 999` (`57` rows) is a legitimate sequential plot number, not a sentinel code.
- **Granularity & Replicates:** `0` exact duplicate rows; `(Env, Experiment, Replicate, Block, Plot)` uniquely identifies all `203,185` field plots. Across `321` environments and `6,203` hybrids, there are **`132,635` unique `(Env, Hybrid)` pairs**: **`56,233`** (`42.40%`) with multiple observations (genuine experimental replicates), **`128,597`** (`96.96%`) with valid yield, and **`4,038`** (`3.04%`) without valid yield.
- **Temporal & Plot-Weighted Variance Structure:** Complete `2014–2025` coverage (24–30 environments/year). Plot-weighted variance decomposition across `192,974` valid plots yields **`57.37%` between-environment** and **`42.63%` within-environment** variation (note: within-environment variation combines genetic main effects, G × E interaction, field spatial heterogeneity, and plot-level residual noise — it must not be equated solely with genetic variance).
- **Provisional Leakage Safeguard:** Ten co-measured harvest/in-season trait columns (`Date_Harvested`, `Grain_Moisture`, `Twt_kg_m3`, `Stand_Count_plants`, `Pollen_DAP_days`, `Silk_DAP_days`, `Plant_Height_cm`, `Ear_Height_cm`, `Root_Lodging_plants`, `Stalk_Lodging_plants`) are **provisionally excluded from direct prediction inputs** pending verification of the 2026 test schema and permitted prediction-time information (logged in `DEC-004`).

### Errors, Debugging & Quality-Review Corrections
- **Windows CLI command-length limit:** Passing the full notebook-generation script inline via PowerShell triggered `The filename or extension is too long`. Resolved by using an external scratch script executed via `.venv\Scripts\python.exe`.
- **Matplotlib 3.11 deprecation warning:** Replaced `boxplot(..., vert=True)` with `orientation="vertical"`.
- **Phase 2.2 Quality Review Refinements:**
  1. Verified redundant `hybrid_yield` aggregation is absent from the committed notebook (only `hyb_meta` is used).
  2. Replaced hardcoded counts/year ranges in plot titles and Section 8 notebook summaries with live dynamically computed values (added dedicated dynamic summary code cell).
  3. Added explicit finite-value filtering (`np.isfinite`) and explicit `+inf` (`0`) / `-inf` (`0`) reporting for `Yield_Mg_ha`.
  4. Refined variance decomposition wording to **plot-weighted between-environment and within-environment variation**, explicitly noting that within-environment variance includes genetic, G × E, spatial, and plot-level residual noise.
  5. Documented official competition evaluation rules and explicitly marked unspecified Pearson $r$ edge cases (weighting, constant predictions, minimum hybrids, undefined $r$) as **unresolved**.
  6. Clarified actual EvalAI / `maizegxeprediction.org` data-sharing terms (open-access model publication does **not** automatically authorize broad dataset redistribution; logged conservative policy in `DEC-005`).
  7. Softened leakage exclusion of the 10 potentially outcome-derived traits to **provisional exclusion pending 2026 test-schema verification** (`DEC-004`).
  8. Re-executed `notebooks/01_trait_data_eda.ipynb` from a fresh `g2f-maize-2026` kernel (`20/20` code cells passed, `0` errors/warnings), cleared the 2 raw-record preview cell outputs, and verified zero raw plot records appear in the committed `.ipynb`.

### Lessons Learned & Next Steps
- Distinguish rigorously between plot-weighted within-environment variation and true genetic variance, and verify every competition rule (metric edge cases, data redistribution licensing, test schema availability) rather than extrapolating beyond official text.
- **Next Step:** Proceed to Phase 2.3 (Training Metadata EDA) upon approval.

---

## 2026-11-27 — Phase 2.3: Training Metadata EDA (Workflow & Engineering Log)

> **Data-Sharing Guardrail (`DEC-007`):** Per updated Phase 2.3 policy, all data-derived statistics, counts, raw-record previews, and visualizations from the training metadata dataset are stored strictly locally (`docs/METADATA_EDA.md` and `reports/figures/local/`, both excluded via `.gitignore`) pending explicit clarification of competition data-sharing permissions. This committed journal entry records methodology, reproducibility checks, leakage-review structure, and unresolved technical questions only.

### Context & Objective
- Built and verified [notebooks/02_metadata_eda.ipynb](../notebooks/02_metadata_eda.ipynb) to perform strictly read-only exploratory data analysis on the raw training metadata table (`2_Training_Meta_Data_2014_2025.csv`) and a read-only environment-key relationship audit against the previously inspected trait dataset (`1_Training_Trait_Data_2014_2025.csv`).
- Preserved all original raw files, avoided cleaning/imputation/merging/feature engineering/modeling, and did not inspect soil, weather, genotype, or environmental covariate datasets.

### Methodology & Implementation Details
- **Reproducible Loading & Dynamic Computation:** Resolved all dataset paths relative to the repository root, inspected headers and sample rows with Pandas' C engine prior to full load, and derived all summary metrics and visualization titles dynamically at runtime.
- **Schema, Sentinel & Parsing Audit:** Implemented dual-layer missingness inspection (both default-parsed dtypes and raw `keep_default_na=False` token scanning) plus finite/infinity validation on numeric columns — specifically designed to detect default Pandas NA-coercion traps (such as literal `"None"` in agronomic text fields) without altering raw values.
- **Granularity, Geographical & Management Audits:** Audited primary-key uniqueness on `Env`, identifier consistency across `Env`/`Year`/`Experiment_Code`/`City`, coordinate bounds and weather-station-vs-field-centroid Haversine diagnostics, cross-year location coordinate consistency, available date fields (confirming metadata date-column scope vs. trait-table planting/harvest/plot-area scope), diagnostic station-deployment durations, and free-text management/comment heterogeneity.
- **Read-Only Cross-Dataset Relationship Audit:** Audited `Env` overlap and many-to-one join cardinality using only required identifier and target columns from the trait table without creating or persisting any merged dataset.
- **Preliminary Eligibility & Leakage Taxonomy:** Classified all metadata columns across six categories (identifiers, potential geographical features, potential temporal features, potential management features, provisionally excluded post-harvest/outcome-derived/unavailable fields, and unknown fields requiring investigation), deferring final feature eligibility to 2026 test-schema verification (`DEC-008`).

### Errors, Debugging & Resolutions
- **Pandas deprecation warning (`Series.dt.dayofyear`):** Initial fresh-kernel execution emitted a `Pandas4Warning` for `.dt.dayofyear`. Replaced with `.dt.day_of_year` and re-executed cleanly from a fresh `g2f-maize-2026` kernel with zero errors and zero warnings.
- **Notebook output stripping & Git hygiene:** Verified all code cells executed cleanly from a fresh kernel, archived complete data-derived findings and visualizations locally (`docs/METADATA_EDA.md`, `reports/figures/local/`), updated `.gitignore` to exclude local data-derived outputs, and stripped all cell outputs and execution counts from `notebooks/02_metadata_eda.ipynb` before committing.

### Unresolved Technical Questions (Tracked for Phase 2.4+ / Phase 3)
1. **Unmatched Environment Coverage:** Verify whether environments missing from metadata appear in weather, soil, or EC datasets before defining Phase 3 left-join fallback policies.
2. **2026 Test Metadata Schema:** Confirm which geographical, temporal, and management fields are populated and consistently defined in 2026 testing inputs.
3. **Coordinate & Date Quality Exceptions:** Investigate large weather-station-to-field-centroid discrepancies, cross-year coordinate spread within shared `Experiment_Code`s, asymmetric corner missingness, and station date-year mismatches before Phase 3 preprocessing.
4. **Custom NA-Parsing Rules:** Design explicit per-column `na_values` handling in Phase 3 so meaningful agronomic values (e.g., `"None"` indicating no tillage) are not silently coerced to missing `NaN`.
5. **Competition Data-Sharing Terms:** Retain data-derived statistics, notebook outputs, and visualizations locally until explicit data-sharing permissions are confirmed.

### Next Step
- Await user approval before proceeding to **Phase 2.4**.





