## ocultation.ipynb

- **FITS folder**: `folder = "Ago08/"` (all files in this folder are read, show be raws from seestar already DEBAYERED - (May implement debayering in code if needed))
- **Target (ICRS, deg)**:
  - `TARGET_RA = 161.2628`
  - `TARGET_DEC = -59.6837`
- **Comparison stars** (sky-fixed):
  - Detected on first frame; selected as the first 10 stars, then **uses `N_COMP = 9`** for the lightcurve.
- **Photometry**:
  - `AP_RADIUS = 5.0`, `ANN_IN = 8.0`, `ANN_OUT = 14.0`
- **Detection**:
  - `DAO_FWHM = 3.0`, `DAO_SIGMA = 3.5`, `MIN_FLUX = 50`

#### Outputs

- **CSV written**: `Ago08.csv`

#### CSV columns (exact)

Core columns:
- `frame`
- `date_obs`
- `target_flux`
- `target_bkg`
- `target_net`
- `comp_net_median`
- `rel_flux_target_over_comp_median`
- `ratio_mean`
- `ratio_median`
- `n_valid_comp`
- `rel_flux_norm1`

Per-comparison star columns (**i = 1..9** because `N_COMP = 9`):
- `star_i_flux`
- `star_i_bkg`
- `star_i_net`
- `star_i_ratio`