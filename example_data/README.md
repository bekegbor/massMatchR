# Example data for MassMatchR

This folder contains small test inputs you can use to verify that the app runs locally.

## Files

### `reference_example.xlsx`
Single-sheet reference database with 3 columns:

1. `Name`
2. `m/z`
3. `ImageID` (numeric, used to map to `www/images/<ImageID>.png`)

### `sample_example.xlsx`
Multi-sheet workbook with 3 sheets (`Sample1`, `Sample2`, `Sample3`).

Each sheet contains:

- column 1: `m/z`
- column 2: `Intensity`

## How to test

1. Run the app (see `docs/LOCAL_INSTALL.md`)
2. Upload `reference_example.xlsx`
3. Set:
   - Names column = 1
   - m/z column = 2
   - Images column = 3
4. Upload `sample_example.xlsx`
5. Set:
   - Start reading at row = 1
   - m/z column = 1
   - Intensity column = 2
   - Number of samples = 3
   - Tolerance = 0.5

You should see a filled table and bar plots with placeholder structure images.
