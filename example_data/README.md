# Example data for MassMatchR

This folder contains test inputs you can use to verify that the app runs locally.

## Files

### [`reference_example.xlsx`](reference_example.xlsx)

Single-sheet reference database with 3 columns:

1. `Name`
2. `m/z` (theoretical m/z values retrieved from [Kronewitter et al., 2009](https://doi.org/10.1002/pmic.200800760))
3. `perMe` (permethylated values calculated from theoretical)

### [`sample_example.xlsx`](sample_example.xlsx)

Multi-sheet workbook with 6 sheets (`SLC35A2-CDG 1`, `SLC35A2-CDG 2`, `SLC35A2-CDG 3`, `Control 1`, `Control 2`, `Control 3`) with data from [Kodríková et al., 2023](https://doi.org/10.3390/biomedicines11020580).

### [`images`](images)

Contains 39 PNG images for testing, generated in GlycoWorkbench. These depict the most probable glycan structures and should be considered strictly theoretical.

## How to test

1. Run the app (see `docs/LOCAL_INSTALL.md`)
2. Upload [`reference_example.xlsx`](reference_example.xlsx)
3. Set:
   - Names column = 1
   - m/z column = 3
   - Images column = 3
4. Upload [`sample_example.xlsx`](sample_example.xlsx)
5. Set:
   - Start reading at row = 1
   - m/z column = 1
   - Intensity column = 3
   - Number of samples = 6
   - Tolerance = 0.7

You can explore the matched results and plot.

## Citations

[Kodríková, R., Pakanová, Z., Krchňák, M., Šedivá, M., Šesták, S., Květoň, F., Beke, G., Šalingová, A., Skalická, K., Brennerová, K., Jančová, E., Baráth, P., Mucha, J., & Nemčovič, M. (2023). N-Glycoprofiling of SLC35A2-CDG: Patient with a Novel Hemizygous Variant. *Biomedicines*, 11(2), 580.](https://doi.org/10.3390/biomedicines11020580)

[Kronewitter, S. R., An, H. J., de Leoz, M. L., Lebrilla, C. B., Miyamoto, S., & Leiserowitz, G. S. (2009). The development of retrosynthetic glycan libraries to profile and classify the human serum N-linked glycome. *Proteomics*, 9: 2986–2994.](https://doi.org/10.1002/pmic.200800760)
