# massMatchR user manual

**massMatchR** is an interactive R Shiny application for matching mass spectrometry (MS) peak lists against a user-defined reference database within a specified tolerance window. This manual provides comprehensive instructions on preparing input files, understanding all application controls, and exporting results.

If you are interested only in running the application locally, please refer to the [Local Installation Guide](docs/LOCAL_INSTALL.md)

---

## Table of Contents

1. [How MassMatchR Works](#1-how-massmatchr-works)
2. [Input Files](#2-input-files)
   - [Reference Database](#reference-database)
   - [Sample Data](#sample-data)
   - [Molecular Structure Images](#molecular-structure-images)
3. [App Layout Overview](#3-app-layout-overview)
4. [Sidebar — Global Controls](#4-sidebar--global-controls)
   - [Reference Database Section](#reference-database-section)
   - [Sample Data Section](#sample-data-section)
   - [Matching Settings](#matching-settings)
5. [Tab: Analyze Data → Samples](#5-tab-analyze-data--samples)
   - [Analysis Controls](#analysis-controls)
   - [Sample Options Panel](#sample-options-panel)
   - [Samples → Table](#samples--table)
   - [Samples → Plot](#samples--plot)
6. [Tab: Analyze Data → Groups](#6-tab-analyze-data--groups)
   - [Group Assignment](#group-assignment)
   - [Group Options Panel](#group-options-panel)
   - [Groups → Group Table](#groups--group-table)
   - [Groups → Group Plot](#groups--group-plot)
7. [Tab: Explore Reference Data](#7-tab-explore-reference-data)
8. [Exporting Results](#8-exporting-results)

---

## 1. How MassMatchR Works

MassMatchR performs **tolerance-based nearest-neighbour m/z matching**:

1. The reference database is loaded and deduplicated by m/z (rounded to 4 decimal places). Rows without an m/z value are ignored.
2. For each reference m/z value, the app checks all sample files and looks for peaks within ± *tolerance* of the reference m/z value. If several peaks fall within this range, the one closest to the reference m/z is chosen, and its intensity is recorded. If no peak is found within the tolerance range, the intensity is set to 0 (no match).
3. Glycans that are not detected in any sample are removed from the results.

---

## 2. Input Files

### Reference Database

**File type:** Microsoft Excel file (`.xlsx`).<br>
**Sheets:** 1 sheet only (if multiple sheets are present, only the first sheet is read).<br>
**Header row:** the first row must contain column names. These will appear in the sidebar under "Pair from columns" panel.

**Minimum required columns**
- **Names column**: glycan name / label (e.g., `H5N4F1`). This column also can be left empty.
- **m/z column**: theoretical or expected m/z values used for matching.

**After uploading the reference file:**
- rows with a missing (`NA`) m/z value are automatically skipped,
- duplicate m/z values (after rounding to 4 decimal places) are deduplicated, keeping only the first occurrence,
- all available columns will be listed in the sidebar under "Pair from columns" panel.

**From the available columns, you will have to specify three:**
- `Names:` → column index containing glycan names (can point to an empty column)
- `m/z:` → column index containing theoretical m/z values (used for matching the sample data)
- `Images:` → column index containing image IDs (used to display glycan structures in the plots)

This structure allows users to create a reference databse e.g. with unmodified monoisotopic values, but also with different modifications like permethylation, etc. Thanks to this, there is no need to create different reference databses with different modifications. Also, this structure allows you to match the sample data with the reference databse via permthylated m/z, and pair the glycan structures from un-modified m/z.

#### Notes on Reference Database Structure

This flexible structure allows users to create a single reference database containing, for example, unmodified monoisotopic m/z values as well as values corresponding to modifiecations (e.g., permethylation).

As a result, there is no need to create separate reference databases for different modification types. It also allows to match sample data using modified (e.g., permethylated) m/z values while displaying glycan structures based on the unmodified m/z values.

---

### Sample Data

**File type:** Microsoft Excel file (`.xlsx`).<br>
**Sheets:** multi-sheet workbook.<br>
**Rule:** one sheet = one sample.

Each sheet must contain at least:
- an **m/z** column (measured peaks),
- an **Intensity** column.

All sheets should have the **same column structure** (same headers and column order).

In the sidebar you control how the sheets are read:
- **Start reading at row:** row number containing the *header* (default `1`),
- **m/z column:** column index containing measured m/z values,
- **Intensity column:** column index containing intensity values.

This flexible setup allows you to use measured data exported from other softwares directly, without modifying the original column structure.

In the main panel, you can select:
- **Number of samples:** number of sheets to use (starting from sheet 1). The app also shows how many sheets (samples) are available in the uploaded file.

By default, sample names are taken from the *sheet names*. You can rename them in the UI.

---
