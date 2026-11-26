# Raw Training Dataset Inventory — G2F Maize Yield Prediction 2026

- **Phase:** Phase 2.1 — Training Dataset Inventory
- **Inspection Date:** 2026-11-26
- **Inspected Root Directory:** `data/raw/training/`
- **Scope:** Read-only filesystem metadata inventory only. No competition files were opened, extracted, loaded into memory, modified, moved, renamed, or analyzed statistically.

> **Verification Distinction:**
> - **Verified Information** is derived strictly from filesystem metadata (`pathlib.Path.stat()`, directory traversal) and Git (`git check-ignore`, `git status`).
> - **Assumptions / Inferences** (file format semantics, temporal coverage, dataset category, compression ratios) are inferred solely from filenames and extensions and **must be confirmed empirically in Phase 2.2**.

---

## 1. Summary Totals (Verified)

| Metric | Verified Value |
| :--- | :--- |
| **Nested Directories Under `data/raw/training/`** | 2 (`Training_Data-20261123T123657Z-1-001/` and `Training_Data-20261123T123657Z-1-001/Training_Data/`) |
| **Total Raw Dataset Files** *(excluding `.gitkeep`)* | **9 files** |
| **Uncompressed Files** | 8 files (`153.62 MB` / `161,077,687 bytes`) |
| **Compressed Files** | 1 file (`417.45 MB` / `437,730,637 bytes`) |
| **Combined Storage Size (Binary MiB / GiB)** | **571.07 MB** (**0.5577 GB**) |
| **Combined Storage Size (Exact Bytes / SI)** | **598,808,324 bytes** (`598.81 MB` / `0.5988 GB` base-10) |
| **Git Ignore Verification** | **9 / 9 dataset files ignored** (`0` unignored; `git status` clean) |

> **Directory Note:** All 9 original files reside in `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/`. The placeholder `data/raw/training/.gitkeep` remains intact at the root of `data/raw/training/`. No files were moved or renamed.

---

## 2. Complete File Inventory

Sizes below are reported in binary megabytes (`1 MB = 1,048,576 bytes`) and binary gigabytes (`1 GB = 1,073,741,824 bytes`), alongside exact byte counts.

| # | Original Filename *(Verified)* | Relative File Path *(Verified)* | Extension & Likely Format *(Extension Verified; Format Inferred)* | Size (MB / GB) *(Verified)* | Exact Bytes *(Verified)* | Compression Status *(Inferred from Extension)* | Likely Dataset Category *(Filename Assumption)* |
| :-: | :--- | :--- | :--- | ---: | ---: | :--- | :--- |
| 1 | `1_Training_Trait_Data_2014_2025.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/1_Training_Trait_Data_2014_2025.csv` | `.csv` (Comma-Separated Values) | 35.54 MB (0.0347 GB) | 37,267,047 | Uncompressed | Phenotype / Trait data (2014–2025) |
| 2 | `2_Training_Meta_Data_2014_2025.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/2_Training_Meta_Data_2014_2025.csv` | `.csv` (Comma-Separated Values) | 0.11 MB (0.0001 GB) | 117,546 | Uncompressed | Metadata (trial/environment/management, 2014–2025) |
| 3 | `3_Training_Soil_Data_2015_2025.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/3_Training_Soil_Data_2015_2025.csv` | `.csv` (Comma-Separated Values) | 0.03 MB (<0.0001 GB) | 33,426 | Uncompressed | Soil data (2015–2025; note 2014 not in filename) |
| 4 | `4_Training_Weather_Data_2014_2025_full_year.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/4_Training_Weather_Data_2014_2025_full_year.csv` | `.csv` (Comma-Separated Values) | 11.64 MB (0.0114 GB) | 12,207,591 | Uncompressed | Weather data (full-year coverage, 2014–2025) |
| 5 | `4_Training_Weather_Data_2014_2025_seasons_only.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/4_Training_Weather_Data_2014_2025_seasons_only.csv` | `.csv` (Comma-Separated Values) | 6.10 MB (0.0060 GB) | 6,393,938 | Uncompressed | Weather data (growing seasons only, 2014–2025) |
| 6 | `5_Genotype_Data_All_2014_2027_hybrids_2kSites.vcf` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/5_Genotype_Data_All_2014_2027_hybrids_2kSites.vcf` | `.vcf` (Variant Call Format) | 58.28 MB (0.0569 GB) | 61,106,222 | Uncompressed | Genotype / SNP data (~2k sites; 2014–2027 hybrids) |
| 7 | `5_Genotype_Data_All_2014_2027_hybrids_2kSites_numeric.txt` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/5_Genotype_Data_All_2014_2027_hybrids_2kSites_numeric.txt` | `.txt` (Delimited text; likely numeric SNP matrix) | 41.38 MB (0.0404 GB) | 43,390,064 | Uncompressed | Genotype / SNP data (~2k sites, numeric encoding) |
| 8 | `5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` | `.vcf.gz` (Gzip-compressed Variant Call Format) | 417.45 MB (0.4077 GB) | 437,730,637 | **Compressed (gzip)** | Genotype / SNP data (~400k sites; 2014–2027 hybrids) |
| 9 | `6_Training_EC_Data_2014_2025.csv` | `data/raw/training/Training_Data-20261123T123657Z-1-001/Training_Data/6_Training_EC_Data_2014_2025.csv` | `.csv` (Comma-Separated Values) | 0.54 MB (0.0005 GB) | 561,853 | Uncompressed | Environmental covariates (EC, 2014–2025) |

