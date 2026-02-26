## create_lightcurve.ipynb

### Input
1) **RAW video**
- `VIDEO_PATH` → path to the Seestar `.avi` (RAW video) made with Nina. The script will read the video frame by frame, extract the brightness of the target star, and detect the occultation event.

2) **Seestar metadata block**
- `META_TEXT` → paste the Seestar log text (the script parses at least `RA [ICRS]`, `Dec [ICRS]`, `DATE-OBS`, `SITELONG`, `SITELAT`, `OBJECT`)

3) **Timing**
- `EXPOSURE_TIME` (seconds)
- `READOUT_GAP` (seconds)
- `DT_FRAME = EXPOSURE_TIME + READOUT_GAP`
- `LOG_TIME_IS_MID_EXPOSURE` (`True` if `DATE-OBS` corresponds to mid-exposure, else `False`)

4) **Output naming**
- `OUTPUT_SUFFIX` → label used in all output filenames

## CSV output schema (exact)

The pipeline writes two CSV files:
- `lightcurve_<OUTPUT_SUFFIX>.csv` (streaming write during processing)
- `lightcurve_<OUTPUT_SUFFIX>_final.csv` (final, post-processed)

Both files have the **same header** and column order.

### Columns (in order)

| # | Column | Type | Meaning |
|---:|---|---|---|
| 1 | `frame` | int | Frame index (0-based). |
| 2 | `time_start_isot` | str | Exposure start time (ISO-8601). |
| 3 | `time_mid_isot` | str | Exposure mid time (ISO-8601). |
| 4 | `time_end_isot` | str | Exposure end time (ISO-8601). |
| 5 | `flux_target` | float | Background-subtracted aperture flux of the target. |
| 6 | `flux_comp_sum` | float | Sum of background-subtracted aperture fluxes of all comparison stars. |
| 7 | `flux_rel` | float | Relative flux: `flux_target / flux_comp_sum`. |
| 8 | `dmag_target` | float | Differential magnitude from normalized relative flux (filled in `_final.csv`). |
| 9 | `flux_target_norm` | float | Normalized `flux_target` (relative to a reference median; filled in `_final.csv`). |
| 10 | `flux_ref_norm` | float | Normalized `flux_comp_sum` (reference; filled in `_final.csv`). |
| 11 | `bkg_norm` | float | Normalized background trend (filled in `_final.csv`). |
| 12 | `bkg_target_perpix` | float | Target background estimate per pixel (from annulus). |
| 13 | `bkg_comp_median_perpix` | float | Median background per pixel across comparison stars. |
| 14 | `median_dmag_target` | float | Global median of `dmag_target` (repeated per row; filled in `_final.csv`). |
| 15 | `inliers` | int | Number of RANSAC inliers for the affine transform on that frame. |
| 16 | `aff_dx` | float | Estimated x-shift (pixels) from affine fit relative to reference. |
| 17 | `aff_dy` | float | Estimated y-shift (pixels) from affine fit relative to reference. |

### Notes
- `flux_rel = flux_target / flux_comp_sum`
- The `_final.csv` is the canonical output for analysis/plotting.