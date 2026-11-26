# Exploratory Data Analysis — Training Phenotype & Trait Data (`2014–2025`)

- **Phase:** Phase 2.2 — Phenotype and Trait Data Exploratory Data Analysis
- **Analysis Date:** 2026-11-26
- **Source File:** `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/1_Training_Trait_Data_2014_2025.csv`
- **Reproducible Notebook:** [notebooks/01_trait_data_eda.ipynb](../notebooks/01_trait_data_eda.ipynb) (executed from a fresh `g2f-maize-2026` kernel; raw-record preview outputs cleared before Git commit)
- **Scope:** Strictly read-only EDA. No original data was modified, cleaned, imputed, aggregated into a modeling dataset, merged, or used for model training.

---

## 1. Reproducible Loading & Memory Profile

- **CSV Engine:** Pandas C engine (`pd.read_csv(..., engine="c", low_memory=False)`), preceded by a header + 5-row inspection (`nrows=5`). Using the C engine avoids assuming `engine="pyarrow"` supports `nrows` or all Pandas kwargs.
- **File Size on Disk:** `35.54 MB` (`37,267,047 bytes`)
- **Loaded Dimensions:** **`203,185` rows × `26` columns**
- **Full Load Time:** `0.317 seconds` (final fresh-kernel execution)
- **In-Memory Footprint (`deep=True`):** **`58.65 MB`** (`61,503,305 bytes`) — fits comfortably within local RAM (`15.85 GB` total).

---

## 2. Dataset Structure, Cardinality & Column Meanings

> **Documentation Boundary:** No local data dictionary file was present inside `data/raw/training/`. Only `Yield_Mg_ha` is **verified directly from the official 2026 G2F EvalAI competition page** (`https://eval.ai/web/challenges/challenge-page/2742/overview`); all other 25 column meanings are **inferred interpretations** from standard agronomic and G2F naming conventions and must be confirmed against official G2F documentation.

| # | Column | Inferred Dtype | Non-Missing | Unique (Non-Null) | Deep Mem (MB) | Definition Status | Scientific Meaning / Interpretation |
| :-: | :--- | :--- | ---: | ---: | ---: | :--- | :--- |
| 1 | `Env` | `str` | 203,185 | 321 | 3.314 | Inferred | Unique field trial environment ID (`<Field_Location>_<Year>`). |
| 2 | `Year` | `int64` | 203,185 | 12 | 1.550 | Inferred | Growing season calendar year (`2014`–`2025`). |
| 3 | `Field_Location` | `str` | 203,185 | 46 | 2.339 | Inferred | Field trial site/location code. |
| 4 | `Experiment` | `str` | 203,185 | 12 | 4.487 | Inferred | G2F experiment / trial series identifier. |
| 5 | `Replicate` | `int64` | 203,185 | 4 | 1.550 | Inferred | Experimental replication number within a trial (`1`–`4`). |
| 6 | `Block` | `int64` | 203,185 | 50 | 1.550 | Inferred | Experimental block number within a trial/replicate (`1`–`50`). |
| 7 | `Plot` | `int64` | 203,185 | 2,588 | 1.550 | Inferred | Individual field plot ID (`1`–`2588`). |
| 8 | `Range` | `float64` | 183,826 | 124 | 1.550 | Inferred | Field layout spatial row/range coordinate. |
| 9 | `Pass` | `float64` | 183,736 | 286 | 1.550 | Inferred | Field layout spatial column/pass coordinate. |
| 10 | `Hybrid` | `str` | 203,185 | 6,203 | 4.963 | Inferred | Standardized maize hybrid identifier. |
| 11 | `Hybrid_orig_name` | `str` | 203,185 | 6,488 | 4.959 | Inferred | Original unstandardized hybrid name recorded in field trials. |
| 12 | `Hybrid_Parent1` | `str` | 200,639 | 2,695 | 3.807 | Inferred | First parental inbred line of the hybrid cross. |
| 13 | `Hybrid_Parent2` | `str` | 200,639 | 81 | 2.535 | Inferred | Second parental inbred / tester line of the hybrid cross. |
| 14 | `Plot_Area_ha` | `float64` | 203,184 | 204 | 1.550 | Inferred | Harvested or planted plot area in hectares (`ha`). |
| 15 | `Date_Planted` | `str` | 202,801 | 246 | 2.877 | Inferred | Calendar planting date of the plot/trial. |
| 16 | `Date_Harvested` | `str` | 201,703 | 388 | 3.021 | Inferred | Calendar harvest date of the plot/trial. |
| 17 | `Stand_Count_plants` | `float64` | 180,711 | 130 | 1.550 | Inferred | Emerged/established plant count per plot. |
| 18 | `Pollen_DAP_days` | `float64` | 157,946 | 59 | 1.550 | Inferred | Days after planting (`DAP`) to male flowering (anthesis / pollen shed). |
| 19 | `Silk_DAP_days` | `float64` | 155,490 | 61 | 1.550 | Inferred | Days after planting (`DAP`) to female flowering (silking). |
| 20 | `Plant_Height_cm` | `float64` | 176,437 | 1,136 | 1.550 | Inferred | Measured maize plant height (`cm`). |
| 21 | `Ear_Height_cm` | `float64` | 177,971 | 884 | 1.550 | Inferred | Measured ear insertion height (`cm`). |
| 22 | `Root_Lodging_plants` | `float64` | 131,814 | 86 | 1.550 | Inferred | Count of root-lodged plants per plot. |
| 23 | `Stalk_Lodging_plants` | `float64` | 146,015 | 94 | 1.550 | Inferred | Count of stalk-lodged plants per plot. |
| 24 | `Yield_Mg_ha` | `float64` | 192,974 | 136,389 | 1.550 | **Verified (EvalAI)** | **Prediction target:** absolute maize grain yield in `Mg/ha` adjusted to `15.5%` moisture. |
| 25 | `Grain_Moisture` | `float64` | 192,693 | 4,897 | 1.550 | Inferred | Grain moisture percentage (%) measured at harvest. |
| 26 | `Twt_kg_m3` | `float64` | 163,690 | 6,245 | 1.550 | Inferred | Grain test weight / bulk density (`kg/m^3`) measured at harvest. |

