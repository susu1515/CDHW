# A response-sensitive moisture window shapes soil moisture loss during global compound drought-heatwave events

This repository contains the analysis code and supporting materials for the manuscript:

**A response-sensitive moisture window shapes soil moisture loss during global compound drought-heatwave events**

The main reproducible entry point is:

```text
code/paper2_methods_order_code_20260611.ipynb
```

This notebook is organized strictly according to the final manuscript and supplement dated `20260611`. In that version, the ExtraTrees-SHAP section appears before the pre-onset hydrothermal pathway analysis, and the notebook follows that order.

## Repository structure

```text
code/
  paper2_methods_order_code_20260611.ipynb   # main methods-ordered notebook
  code.ipynb                                 # legacy analysis notebook
  fig.ipynb                                  # legacy figure notebook

data/                                        # processed/intermediate data products
result/                                      # generated figures and result files
uncertainty/                                 # uncertainty and validation analyses
modify/敏感区/
  code/                                      # final response-sensitive-region and SHAP scripts
  data/                                      # final sensitive-region and SHAP summary data
  figures/                                   # final supplementary and SHAP figures
  reports/                                   # run manifests and validation reports
```

Legacy notebooks are retained for traceability, but the final analysis should be followed through `paper2_methods_order_code_20260611.ipynb` and the final scripts under `modify/敏感区/code/`.

## Analysis workflow

The final notebook follows the paper methods in this order:

1. Identify compound drought-heatwave (CDHW) events using daily SPEI and Tmax thresholds.
2. Calculate CDHW frequency, duration, intensity and Time of Climate Impact Emergence (TCIE).
3. Define available soil moisture content (ASM) from CWatM soil moisture, field capacity and wilting point.
4. Quantify CDHW-related soil moisture depletion and convert ASM response slopes to physical water-loss units.
5. Identify CDHW event-window soil moisture response-sensitive regions using controlled event baselines.
6. Run event-level ExtraTrees-SHAP predictive diagnostics for ASM anomalies.
7. Diagnose pre-onset hydrothermal pathway types using event-level ETa and Tmax changes.
8. Reproduce Supplementary A-E utilities, validation and uncertainty analyses.

## Final sensitive-region and SHAP code

The final response-sensitive-region and SHAP analyses are **not** taken from the old notebook SHAP cells. They are stored in:

```text
modify/敏感区/code/
```

Key scripts include:

```text
run_event_window_specific_framework.py
compute_A_mask_robustness.py
analyze_event_level_critical_window.py
make_landcover_metric_trigger_figures.py
run_event_level_original_consistent_m0_m1_m2_shap_parallel.py
redraw_fig4_rank_scaled_colors.py
```

The SHAP results should be interpreted as model-based predictive diagnostics of fitted ASM anomaly responses, not as physical causal contributions.

## Data requirements

The full workflow requires large gridded climate, hydrological and validation datasets that are not necessarily suitable for direct GitHub storage. Required inputs include:

- GSWP3-W5E5 daily meteorological forcing.
- Daily precipitation, Tmax and reference evapotranspiration/PET.
- CWatM soil moisture outputs and soil hydraulic parameters.
- Field capacity and wilting-point water-storage products.
- CDHW, drought-only and heatwave-only event masks.
- Land-cover fractions used by CWatM.
- SREX region masks.
- ERA5 soil moisture, ISMN observations and scPDSI data for uncertainty checks.

Some cells preserve the original local absolute paths used during analysis. Before running on another machine, update the path definitions at the beginning of the notebook and inside individual scripts where needed.

## Environment

The workflow was developed in Python/JupyterLab. Core packages include:

```text
xarray
numpy
pandas
dask
scipy
statsmodels
scikit-learn
shap
matplotlib
cartopy
geopandas
regionmask
h5netcdf
netCDF4
tqdm
```

For large NetCDF operations, use a machine with sufficient memory and configure Dask workers according to local resources.

## Running the workflow

Open the final notebook in JupyterLab:

```bash
jupyter lab code/paper2_methods_order_code_20260611.ipynb
```

Run the notebook section by section. Do not blindly run all cells, because some steps:

- start local Dask clusters,
- read large NetCDF/CSV files,
- train ExtraTrees models,
- calculate SHAP values,
- write or overwrite output files.

For the final SHAP workflow, use the script under `modify/敏感区/code/` and set the paper-facing model parameters explicitly when reproducing the manuscript analysis.

## Reproducibility notes

- CDHW response-sensitive regions are defined as response after CDHW occurrence, not CDHW exposure or antecedent triggering sensitivity.
- Trigger-sensitive regions are analyzed separately from event-window response-sensitive regions.
- ASM is a relative available-water metric; physical water loss is obtained only after conversion using local field-capacity and wilting-point storage.
- SHAP values summarize fitted prediction structure and should not be described as causal physical attribution.

## License and citation

Please cite the associated manuscript when using this code. Add the final DOI, journal reference and license information after publication.