---

## 3. Dataset Category Breakdown (Filename-Based Assumptions)

| Category | Availability | File Count | Combined Size (MB) | Files |
| :--- | :--- | :-: | ---: | :--- |
| **Phenotype / Trait data** | Present | 1 | 35.54 MB | `1_Training_Trait_Data_2014_2025.csv` |
| **Metadata** | Present | 1 | 0.11 MB | `2_Training_Meta_Data_2014_2025.csv` |
| **Soil data** | Present | 1 | 0.03 MB | `3_Training_Soil_Data_2015_2025.csv` |
| **Weather data** | Present | 2 | 17.74 MB | `4_Training_Weather_Data_2014_2025_full_year.csv`, `4_Training_Weather_Data_2014_2025_seasons_only.csv` |
| **Genotype / SNP data** | Present | 3 | 517.11 MB | `5_Genotype_Data_All_2014_2027_hybrids_2kSites.vcf`, `5_Genotype_Data_All_2014_2027_hybrids_2kSites_numeric.txt`, `5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` |
| **Environmental covariates** | Present | 1 | 0.54 MB | `6_Training_EC_Data_2014_2025.csv` |
| **Documentation** | **Not present** | 0 | 0.00 MB | None found inside `data/raw/training/` |

### Notable Filename Observations (To Verify in Phase 2.2)
1. **Soil coverage starts at 2015 in filename:** `3_Training_Soil_Data_2015_2025.csv` omits `2014` compared to `2014_2025` in trait, metadata, weather, and EC filenames.
2. **Two weather files:** Both `full_year` (`11.64 MB`) and `seasons_only` (`6.10 MB`) variants are present; schema differences and temporal boundaries should be audited in Phase 2.2.
3. **Multi-year hybrid genotype span (`2014_2027`):** All three genotype filenames reference `All_2014_2027_hybrids`, suggesting a shared marker panel across historical and testing/future hybrids. Care must be taken during Phase 3+ to ensure any unsupervised genotype transformations (e.g., PCA, imputation, allele frequency filtering) respect leakage prevention rules.
4. **Missing competition documentation files:** No README, data dictionary, or column-description file was present inside `data/raw/training/`.

---

## 4. Hardware, Memory & Storage Risk Assessment

Mapped against the verified local hardware recorded in Phase 1 (`Intel Core i7-9750H`, 6C/12T; `15.85 GB` total RAM, `2.63 GB` available during Phase 1 check; `NVIDIA GeForce GTX 1660 Ti` with `6 GB` VRAM; `C:` drive `464.04 GB` total, **`20.49 GB` currently free** after copying raw training data):

