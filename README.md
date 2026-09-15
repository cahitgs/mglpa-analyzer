# MG-LPA Analyzer

**A browser-based tool that generates, organizes, and reports multi-group tests of profile similarity for latent profile analysis (LPA) in Mplus**, following the six-step sequence of Morin, Meyer, Creusier, and Biétry (2016).

- Online version: **https://cahitgs.github.io/mglpa-analyzer/**
- Offline version: download the latest release ZIP from the **Releases** page, unzip it, and open `index.html` in your browser. No installation, no server, no internet connection needed.
- Example materials: the simulated dataset and the project file that reproduces every result of the article are in `example/` (see below).
- Archived releases with DOI (all versions): [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22698717.svg)](https://doi.org/10.5281/zenodo.22698717)

> **What you need.** A licensed copy of **Mplus** (version 8 or later) installed on your own computer. The MG-LPA Analyzer does **not** estimate any model: it writes Mplus input files, reads the Mplus output files you upload, and turns them into tables and figures. All estimation runs locally in Mplus on your machine, so run times depend on your own CPU and memory.

> **What it is for.** Latent profile analysis with continuous indicators, compared across two or more known groups (e.g., gender, culture, age group). It is not a general clustering tool and it does not (yet) support latent class analysis with categorical indicators.

> **What it is not.** A substitute for mixture-modeling expertise. The tool removes the coding burden and standardizes reporting; deciding how many profiles to retain, judging the theoretical meaning of a solution, and interpreting borderline fit comparisons remain the researcher's responsibility.

## Workflow

| Step | What the app does | What you do in Mplus |
|---|---|---|
| **0 — Class enumeration** | Generates one input file per group and per number of profiles (e.g., 1–8), packages them in a ZIP, then parses the outputs into fit tables and an elbow plot | Run the input files |
| **1 — Similarity tests** | Generates the configural, structural, dispersion, and distributional models; compares CAIC/BIC/ABIC across nested models; exports start values | Run the four input files |
| **2 — Profile plot** | Draws standardized profile plots (bar/line/radar, nine palettes incl. colorblind-safe); exports PNG and Excel | — |
| **3 — Predictive similarity** | Builds the free and equal multinomial-logistic models on fixed start values, plus all pairwise re-orderings; tabulates odds ratios and CIs | Run the input files |
| **4 — Explanatory similarity** | Builds the free and equal outcome models (with free or equal outcome variances) including delta-method contrasts; tabulates means, CIs, and pairwise tests | Run the input files |

Every step offers **Save Project / Load Project** (a JSON file with all settings and parsed results) and APA-style tables that can be downloaded as Word documents. The `example/` folder contains the simulated working-memory dataset used in the article (N = 1,000; two age groups), a saved project file that reproduces all results shown in the paper (open the app, click **Load Project**, and choose `example/mglpa_project.json`). The project file restores the parsed results of every model, and the application regenerates every Mplus input file from the saved settings.

## Example dataset

`example/mglpa_wm_dataset_n1000.csv` (and the Mplus-ready `.dat` version without a header row) is a simulated working-memory dataset generated in R: N = 1,000 (500 younger adults, 500 older adults).

| Role | Variables |
|---|---|
| Profile indicators (z-scores) | `verbal_wm`, `visual_wm`, `updating`, `inhibition`, `proc_speed` |
| Predictors | `education` (years), `cog_reserve` (z-score) |
| Distal outcomes | `task_perf` (percent correct), `everyday_func` (0–100 scale) |
| Grouping variable | `group` (1 = younger adults, 2 = older adults) |

`example/mglpa_project.json` restores every parsed result of the article; the application regenerates all Mplus input files from it.

## About the batch script (`RUN_ALL.bat`)

Each ZIP produced by the app contains the Mplus input files, a `README.txt`, and a small Windows batch file named `RUN_ALL.bat`. The batch file is **optional** and **fully transparent**: it is a plain-text file that contains nothing but one `Mplus "file.inp"` line per input file, for example:

```
@echo off
echo Mplus LPA Batch Runner
Mplus "LPA_Group1_k1_FIXED.inp"
Mplus "LPA_Group1_k2_FIXED.inp"
...
pause
```

It performs no downloads, changes no settings, and needs no administrator rights. Open it in any text editor before running it if you wish. If your institution blocks batch files (Windows SmartScreen, AppLocker, or group policy), you can instead:

1. open each `.inp` file in the Mplus editor and click **Run**, or
2. run them from R with `MplusAutomation::runModels("path/to/folder")` (Hallquist & Wiley, 2018), or
3. on macOS/Linux, run `mplus file.inp` in a terminal for each file.

## Platform and browser notes

- The app is a single HTML/JavaScript page. The offline bundle was verified to load and run all five steps of the example project on Windows 11 and Windows 10 (Chrome 146, Edge 152, and Firefox 152, with the network disabled) and on macOS 26.5 (Safari 26.5.2 and Chrome 152); on Windows 10 the online version was verified in the same three browsers. Linux has not been tested by the authors.
- Mplus itself runs on Windows, macOS, and Linux; the generated input files are identical on all platforms. Only `RUN_ALL.bat` is Windows-specific.
- Data privacy: uploaded data and output files are processed entirely inside your browser and are never sent anywhere. The app contains no analytics or tracking code.

## Repository contents

```
index.html                 the application (single page)
lib/                       vendored JavaScript libraries (JSZip, Chart.js, SheetJS)
fonts/                     vendored web fonts (offline use)
example/                   simulated dataset (.csv/.dat) and the saved project file
CITATION.cff               citation metadata
LICENSE                    MIT (plus third-party notices)
```

## License

MIT License. See `LICENSE` for the full text and for the licenses of the bundled third-party libraries and fonts.
