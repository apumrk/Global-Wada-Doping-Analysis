# Global Anti-Doping Testing and Detected-Risk Analysis

## Project overview

This repository contains the R Markdown code, prepared datasets, and report files for my individual data-science project within the seminar **Doping in Sports**.

The main part of the project is a **global analysis of World Anti-Doping Agency (WADA) data**. The analysis examines how anti-doping testing is distributed across sports and whether testing volume is aligned with detected doping-risk signals. Testing data are analysed for **2016–2024** and confirmed Anti-Doping Rule Violation (ADRV) data for **2016–2023**.

The project does **not** attempt to estimate the true prevalence of doping in a sport. Instead, it compares observed testing activity with detected outcomes such as Adverse Analytical Findings (AAFs) and confirmed ADRVs.

A global benchmark file was prepared after the main analysis as an additional output for the Germany-focused part of the group project.

## Main research question

> To what extent do global anti-doping testing patterns correspond to sport-specific detected doping-risk signals?

The analysis covers the following main topics:

1. Development of global testing volumes from 2016 to 2024.
2. Sports with the strongest AAF-based detected-risk signals.
3. Alignment between testing share and AAF share.
4. Relationship between AAFs and confirmed ADRVs.
5. Confirmed ADRV risk after adjusting for testing volume.
6. Differences in in-competition and out-of-competition testing strategy.
7. Final synthesis of global testing and detected-risk patterns.

Additional Olympic/Paralympic exploratory analyses are included in `data_Research.Rmd`, but they are supplementary and are not the main focus of the final report.

---

## Data sources

The project is based on publicly available WADA annual reports:

- **WADA Anti-Doping Testing Figures Reports** – testing volume, AAFs, in-competition/out-of-competition testing, and sample-type information.
- **WADA Anti-Doping Rule Violations Reports** – analytical and non-analytical confirmed ADRVs and available AAF case outcomes.

Source pages:

- WADA Testing Figures: https://www.wada-ama.org/en/resources/anti-doping-stats/anti-doping-testing-figures-report
- WADA ADRV Reports: https://www.wada-ama.org/en/resources/general-anti-doping-information/anti-doping-rule-violations-adrvs-report

The annual reports were processed into harmonised sport-year datasets before the analysis was carried out.

---

## Main files

### R Markdown code

| File | Purpose |
|---|---|
| `data_processing_wada.Rmd` | Combines and cleans annual WADA Testing Figures sport-level files for 2016–2024. Standardises variables, harmonises sport names, aggregates duplicate sport-year rows, and performs validation checks. |
| `data_processing_adrv.Rmd` | Combines and cleans annual WADA ADRV sport-level files for 2016–2023. Standardises changing report structures, separates analytical and non-analytical ADRVs, harmonises sport names, and creates validation/audit outputs. |
| `test_adrv_join.Rmd` | Joins the cleaned Testing Figures and ADRV datasets by `year` and harmonised sport name. Creates sport-year match indicators, rate/share variables, mismatch measures, and join-validation files. |
| `data_Research.Rmd` | Main analysis file. Contains the research-question analyses, tables, figures, interpretation, final synthesis, and additional exploratory Olympic/Paralympic analyses. |
| `global_benchmark.Rmd` | Creates the transferable global sport-year benchmark used as an additional output for comparison with the Germany-focused project. |

### Final datasets

| File | Coverage | Size / role |
|---|---|---|
| `wada_testing_sport_year_2016_2024.csv` | 2016–2024 | Final cleaned WADA testing dataset; 1,416 sport-year rows and 223 harmonised sports. |
| `wada_adrv_sport_year_2016_2023.csv` | 2016–2023 | Final cleaned ADRV dataset; 831 sport-year rows and 160 harmonised sports. |
| `wada_testing_adrv_sport_year_2016_2023_joined_analysisR.csv` | 2016–2023 | Joined analysis dataset combining testing, AAF, and ADRV information; 1,262 rows. |
| `wada_global_sport_year_benchmark_2016_2024.csv` | 2016–2024 | Global benchmark dataset prepared for the Germany comparison; 1,416 rows. |

### Report and rendered analysis

| File | Purpose |
|---|---|
| `Report.pdf` | Compiled final report. |
| `Data Research_World Doping Analysis.pdf` / `.html` | Earlier rendered R Markdown version of the global analysis. |
| `data_Research.html` | Rendered output of the extended analysis R Markdown file. |

---

## Project workflow

The project has two independent data-preparation streams that are joined for the main analysis:

```text
WADA Testing Figures (2016–2024)       WADA ADRV Reports (2016–2023)
                |                                  |
                v                                  v
      Clean / standardise                 Clean / standardise
      Harmonise sport names               Harmonise sport names
                |                                  |
                v                                  v
        Testing dataset                       ADRV dataset
                \                                  /
                 \                                /
                  -------- sport-year join -------
                               |
                               v
                     Joined analysis dataset
                               |
                               v
                    Global analysis in R
                               |
                               v
            Additional Germany benchmark output
```

The main analytical work is the global analysis. The benchmark file is a secondary output created so that the global indicators can be reused in the Germany case study.

---

## Recommended execution order

### A. Rebuild everything from the annual source files

If the annual sport-level source CSV files are available, run the files in this order:

