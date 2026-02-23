# MassMatchR

**MassMatchR** is an interactive R Shiny application for matching mass spectrometry (MS) peak lists against a user-defined reference database. It enables researchers to identify compounds across multiple samples simultaneously, visualize relative or absolute signal intensities, annotate bar charts with molecular structure images, and export publication-ready figures and tables — all without writing a single line of code.

---

## Table of Contents

1. [Overview](#overview)
2. [Where to Find and Use It](#where-to-find-and-use-it)
3. [Input File Requirements](#input-file-requirements)
   - [Reference Database File](#reference-database-file)
   - [Sample Data File](#sample-data-file)
   - [Molecular Structure Images](#molecular-structure-images)
4. [App Walkthrough and Manual](#app-walkthrough-and-manual)
   - [Sidebar — Global Controls](#sidebar--global-controls)
   - [Tab: Analyze Data → Samples](#tab-analyze-data--samples)
   - [Tab: Analyze Data → Groups](#tab-analyze-data--groups)
   - [Tab: Explore Reference Data](#tab-explore-reference-data)
5. [Local Installation](#local-installation)
   - [Prerequisites](#prerequisites)
   - [Required R Packages](#required-r-packages)
   - [Directory Structure](#directory-structure)
   - [Configuring the Default Browser](#configuring-the-default-browser)
   - [Running the App](#running-the-app)
6. [Citation](#citation)
7. [License](#license)

---

## Overview

MassMatchR compares measured m/z (mass-to-charge) peak lists against a reference compound database using a user-defined tolerance window. For each compound in the reference, the closest measured peak within ± tolerance is located in each sample. The matched intensities are then displayed as grouped bar charts (one bar per sample per compound), with optional overlays of molecular structure images, compound names, and percentage labels. Samples can be freely grouped for between-group comparisons.