---

## 3. Missing Data & Sentinel Value Audit

### Per-Column Missingness (`n = 203,185`)

| Column | Non-Missing | Missing Count | Missing (%) |
| :--- | ---: | ---: | ---: |
| `Root_Lodging_plants` | 131,814 | 71,371 | 35.13% |
| `Stalk_Lodging_plants` | 146,015 | 57,170 | 28.14% |
| `Silk_DAP_days` | 155,490 | 47,695 | 23.47% |
| `Pollen_DAP_days` | 157,946 | 45,239 | 22.26% |
| `Twt_kg_m3` | 163,690 | 39,495 | 19.44% |
| `Plant_Height_cm` | 176,437 | 26,748 | 13.16% |
| `Ear_Height_cm` | 177,971 | 25,214 | 12.41% |
| `Stand_Count_plants` | 180,711 | 22,474 | 11.06% |
| `Pass` | 183,736 | 19,449 | 9.57% |
| `Range` | 183,826 | 19,359 | 9.53% |
| `Grain_Moisture` | 192,693 | 10,492 | 5.16% |
| `Yield_Mg_ha` | 192,974 | 10,211 | 5.03% |
| `Hybrid_Parent1` | 200,639 | 2,546 | 1.25% |
| `Hybrid_Parent2` | 200,639 | 2,546 | 1.25% |
| `Date_Harvested` | 201,703 | 1,482 | 0.73% |
| `Date_Planted` | 202,801 | 384 | 0.19% |
| `Plot_Area_ha` | 203,184 | 1 | 0.00% |
| `Env`, `Year`, `Field_Location`, `Experiment`, `Replicate`, `Block`, `Plot`, `Hybrid`, `Hybrid_orig_name` *(9 columns)* | 203,185 | 0 | 0.00% |

### Missing Target (`Yield_Mg_ha`) Distribution Across Years & Environments
- **Overall:** `10,211` missing plot targets (`5.03%`), leaving `192,974` valid yield observations.
- **By Year:** Missingness ranges from **`2.27%` (2018)** and **`2.54%` (2025)** up to **`8.54%` (2014)** and a notable spike of **`17.41%` (2017; 2,705 missing rows)**.
- **By Environment (`n = 321`):**
  - `8` environments have `0%` missing targets.
  - `313` environments have partial missingness.
  - **`0` environments** have `100%` missing targets (every environment has at least `38` valid yield plots).
  - `1` environment exceeds `50%` missing targets: **`SCH1_2024`** (`78.40%` missing — `392` missing, `108` valid out of `500`), followed by `NCH1_2019` (`43.50%`), `SCH1_2022` (`38.05%`), `INH1_2024` (`33.40%`), and `DEH1_2017` (`30.19%`).