1. **`data_processing_wada.Rmd`**  
   Reads annual files matching approximately `wada_testing_YYYY_sport_totals.csv`, combines them, cleans variables and sport names, aggregates to sport-year level, and produces validation files.

2. **`data_processing_adrv.Rmd`**  
   Reads annual files matching approximately `wada_adrv_YYYY_sport_totals.csv`. The later harmonisation stage also uses the cleaned testing sport names so that ADRV sports match the testing dataset consistently.

3. **`test_adrv_join.Rmd`**  
   Joins the final cleaned testing and ADRV datasets for 2016–2023 and creates the joined analysis file plus unmatched-row and validation outputs.

4. **`data_Research.Rmd`**  
   Runs the main global analysis and produces the statistical summaries, figures, and final synthesis.

5. **`global_benchmark.Rmd`**  
   Creates the additional global benchmark file for the Germany-focused project.

### B. Reproduce the analysis from the submitted final datasets

If the four final CSV files listed above are already available, the full raw-data cleaning stage does not need to be repeated. The main analysis can be reproduced from:

- `wada_testing_sport_year_2016_2024.csv`
- `wada_testing_adrv_sport_year_2016_2023_joined_analysisR.csv`

The benchmark script uses:

- `wada_testing_sport_year_2016_2024.csv`
- `wada_adrv_sport_year_2016_2023.csv`

---

## Important setup before running the code

The R Markdown files were developed locally in RStudio and currently contain a Windows-specific path such as:

```r
base_dir <- "C:/Users/Apu/Desktop/asl/rstudio files/seminar/project1/data"
```

**Change `base_dir` to the folder containing the project data on your computer before running the files.**

Some processing files also refer to intermediate development filenames. The submitted final equivalents are:

| Development / intermediate name | Submitted final file |
|---|---|
| `wada_testing_sport_year_2016_2024_final_sport_names.csv` | `wada_testing_sport_year_2016_2024.csv` |
| `wada_adrv_sport_year_2016_2023_cleaned_for_testing_join.csv` | `wada_adrv_sport_year_2016_2023.csv` |
| `wada_testing_adrv_sport_year_2016_2023_joined_analysis.csv` | `wada_testing_adrv_sport_year_2016_2023_joined_analysisR.csv` |

If the repository is reorganised into subfolders, the corresponding file paths in the R Markdown files must be updated as well.

---

## R packages

The project was written in **R / RStudio** and uses the following packages across the R Markdown files:

```r
install.packages(c(
  "tidyverse",
  "janitor",
  "stringr",
  "scales",
  "knitr",
  "DT",
  "rmarkdown"
))
```

Main uses include:

- `tidyverse` / `dplyr` – data manipulation and aggregation
- `readr` – CSV input/output
- `ggplot2` – visualisation
- `stringr` – text and sport-name cleaning
- `janitor` – column-name standardisation
- `scales` – axis and number formatting
- `knitr` / `rmarkdown` – report generation
- `DT` – interactive tables in rendered analysis output

---

## Key analytical measures

The main derived measures include:

```text
AAF rate per 1,000 tests
    = AAF count / total samples * 1,000

ADRV rate per 1,000 tests
    = total ADRVs / total samples * 1,000

Testing share
    = sport samples / samples across all sports

AAF share
    = sport AAFs / AAFs across all sports

Testing–Risk Mismatch Score
    = AAF share - Testing share
```

The main rate-based comparisons use a minimum threshold of **1,000 tests** to reduce unstable interpretations from very small denominators. The final synthesis also uses relative top-quartile thresholds; these are analytical classifications and are **not official WADA risk categories**.

---

## Validation and reproducibility notes

The processing workflow includes checks for:

- duplicate sport-year records;
- changes in totals before and after aggregation;
- harmonisation of sport names across years and sources;
- unmatched testing/ADRV sport-year rows;
- recalculated testing, AAF, and ADRV shares;
- internal consistency of in-competition and out-of-competition values;
- year-level totals after joining.

Where detailed reporting components did not reproduce the published total exactly, the original reported total was retained rather than being adjusted artificially. Audit and validation files are produced by the processing scripts so that these cases can be reviewed.

Because WADA report formats changed across years, exact reproduction from the original annual source tables requires the same extracted annual sport-level CSV inputs used during development.

---

## Notes on interpretation

AAFs and ADRVs should not be treated as the same measure:

- An **AAF** is a laboratory finding that requires further case handling.
- An **ADRV** is a confirmed anti-doping rule violation.

The analysis therefore refers to **detected-risk signals**, not the true prevalence of doping. Testing volume itself affects the opportunity to detect findings, and anti-doping organisations may use additional risk information that is not contained in these datasets.

---

## AI assistance

ChatGPT (OpenAI) was used as a supporting tool during parts of the project, mainly for discussing data-cleaning approaches, debugging R code, and reviewing possible analytical and visualisation structures. Final data-processing decisions, validation, statistical analysis, interpretation, and report preparation were reviewed and implemented as part of the project work.

---

## Author and project context

**Author:** Apu Kumar Saha  
**Seminar:** Doping in Sports  
**Institution:** TU Dortmund University  
**Project:** Global WADA anti-doping analysis  
**Analysis period:** Testing 2016–2024; ADRV 2016–2023

