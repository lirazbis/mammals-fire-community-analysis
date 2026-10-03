# Mammal communities and fire in Sierra Nevada forests

R code and input tables for analyses of mammal community composition across
97 Sierra Nevada forest sites with contrasting fire histories. The workflow
includes multispecies occupancy models, NMDS and RDA, variance partitioning,
occupancy-uncertainty simulations, and manuscript figures.

## Run the analysis

1. Clone this repository or choose **Code > Download ZIP** and extract it.
   Keep the folder names and file names unchanged.
2. Open `Mammals_analysis.Rproj` in RStudio. This sets the working directory
   to the folder containing the two Rmd files. No personal computer path is needed.
3. In a fresh R session, run `01_analysis.Rmd` from top to bottom, or knit it.
   The eight occupancy candidate models can take some time to fit.
4. Open `02_figures.Rmd`, run its setup chunk, then run the desired figure chunks.
   Knitting it also produces the retained supplementary/diagnostic figures.

Without RStudio, set the working directory to the extracted archive folder.
To render the documents with R Markdown:

```r
rmarkdown::render("01_analysis.Rmd")
rmarkdown::render("02_figures.Rmd")
```

Rendering HTML requires Pandoc (included with RStudio). Figures use Arial;
having Arial installed preserves the manuscript typography.

## Input and output folders

```
01_analysis.Rmd
02_figures.Rmd
Mammals_analysis.Rproj
to rerun the archived code/
    [11 vegetation, environmental, fire, trait, image, and plot metadata CSVs]
    detection data/
        det_his_forordering.csv
        survey_visit_dates_julian_day_level_correct.csv
        [nine y_SPECIES.csv daily detection tables]
```

All 22 CSVs are required by the analysis or its study-summary sections. The
spatial covariates have already been extracted into these tables; the code
does not read the upstream GIS layers or the original camera photographs.

The analysis creates `results/tables/` and `results/analysis_results.rds`.
The figures read that RDS and write to `figures/final for submission/`.
The analysis defines only `data_dir`, `detection_dir`, `results_dir`, and
`tables_dir`. The figures define only `results_dir` and `figures_dir`.
All paths are relative and are joined with `file.path()`; old folder aliases
are no longer used. Outputs stay inside the extracted archive. Rerunning replaces matching output
files. This repository contains code and inputs; generated model results and figures
are not bundled. Run the analysis before the figures on a new computer.

## R packages

The validation environment used R 4.5.1 on Windows. `package_versions.csv`
records the installed versions of the 18 direct packages and their dependencies.
`archive_sessionInfo.txt` records the R/platform information. R and the package
binaries are not inside this data archive.

For a computer that does not already have the packages, install the missing
CRAN packages (this installs currently available versions):

```r
cran_packages <- c(
  "unmarked", "dplyr", "tidyr", "vegan", "e1071", "bestNormalize", "usdm",
  "AICcmodavg", "tibble", "ggplot2", "ggrepel", "patchwork", "corrplot",
  "ggnewscale", "ragg", "knitr", "rmarkdown"
)
missing <- setdiff(cran_packages, rownames(installed.packages()))
if (length(missing)) install.packages(missing)
```

`pairwiseAdonis` is a GitHub package. The installed version was 0.4.1 at the
following recorded commit:

```r
if (!requireNamespace("pairwiseAdonis", quietly = TRUE)) {
  if (!requireNamespace("remotes", quietly = TRUE)) install.packages("remotes")
  remotes::install_github(
    "pmartinezarbizu/pairwiseAdonis", subdir = "pairwiseAdonis",
    ref = "cb190f7668a0c82c0b0853927db239e7b9ec3e83", upgrade = "never"
  )
}
```

For reproducing the original software environment, use the versions recorded
in `package_versions.csv`; the installation commands above do not pin CRAN
versions. Package installation requires internet access and may require build
tools if a binary for the requested platform/version is unavailable.

## Validation and provenance

Archive update: 2026-10-02. Changes adapt the input/output paths to this folder
structure. Statistical formulas, transformations, and plotting code are preserved.
Both Rmds were parsed; all 22 input CSVs were read by their analysis sections.
The prepared model inputs matched the previous checked inputs: 97 sites,
115 daily visits, and nine species, with unchanged detections, scaled covariates,
and scaled dates. Figure setup was checked using a temporary saved-results
fixture inside the test archive. This directory update did not refit the models.

The selected occupancy model is named `m6`. The two-group partition `vp5`
has been removed; the three- and four-group partitions remain. The current
figure code exports PNG files without PDF copies.

Existing input-audit warnings, including missing survey dates, remain visible
in the analysis. Input values were not edited for packaging.

`archive_manifest.csv` lists the included files, sizes, and SHA-256 hashes
(excluding the manifest itself). The source ZIP was checked for extraction integrity.
