
# Local Installation Guide

This guide explains how to install dependencies and run **MassMatchR** on your own computer (Windows/macOS/Linux).

## Prerequisites

- Download and install **R** (≥ 4.1.0) [https://www.r-project.org/](https://www.r-project.org/).
- A modern web browser (Google Chrome or Mozilla Firefox recommended)

## Required R Packages

Install all dependencies by running the following in your R console:

### Required R Packages

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

### Directory Structure

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

### Configuring the Default Browser

The app runs on a local port (`8100` by default). The browser to open automatically is configured in `global.R`:

```r
# global.R — uncomment/edit the line matching your system

# Google Chrome on Windows (64-bit)
# options(browser = "C:/Program Files/Google/Chrome/Application/chrome.exe")

# Mozilla Firefox on Windows (default)
options(browser = "C:/Program Files/Mozilla Firefox/firefox.exe")

# macOS — Chrome
# options(browser = "open -a 'Google Chrome'")

# macOS — Firefox
# options(browser = "open -a Firefox")

# Linux — default system browser
# options(browser = "xdg-open")
```

Edit `global.R` to uncomment and correct the path for your operating system and preferred browser **before** starting the app.

If you prefer to open the URL manually, simply comment out all `options(browser = ...)` lines and navigate to `http://localhost:8100` in your browser after starting the app.

---

### Running the App

**Option A — using `run.R` (recommended):**

Open R or RStudio, set the working directory to the `MassMatchR/` folder, then run:

```r
setwd("/path/to/MassMatchR")   # adjust path
source("run.R")
```

The app starts on port `8100` and should open automatically in the configured browser.

**Option B — using `shiny::runApp()`:**

```r
library(shiny)
setwd("/path/to/MassMatchR")
runApp(appDir = getwd(), port = 8100L, launch.browser = TRUE)
```

**Option C — directly from RStudio:**

Open any of the four R files (`global.R`, `ui.R`, `server.R`, or `run.R`) in RStudio. A **"Run App"** button will appear in the top-right corner of the editor pane. Click it to launch.

> **Port conflicts:** If port `8100` is already in use, change `port = 8100L` to another free port (e.g. `port = 8200L`) in both `run.R` and your browser bookmark.

---
