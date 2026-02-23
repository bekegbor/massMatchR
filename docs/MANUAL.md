# MassMatchR user manual

This manual describes how to use **MassMatchR** (Shiny app) for matching MALDI‑TOF MS peak lists to a user‑provided reference database and for comparing **samples** and **groups**.

> Tip: If you only want to run the app locally, jump to `docs/LOCAL_INSTALL.md`.

---

## 1. What MassMatchR does

Given:

- a **reference table** of theoretical masses (m/z) with glycan names (and optional structure images)
- one or more **experimental samples** (m/z + intensity), stored as sheets in an Excel workbook

MassMatchR:

1. matches each reference mass to the **closest** experimental mass in each sample
2. keeps the match **only if** it is within the user‑defined tolerance (± tolerance)
3. outputs results as:
   - a **table** (exportable as `.xlsx`)
   - interactive **bar plots** (Plotly) for samples and groups
   - an optional reference browser tab with structure thumbnails

### Matching logic (important)
For every reference entry (row):

- find the experimental peak with minimal `abs(reference_mz - sample_mz)`
- if that peak is inside `[reference_mz - tol, reference_mz + tol]` → keep its m/z and intensity
- otherwise → set intensity to `0` (no match)

---

## 2. Input files

MassMatchR uses **two** Excel files (`.xlsx`):

1. **Reference database**
2. **Sample dataset**

### 2.1 Reference database (.xlsx)

**File type:** Microsoft Excel Workbook (`.xlsx`)  
**Sheets:** must contain the reference data in **Sheet 1**  
**Header row:** the first row should contain column names

**Minimum required columns**
- **Names column**: glycan name / label (e.g., `H5N4F1`)
- **m/z column**: theoretical or expected m/z to match against

**Optional column**
- **Images column**: numeric IDs used to look up PNG structures in `www/images/`

In the app you will specify these columns using the three numeric inputs:

- `Names:` → column index for glycan names
- `m/z:` → column index for theoretical m/z
- `Images:` → column index for image IDs (optional but recommended if you want structure overlays)

> The app shows you the available column names under **“Pair from columns”** after uploading the reference file.

#### Notes on the Images column
- The Images column is expected to be **numeric** (the app rounds it to 3 decimals when looking for a PNG).
- For easiest use, set the image ID equal to the theoretical mass (m/z), so the same ID works everywhere.

### 2.2 Sample data (.xlsx)

**File type:** Microsoft Excel Workbook (`.xlsx`)  
**Sheets:** **multi‑sheet** workbook  
**Rule:** **each sheet = one sample**

Each sheet must contain at least:

- an **m/z** column (measured peaks)
- an **Intensity** column

All sheets should have the **same column structure** (same headers / order), because you select columns by **index**.

In the sidebar you control how the sheets are read:

- **Start reading at row:** the row that contains the *header* (default `1`)
- **m/z column:** column index of measured masses
- **Intensity column:** column index of intensities
- **Number of samples:** how many sheets to use (starting from sheet 1)

> Sample names default to the *sheet names*. You can rename them in the UI.

---

## 3. Optional structure images

### 3.1 Where images go (local app)
Put PNG files here:

```
www/images/
```

### 3.2 File naming rule
The app expects filenames like:

```
<image_id_rounded_to_3_decimals>.png
```

Examples:

- `1579.783.png`
- `2729.326.png`

Where `<image_id_rounded_to_3_decimals>` is taken from the **Images column** of your reference file (rounded to 3 decimals).

> Important: In the **plots**, the app looks for an *exact* filename of the form `<value>.png`.  
> If you use suffixes like `_1.png`, they may show in the reference browser but not in plots.

### 3.3 Where images are used
- **Explore reference data** tab: displays thumbnails in a table (hover to zoom).
- **Plots**: if “Structures” is enabled, images are shown above bars.

---

## 4. Walkthrough of the interface

A labeled interface overview is shown in `docs/images/image1.png`.