### Sentinel String & Numerical Anomaly Audit (No Automatic Replacement)
- **Parsed string columns:** Zero unparsed sentinel strings (`"NA"`, `"null"`, `"."`, `"?"`, `"-"`, `"missing"`, etc.) remained after default Pandas loading.
- **Raw CSV NA tokens (`keep_default_na=False`):** Missing values in the raw CSV are encoded consistently as literal `"NA"` or empty string `""` (e.g., `Yield_Mg_ha`: `9,975` `"NA"` + `236` `""` = `10,211`).
- **Numerical sentinel codes (`-9999`, `-999`, `-99`, `-1`, `999`, `9999`):**
  - Zero negative values across all 17 numeric columns.
  - `Plot == 999` occurs `57` times, but continuity check (`997: 57`, `998: 57`, `999: 57`, `1000: 57`, `1001: 41`) confirms it is a **legitimate sequential plot number**, not a sentinel code.
  - `Root_Lodging_plants == 0` (`88,802` rows) and `Stalk_Lodging_plants == 0` (`68,741` rows) represent legitimate zero-lodging plots.
- **Range anomalies flagged for future inspection (not altered):**
  - `Range == 0`: `1` row (minimum is `0`, whereas `Pass` starts at `1`).
  - `Grain_Moisture`: `5` rows `< 5%` (min `0.08%`) and `391` rows `> 40%` (max `45.0%`).
  - `Twt_kg_m3`: `103` rows `< 500 kg/m^3` (min `33.85`) and `462` rows `> 900 kg/m^3` (max `2,188.21` — physically implausible for dry maize grain, suggesting possible unit-conversion or recording artifacts in those harvest-trait rows).

**Visualization:** [reports/figures/01_trait_missing_values.png](../reports/figures/01_trait_missing_values.png)

---

## 4. Target Variable Analysis (`Yield_Mg_ha`)

| Metric | Verified Value |
| :--- | ---: |
| Total rows | `203,185` |
| Valid numerical observations | `192,974` (`94.97%`) |
| Missing (`NaN`) observations | `10,211` (`5.03%`) |
| Non-numeric string observations | `0` |
| Minimum | `0.4282 Mg/ha` |
| 1st percentile (`P01`) | `1.9562 Mg/ha` |
| 5th percentile (`P05`) | `4.2781 Mg/ha` |
| 1st quartile (`Q1` / `P25`) | `7.7062 Mg/ha` |
| Median (`Q2` / `P50`) | `9.9179 Mg/ha` |
| Mean | `9.7504 Mg/ha` |
| 3rd quartile (`Q3` / `P75`) | `11.9271 Mg/ha` |
| 95th percentile (`P95`) | `14.6527 Mg/ha` |
| 99th percentile (`P99`) | `16.5029 Mg/ha` |
| Maximum | `23.2680 Mg/ha` |
| Interquartile range (`IQR = Q3 - Q1`) | `4.2209 Mg/ha` |
| Standard deviation (`ddof=1`) | `3.1278 Mg/ha` |
| Variance (`ddof=1`) | `9.7829 (Mg/ha)^2` |
| Skewness | `-0.2164` *(mild negative / left skew)* |
| Excess kurtosis | `-0.1311` |
| Zero values (`== 0`) | `0` |
| Negative values (`< 0`) | `0` |
| IQR lower fence (`Q1 - 1.5 × IQR`) | `1.3748 Mg/ha` (`835` low-tail plots) |
| IQR upper fence (`Q3 + 1.5 × IQR`) | `18.2585 Mg/ha` (`147` high-tail plots) |
| Total IQR-flagged observations | **`982` plots (`0.5089%` of valid yields)** |

> **Interpretation:** Yield is unimodal and nearly symmetric with a mild left tail (`skewness = -0.2164`, `mean = 9.75 < median = 9.92`), consistent with occasional stressed/low-yielding plots in multi-environment field trials. The `982` IQR-flagged plots (`0.51%`) are **not** removed or assumed invalid.

