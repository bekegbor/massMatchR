# massMatchR user manual

**massMatchR** is an interactive R Shiny application for matching mass spectrometry (MS) peak lists against a user-defined reference database within a specified tolerance window. This manual provides comprehensive instructions on preparing input files, understanding all application controls, and exporting results.

If you are interested only in running the application locally, please refer to the [Local Installation Guide](LOCAL_INSTALL.md)

---

## Table of Contents

1. [How MassMatchR Works](#1-how-massmatchr-works)
2. [Input Files](#2-input-files)
   - [Reference Database](#reference-database)
   - [Sample Data](#sample-data)
   - [Glycan Structures](#glycan-structures)
3. [UI Overview](#3-ui-overview)
   - [Sidebar](#sidebar)
   - [Analyze Data Tab](#analyze-data-tab)
     - [Data Filtering/Selection Controls](#data-filteringselection-controls)
     - [Samples Tab](#samples-tab)
       - [Table Tab](#table-tab)
       - [Plot Tab](#plot-tab)
     - [Groups Tab](#groups-tab)
       - [Group Assignment](#group-assignment)
       - [Group Options Panel](#group-options-panel)
       - [Group Table Tab](#group-table-tab)
       - [Group Plot Tab](#group-plot-tab)
   - [Explore Reference Data tab](#explore-reference-data-tab)
  
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

You can download and test an example reference file here:  
[reference_example.xlsx](../example_data/reference_example.xlsx)

---

### Sample Data

**File type:** Microsoft Excel file (`.xlsx`).<br>
**Sheets:** multi-sheet workbook.<br>
**Rule:** one sheet = one sample.

**Each sheet must contain at least:**
- an **m/z** column (measured peaks),
- an **Intensity** column.

All sheets should have the **same column structure** (same headers and column order).

**In the sidebar you control how the sheets are read:**
- **Start reading at row:** row number containing the *header* (default `1`),
- **m/z column:** column index containing measured m/z values,
- **Intensity column:** column index containing intensity values.

This flexible setup allows you to use measured data exported from other softwares directly, without modifying the original column structure.

**In the main panel, you can select:**
- **Number of samples:** number of sheets to use (starting from sheet 1). The app also shows how many sheets (samples) are available in the uploaded file.

By default, sample names are taken from the *sheet names*. You can rename them in the UI.

You can download and test an example reference file here:  
[sample_example.xlsx](../example_data/sample_example.xlsx)

---

### Glycan Structures

To display glycan structures, the image filenames must match either:
- the glycan names from the reference database, or
- an m/z value from any column of the reference database (rounded to 4 decimal places).


Example filenames:
- `H5N4F1.png`
- `1579.783.png`

The filename (without the extension) must correspond to the values specified in the **Images** column of the reference file.

You can verify whether images are correctly paired with the reference database in the **Explore Reference Data** tab. This tab displays image thumbnails in a table (hover over an image to zoom).

In the **Plots** section, structures can be displayed above the bars when the **"Structures"** option is enabled.

---

## 3. UI Overview

### Sidebar

The **Sidebar** (left panel) contains the following controls:

- **Upload reference database file (.xlsx)**
- **Pair from columns:** Lists all available column names from the reference file. From these, you must select:
  - **Names** (column index)
  - **m/z** (column index)
  - **Images** (column index)
- **Upload data file (.xlsx)** (multi-sheet workbook)
- **Start reading at row:** Select the row in your sample file that contains the header.
- **m/z column:** Column index containing measured m/z values.
- **Intensity column:** Column index containing intensity values.
- **Tolerance:** Matching window in ± Da (default: 0.5).
- **Intensity display mode:**
  - **Absolute:** Use intensity values as provided.
  - **Relative:** Calculate percentages per sample.

---

### Analyze Data Tab

This is the main analysis view. It is divided into:

- **Data Filtering/Selection Controls**
- **Samples**
- **Groups**

---

#### Data Filtering/Selection Controls

These settings apply to both **Samples** and **Groups**.

- **Recalculate (Yes/No)**  
  Controls how relative intensities are recalculated when m/z values are excluded:
  - **Yes:** Relative intensities are calculated using only the currently selected m/z values. The total per sample will always sum to 100%.
  - **No:** Relative intensities retain the original denominator.

- **Filter by minimum value**  
  Values below this threshold are hidden in plots but remain in the results table.

- **Select/Deselect m/z**  
  Determines which glycans are included in the results table and relative calculations.

- **Select/Deselect Info**  
  Determines which annotations (names, structures, percentages) are displayed above the bars in plots.

---

#### Samples Tab

Here you can configure:

- **Number of samples:** Number of sheets to analyze (starting from sheet 1).

In the **Sample Options** panel, you can:

- **Rename samples:**  
  Sample names default to the sheet names in your Excel file.  
  You can rename them by entering names separated by `;`  
  Example:  
  `Sample1;Sample2;Control;`

- **Define sample color:**  
  Choose the bar color for each sample in the plot.

---

##### Table Tab

- Displays the matched results table (reference data + matched sample intensities).
- **Export table:** Downloads the results as `sample_table.xlsx`.

---

##### Plot Tab

Displays an interactive Plotly bar chart.

In the **Plot Settings**, you can customize:

**Plot Title Options**
- Plot title (updates when clicking the “Name plot” button)
- Plot title font size

**Axis Label Options**
- X-axis label rotation
- Axis label font size
- Axis tick font size

**Display Options (above bars)**

You can show or hide:

- **Structures**
  - Image size
- **Names**
  - Font size
  - Angle
- **Percentages**
  - Font size
- **Image space:** Controls spacing between bars and annotations.

---

#### Groups Tab

The **Groups** tab allows you to combine individual samples into user-defined groups and compare aggregated results. This is useful for summarizing biological replicates or experimental conditions.

For each group, the app calculates the **mean intensity** of the matched glycans across the assigned samples. You can choose to display variability using either **standard deviation (SD)** or **standard error of the mean (SEM)**.

---

##### Group Assignment

- **Number of groups:** Select how many groups you want to define.

---

##### Group Options Panel

In the **Group Options** panel, you can:

- **Rename groups:**  
  Enter custom group names separated by `;`  
  Example:  
  `Control;Treatment;Knockout;`

- **Define group colors:**  
  Select the color used for each group in the plots.

---

##### Group Table Tab

- Displays the grouped results table.  
- Intensity values represent the **mean intensity per group**.
- For each group, both **SD** and **SEM** are calculated.
- **Export table:** Downloads the results as `group_table.xlsx`.

---

##### Group Plot Tab

Displays an interactive Plotly bar chart showing aggregated group values.

In the **Plot Settings**, you can customize:

**Plot Title Options**
- Plot title
- Plot title font size

**Axis Label Options**
- X-axis label rotation
- Axis label font size
- Axis tick font size

**Error Bars (SD/SEM)**
- Select whether **SD** or **SEM** is displayed in the plot.

**Display Options (above bars)**

You can show or hide:

- **Structures**
  - Image size
- **Names**
  - Font size
  - Angle
- **Percentages**
  - Font size
- **Image space:** Controls spacing between bars and annotations.

---

### Explore Reference Data tab

This tab displays the full reference database as a scrollable table with three columns:

- **Name** — glycan name from the selected *Names* column  
- **m/z** — reference m/z value  
- **Structure** — thumbnail image of the matched images (hover to zoom)

This tab is useful for:

- Confirming that the reference file was read correctly  
- Checking which glycans have associated structure images  
- Browsing the database before starting the analysis