### 4.1 Sidebar (left)

1. **Upload reference database file (.xlsx)**
2. **Select reference columns**
   - Names (column index)
   - m/z (column index)
   - Images (column index; optional)
3. **Upload data file (.xlsx)** (multi‑sheet)
4. **Define which row/columns to read**
5. **Tolerance** (± Da)
6. **Intensity**
   - **Absolute**: keep the intensity values as provided
   - **Relative**: compute percentages per sample (see below)

### 4.2 Analyze data tab

This is the main analysis view, split into **Samples** and **Groups**.

#### Data filtering / selection controls
- **Recalculate (Yes/No)**  
  Controls how relative intensities are re‑normalized when you exclude m/z values:
  - **Yes**: relative intensities are computed using only the currently selected m/z values
  - **No**: relative intensities keep the original denominator

- **Filter by min. value**  
  Values below this threshold are hidden in plots (set to `NA` for plotting).  
  The underlying tables/exports are not permanently removed.

- **Select/Deselect m/z**  
  Controls which glycans are included in the results table and relative calculations.

- **Select/Deselect Info**  
  Controls which glycans show plot annotations (names/structures/percentages).

### 4.3 Samples view

#### Table tab
- Shows the matched results table (reference + per‑sample matched masses/intensities)
- **Export table** downloads `sample_table.xlsx`

#### Plot tab
Interactive Plotly bar plot.

You can customize:
- plot title
- axis font sizes and rotation
- show/hide:
  - **Structures**
  - **Names**
  - **Percentage**
- structure and text sizes
- spacing between bars and annotations (“Image space”)

> Note: the UI contains “Save settings” (format/width/resolution), but this repository version does not currently include a plot download button/handler.

#### Sample options
- Rename samples (semicolon‑separated list)
- Set a color per sample

### 4.4 Groups view

Requires **at least 2 samples**.

1. Set **Number of Groups**
2. Assign samples to groups using checkboxes
3. Optionally rename groups and adjust group colors

**Group Table**
- Export as `group_table.xlsx`

**Group Plot**
- Similar to Sample plot, but bars represent group means
- Optional variability display:
  - **SEM** (standard error of mean)
  - **SD** (standard deviation)

---

## 5. Output and exports

### 5.1 Sample table export (`sample_table.xlsx`)
Includes (typical):
- reference columns: `Name`, `m/z` (and internal `rn`)
- for each sample:
  - `<SampleName>_m/z`
  - `<SampleName>_Intensity_relative` or `_Intensity_absolute`

### 5.2 Group table export (`group_table.xlsx`)
Includes:
- `Name`, `m/z`
- for each group:
  - `<GroupName>_<relative|absolute>_Intensity`
  - `<GroupName>_<relative|absolute>_SD`
  - `<GroupName>_<relative|absolute>_SEM`

---

## 6. Example files

This repository ships with small example inputs:

- `example_data/reference_example.xlsx`
- `example_data/sample_example.xlsx`

And matching placeholder PNGs in:

- `www/images/`

Use them to verify your local installation works.

---

## 7. Troubleshooting

### “Please upload a reference database file.”
Make sure you uploaded a `.xlsx` file and it has at least one sheet.

### “Sheet number must be between 1 and N”
Your sample workbook has fewer sheets than the “Number of samples” setting.

### No matches / table is empty
Common reasons:
- tolerance too small
- wrong m/z column selected
- wrong start row (header row not read correctly)
- reference m/z values are not comparable to your measured m/z values (different adduct/charge)

### Structure images don’t appear
Check:
- images are in `www/images/`
- filenames match exactly `<image_id rounded to 3 decimals>.png`
- the reference file “Images” column is numeric and points to the correct IDs

---

## 8. Getting help

- Check `docs/LOCAL_INSTALL.md` for setup issues.
- If you publish this repository on GitHub, use the **Issues** tab for bug reports and feature requests.