1. **Critical Risk — Uncompressed Expansion of `5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` (`417.45 MB` compressed):**
   - **Disk Risk:** Text VCF files typically compress by ~5×–20× under gzip (exact uncompressed size is unverified and must not be extracted to disk). Decompressing this archive could require multiple gigabytes (potentially ~4–10+ GB), consuming a large share of the remaining **`20.49 GB`** free space on `C:`.
   - **RAM Risk:** Loading ~400k variant sites across all hybrids into a naive Pandas DataFrame (object strings or `float64`) could easily exceed both available RAM (`2.63 GB`) and total system RAM (`15.85 GB`), causing Out-Of-Memory (OOM) failures or heavy disk thrashing.
2. **Moderate Risk — 2k-Site Genotype Files (`58.28 MB` `.vcf` + `41.38 MB` `_numeric.txt`):**
   - Safe to process in RAM with compact numeric dtypes (`int8`/`float32`), but parsing raw `.vcf` strings into Python objects expands memory several-fold. The `_numeric.txt` panel is strongly preferred for early baseline pipelines once validated against the `.vcf`.
3. **Low Risk — Tabular `.csv` Files (`53.96 MB` combined):**
   - Phenotype (`35.54 MB`), Weather (`17.74 MB` combined), EC (`0.54 MB`), Metadata (`0.11 MB`), and Soil (`0.03 MB`) fit comfortably in RAM. However, careless many-to-many joins (e.g., expanding daily weather directly onto plot-level trait rows) can still inflate memory unexpectedly.
4. **Tight Disk Headroom (`20.49 GB` free on `C:`):**
   - Remaining free space must accommodate `data/raw/testing/` (when copied), derived `data/processed/` caches, experiment outputs, and any future ML/deep learning artifacts. Avoid duplicating uncompressed data files and monitor disk usage before each new phase.

---

## 5. Recommended Loading Strategies (Design Recommendations Only — Not Yet Implemented)

| File Format / Group | Files | Recommended Strategy for Phase 2.2+ |
| :--- | :--- | :--- |
| **Small-to-Medium Tabular `.csv`** | Trait, Metadata, Soil, Weather (`full_year`, `seasons_only`), EC | Inspect schema/headers on small row slices first (`nrows`). Load with `pandas.read_csv(..., engine="pyarrow")` or explicit `dtype`/`parse_dates` mappings (`float32`, `int32`, `category`). Cache validated tables in `data/processed/` as compressed Parquet (`pyarrow`) for fast, low-memory reloads. |
| **Delimited Numeric Genotype `.txt`** | `5_Genotype_Data_All_2014_2027_hybrids_2kSites_numeric.txt` (`41.38 MB`) | Inspect first few lines to confirm delimiter, orientation (hybrids × markers vs. markers × hybrids), and missing-value tokens. Load directly into compact numeric arrays (`float32` or `int8` if discrete dosage values) rather than default `float64`; persist validated matrices as Parquet or memory-mapped NumPy (`.npy` / `mmap_mode="r"`). |
| **Uncompressed `.vcf`** | `5_Genotype_Data_All_2014_2027_hybrids_2kSites.vcf` (`58.28 MB`) | Stream line-by-line with Python's file iterator, parsing `##` metadata and `#CHROM` headers separately from variant rows. Avoid loading raw VCF fields into an untyped Pandas DataFrame. Cross-check consistency with `2kSites_numeric.txt` and prefer the numeric file for baseline iterations. |
| **Gzip-Compressed High-Density `.vcf.gz`** | `5_Genotype_Data_All_2014_2027_hybrids_400kSites.vcf.gz` (`417.45 MB`) | **Never decompress to plain `.vcf` on disk.** Stream directly via `gzip.open(..., "rt")` (or an approved genomics parser in later phases) in fixed site chunks (e.g., 1,000–5,000 variants per batch). Encode genotype calls on the fly into `int8`/`uint8` dosage blocks, compute incremental summaries / genomic relationship matrices (GRM) / incremental PCA per chunk, or export filtered/compressed binary representations rather than holding a dense `float64` 400k-site matrix in RAM. |

---

## 6. Git Safety Verification (Verified)

- Checked all 9 files individually using `git check-ignore -q --no-index`: **9 / 9 returned ignored (`exit code 0`)**.
- Checked repository working tree using `git status -uall -s`: **zero untracked or staged training data files**.
- Confirmed existing `.gitignore` rule `data/raw/training/*` (with `!data/raw/training/.gitkeep`) protects the entire nested `Training_Data-20261123T123657Z-1-001/` directory tree from accidental commits.