**Visualizations:**
- [reports/figures/02_trait_yield_distribution.png](../reports/figures/02_trait_yield_distribution.png) — Histogram with mean, median, and IQR fences alongside boxplot.
- [reports/figures/03_trait_yield_by_year.png](../reports/figures/03_trait_yield_by_year.png) — Yearly boxplots and yearly mean trendline (`2014–2025`).

---

## 5. Observation Granularity, Replicates & Duplicate Audit

### Identifier Counts & Group Distributions
- **Unique environments (`Env`):** `321` across `46` unique `Field_Location`s and `12` unique `Year`s (`12` unique `Experiment` labels).
- **Unique hybrids (`Hybrid`):** `6,203` standardized hybrids (`6,488` unique `Hybrid_orig_name`, `2,695` unique `Hybrid_Parent1`, `81` unique `Hybrid_Parent2`).
- **Observations per group:**
  - **Per `Year` (`12` years):** min `12,675`, median `16,236.5`, mean `16,932.08`, max `20,851`
  - **Per `Env` (`321` envs):** min `44`, median `514.0`, mean `632.98`, max `2,065`
  - **Per `Hybrid` (`6,203` hybrids):** min `1`, median `23.0`, mean `32.76`, max `1,018`

### `(Env, Hybrid)` Granularity & Experimental Replication
- **Unique `(Env, Hybrid)` combinations:** **`132,635`**
  - Exactly `1` plot observation: `76,402` (`57.60%`)
  - Multiple observations (**replicated**): **`56,233` (`42.40%`)** — primarily 2 replicates (`49,033`), with 3 (`3,414`), 4 (`2,727`), up to a max of `37` plots (likely repeated check hybrids)
  - With `≥ 1` valid `Yield_Mg_ha` label: **`128,597` (`96.96%`)**
  - With `≥ 2` valid `Yield_Mg_ha` labels: `52,392` (`39.50%`)
  - With `0` valid `Yield_Mg_ha` labels (all plots missing): **`4,038` (`3.04%`)**

### Duplicate Audit: Experimental Replicates vs. Accidental Duplicates
- **Exact duplicate rows (across all 26 columns):** **`0`**
- **Composite primary key uniqueness audit:**

| Candidate Key | Unique Across All 203,185 Rows? | Extra Duplicate Rows | Rows in Duplicate Groups |
| :--- | :---: | ---: | ---: |
| `Env + Plot` | No | 33,732 | 62,596 |
| `Env + Experiment + Plot` | No | 31,091 | 60,497 |
| `Env + Replicate + Plot` | No | 5,098 | 9,380 |
| `Env + Replicate + Block + Plot` | No | 178 | 326 |
| `Env + Experiment + Replicate + Plot` | No | 1,121 | 2,242 |
| **`Env + Experiment + Replicate + Block + Plot`** | **Yes** | **0** | **0** |
| `Env + Experiment + Replicate + Block + Plot + Range + Pass` | Yes | 0 | 0 |
| `Env + Hybrid` | No | 70,550 | 126,783 |
| `Env + Hybrid + Replicate` | No | 11,795 | 20,406 |
| `Env + Hybrid + Replicate + Block + Plot` | Yes | 0 | 0 |

- **Conclusion:** Every single row represents a distinct physical field plot uniquely keyed by `(Env, Experiment, Replicate, Block, Plot)` with **`0` exact duplicates**. The `70,550` additional rows sharing `(Env, Hybrid)` are **genuine experimental field replicates** (including `4,499` rows where the same hybrid appears in multiple plots within the same `(Env, Replicate, Block)` — e.g., check hybrids repeated across plots). Neither replicates nor records were removed or aggregated.

---

## 6. Temporal (2014–2025) & Environmental / Hybrid Variability

### Coverage & Yield by Year (`2014–2025` — All 12 Years Present, 0 Unexpected Years)

