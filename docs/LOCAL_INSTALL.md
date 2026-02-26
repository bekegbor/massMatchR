
# Local Installation Guide

This guide explains how to install dependencies and run **MassMatchR** on your own computer (Windows/macOS/Linux).

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Required R Packages](#2-required-r-packages)
3. [Directory Structure](#3-directory-structure)
4. [Browser Configuration](#4-browser-configuration)
   - [4.1 Windows: Set a Specific Browser Executable (Optional)](#41-windows-set-a-specific-browser-executable-optional)
   - [4.2 macOS / Linux](#42-macos--linux)
5. [Running the App](#5-running-the-app)

---

## 1. Prerequisites

- Download and install **R** (≥ 4.1.0) [https://www.r-project.org/](https://www.r-project.org/).
- A modern web browser (Google Chrome or Mozilla Firefox recommended)

---

## 2. Required R Packages

Install all dependencies by running the following in your R console:

```r
install.packages(c(
  "shiny",
  "openxlsx",
  "processx",
  "shinyBS",
  "shinyjs",
  "htmlwidgets",
  "DT",
  "tableHTML",
  "ggplot2",
  "ggrepel",
  "colourpicker",
  "plotly"
))
```

---

## 3. Directory Structure

Download the latest release as a ZIP file and extract it. The following folder structure is expected:

```
MassMatchR/
├── global.R            ← sets browser path and loads reference file list
├── run.R               ← launches the app
├── ui.R                ← user interface definition
├── server.R            ← application logic and matching engine
└── www/
    ├── images/         ← molecular structure PNG images (named by m/z)
    │   ├── 386.355.png
    │   ├── 702.567.png
    │   └── ...
```

To use your own images, copy them into the `www/images/` folder. For detailed instructions on naming and file format, see the [Glycan Structures](MANUAL.md#glycan-structures) section in the [User Manual](MANUAL.md).

---

## 4. Browser configuration

### 4.1 Windows: set a specific browser executable (optional)

In `global.R`, Chrome is set as the default browser:

```r
options(browser = "C:/Program Files/Google/Chrome/Application/chrome.exe")
```

If you would like to switch to Firefox, comment out the line above and uncomment the following line:

```r
# options(browser = "C:/Program Files/Mozilla Firefox/firefox.exe")
```

If these paths do not match your system, you can either:
- update the path to match your browser installation, or
- comment out the line entirely.

### 4.2 macOS / Linux

Usually, you **do not** need to set `options(browser = ...)`.  
If your system does not automatically open a browser, you can either:

- set `options(shiny.launch.browser = TRUE)` and let R choose the default browser, or  
- set `options(browser = "/usr/bin/firefox")` (or a similar path to your preferred browser).

---

## 5. Running the App

**Option A — using `run.R` (recommended):**

Open R, set the working directory to the `massMatchR/` folder, then run:

```r
setwd("/path/to/MassMatchR")
shiny::runApp(appDir = getwd())
```

**Option B — using `shiny::runApp()`:**

Open R, set the working directory to the `massMatchR/` folder, then run:

```r
setwd("/path/to/MassMatchR")   # adjust path
source("run.R")
```

The app starts on port `8100` and should open automatically in the configured browser.

---