| Year | Plot Obs | Valid Yield | Missing Yield (%) | Envs (`Env`) | Locations | Unique Hybrids | Mean Yield | Median Yield | SD Yield |
| :-: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 2014 | 12,675 | 11,593 | 8.54% | 24 | 24 | 882 | 9.189 | 9.486 | 2.795 |
| 2015 | 13,688 | 13,219 | 3.43% | 27 | 27 | 1,210 | 8.718 | 8.776 | 2.868 |
| 2016 | 15,387 | 14,452 | 6.08% | 27 | 27 | 645 | 8.143 | 8.267 | 2.770 |
| 2017 | 15,533 | 12,828 | 17.41% | 30 | 30 | 579 | 9.692 | 10.015 | 3.069 |
| 2018 | 20,851 | 20,377 | 2.27% | 29 | 28 | 1,076 | 9.683 | 9.759 | 2.739 |
| 2019 | 20,806 | 19,876 | 4.47% | 28 | 28 | 1,207 | 10.996 | 11.088 | 3.002 |
| 2020 | 16,940 | 16,157 | 4.62% | 25 | 24 | 1,177 | 8.980 | 8.910 | 2.768 |
| 2021 | 20,132 | 19,577 | 2.76% | 27 | 27 | 1,193 | 10.044 | 10.320 | 2.783 |
| 2022 | 19,084 | 18,568 | 2.70% | 28 | 28 | 570 | 9.641 | 10.207 | 3.613 |
| 2023 | 18,864 | 18,274 | 3.13% | 27 | 27 | 564 | 10.214 | 10.438 | 3.368 |
| 2024 | 14,328 | 13,534 | 5.54% | 25 | 25 | 1,097 | 10.651 | 10.937 | 3.193 |
| 2025 | 14,897 | 14,519 | 2.54% | 24 | 24 | 1,081 | 10.357 | 10.568 | 3.149 |

### Environment-Level & Hybrid-Level Yield Variability
- **Environment means (`n = 321` envs):** range from **`1.445` to `15.051 Mg/ha`** (mean `9.514`, SD `2.511`, IQR `8.043–11.189`). Within-environment SD ranges from `0.350` to `3.696 Mg/ha` (median `1.959`, mean `1.963`).
- **Plot-level Variance Decomposition (Law of Total Variance, `n = 192,974`):**
  - Total variance: `9.7828`
  - **Between-environment variance:** `5.6125` (**`57.37%`**)
  - **Within-environment variance:** `4.1703` (**`42.63%`**)
  - *Modeling implication:* Over half of plot-level yield variance is environment-level shift, yet the competition metric (**average within-environment Pearson $r$**) evaluates **within-environment hybrid ranking** — making accurate modeling of the `42.63%` within-environment signal (genotype main effect + G × E interaction) critical!
- **Hybrid variability (`6,024` hybrids with ≥1 valid yield out of `6,203`):**
  - `4,307` hybrids (`71.50%`) were tested across `> 1` year (median `2.0` years, max `12` years).
  - `5,236` hybrids (`86.92%`) have `≥ 10` valid plots.
  - Per-hybrid mean yield across all `6,024` hybrids: mean `9.696 Mg/ha`, SD `1.383`, range `1.651–14.808 Mg/ha`; median within-hybrid SD across environments is `2.702 Mg/ha` (reflecting strong environmental sensitivity).

**Visualizations:**
- [reports/figures/04_trait_temporal_coverage.png](../reports/figures/04_trait_temporal_coverage.png) — Yearly plot counts, environments, unique hybrids, and mean (±1 SD) / median yield trend.
- [reports/figures/05_trait_env_hybrid_variability.png](../reports/figures/05_trait_env_hybrid_variability.png) — Histograms of environment mean yields, within-environment yield SDs, and hybrid mean yields (`≥10` plots).

---

## 7. Preliminary Data Leakage Assessment

> **Policy:** No column is automatically approved for modeling based on correlation with `Yield_Mg_ha`. Final feature eligibility requires auditing the 2026 testing files and metadata in subsequent phases.

| Category | Count | Columns | Assessment & Rationale |
| :--- | :-: | :--- | :--- |
| **Target variable** | 1 | `Yield_Mg_ha` | Official competition prediction target (`Mg/ha` at `15.5%` moisture). Never an input feature. |
| **Identifiers** | 11 | `Env`, `Year`, `Field_Location`, `Experiment`, `Replicate`, `Block`, `Plot`, `Range`, `Pass`, `Hybrid`, `Hybrid_orig_name` | Join keys (`Env`, `Hybrid`), temporal/CV grouping keys (`Year`, `Env`, `Field_Location`), and plot-level field-design IDs (`Replicate`, `Block`, `Plot`, `Range`, `Pass`). Plot-level layout IDs are unlikely to exist in 2026 `(Env, Hybrid)` test rows. |
| **Potential prediction-time features** | 2 | `Hybrid_Parent1`, `Hybrid_Parent2` | Parental inbred/tester pedigree; potentially available or derivable from `Hybrid` or genotype files (must verify against 2026 test inputs; missing in `1.25%` of training rows). |
| **Unknown availability requiring investigation** | 2 | `Plot_Area_ha`, `Date_Planted` | Management/design attributes; `Date_Planted` is valuable for weather-window alignment **only if** provided in 2026 test metadata. `Plot_Area_ha` is likely unavailable at test time. |
| **Potential post-harvest or outcome-derived features** *(HIGH / SEVERE LEAKAGE RISK)* | 10 | `Date_Harvested`, `Grain_Moisture`, `Twt_kg_m3`, `Stand_Count_plants`, `Pollen_DAP_days`, `Silk_DAP_days`, `Plant_Height_cm`, `Ear_Height_cm`, `Root_Lodging_plants`, `Stalk_Lodging_plants` | **Prohibited as direct input features.** `Date_Harvested`, `Grain_Moisture`, and `Twt_kg_m3` are measured **at harvest** (and `Grain_Moisture` is used directly to moisture-adjust `Yield_Mg_ha`). The other 7 are **in-season plot phenotypes** observed on growing 2026 crops and unavailable as test-time inputs. |

---

## 8. Confirmed Findings vs. Unresolved Technical Questions

### Confirmed Findings (Verified Empirically)
1. `1_Training_Trait_Data_2014_2025.csv` contains `203,185` plot-level rows × `26` columns (`58.65 MB` deep RAM), spanning all 12 years `2014–2025` with `321` environments and `6,203` standardized hybrids.
2. Zero exact duplicate rows exist; `(Env, Experiment, Replicate, Block, Plot)` uniquely identifies all `203,185` plots, while `56,233` `(Env, Hybrid)` pairs (`42.40%`) have genuine experimental replicates.
3. `Yield_Mg_ha` has `192,974` valid positive observations (`5.03%` missing, `0` zeros, `0` negatives, `0` non-numeric strings), mean `9.7504`, median `9.9179`, SD `3.1278`, skewness `-0.2164`, and `982` IQR-flagged plots (`0.51%`).
4. Plot-level yield variance splits `57.37%` between-environment vs. `42.63%` within-environment.
5. Ten co-measured harvest/in-season trait columns pose severe target/temporal leakage risk if used as direct predictors.

### Unresolved Technical Questions & Proposed Decisions (Not Yet Finalized)
1. **Official G2F Data Dictionary:** Confirm inferred meanings and units for the 25 non-target columns (especially `Twt_kg_m3` extreme values `> 900` / `< 500` and `Grain_Moisture < 5%`).
2. **2026 Test Schema Audit (Phase 2.3+):** Verify exact columns available in 2026 testing metadata and submission template (`Date_Planted`, `Field_Location`, `Hybrid_Parent1/2`, `Plot_Area_ha`, `Experiment`).
3. **Training Target Granularity (Phase 3 Decision):** Decide whether to model at plot level (`192,974` valid rows) vs. `(Env, Hybrid)` aggregate mean/BLUP level (`128,597` valid combinations), and how to weight replicated vs. unreplicated `(Env, Hybrid)` combinations.
4. **High-Missingness & Tail-Yield Policy (Phase 3 Decision):** Investigate `2017` (`17.41%` missing targets) and `SCH1_2024` (`78.40%` missing targets) against trial metadata/weather before deciding whether to retain or weight them during training/CV.
5. **Potential Multi-Task Learning (Phase 4+ Proposal):** Explore whether in-season/harvest traits (`Plant_Height_cm`, `Pollen_DAP_days`, `Silk_DAP_days`, `Grain_Moisture`) improve neural or multi-output models strictly as **auxiliary training targets**, never as test inputs.

---

## 9. Compliance & Data-Sharing Hygiene

- Checked official 2026 G2F EvalAI challenge terms (`https://eval.ai/web/challenges/challenge-page/2742/overview`): the historical dataset is openly shared for competition research and open-access model publication, and no rule prohibits sharing non-sensitive aggregate summary statistics or aggregate figures.
- All raw competition files remain strictly excluded by `.gitignore` (`git ls-files` verified).
- Outputs of the two notebook cells displaying raw sample rows (`preview_df` and `df.head(5)`) were cleared prior to committing `notebooks/01_trait_data_eda.ipynb`, and the committed notebook was scanned to verify zero raw plot records appear in the file.
