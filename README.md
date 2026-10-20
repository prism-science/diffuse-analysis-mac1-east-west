[![DOI](https://zenodo.org/badge/1425008409.svg)](https://doi.org/10.5281/zenodo.23487401)

# Analysis of diffuse scattering data for apo Mac1 (SARS-CoV-2 nsp3 macrodomain)

This repository contains the Jupyter notebooks used to analyze the processed diffuse scattering maps and to generate the figures in:

Zielinski K, Meisburger S, Lu K, Bassim V, Holton J, Fraser JS, Ando N. (2026). Iterating diffuse scattering protocols in parallel leads to reproducible, high-quality data from Mac1. The Stacks. [DOI: 10.82153/hrpy-t681](https://doi.org/10.82153/hrpy-t681)

It is a [DataLad](https://www.datalad.org) dataset. The processed data are taken from the companion repository [diffuse-processing-mac1-east-west](https://github.com/prism-science/diffuse-processing-mac1-east-west), included as the subdataset `processing/`. Large files are tracked with git-annex and are publicly available through anonymous access.

## Contents

| Directory | Description |
|---|---|
| `analysis/` | Notebooks that read `processing/results` and compute the statistics, correlations, and map slices compared in the article. Most write their outputs to `figures/source_data/`. |
| `figures/` | Notebooks `fig2_*` through `fig6_*` that plot the figures from `figures/source_data/` (CSV and HDF5) and write PDF, PNG, and SVG files to `figures/export/`. |
| `processing/` | Subdataset with processing scripts and results (maps, MTZ files, reports, logs). |
| `env.yaml` | Conda environment for the notebooks. |

## Figures

Each figure notebook plots data written by an analysis notebook. Datasets are named as in `processing/results/<collection>/<dataset>`.

| Figure | Plot notebooks (`figures/`) | Source-data notebook (`analysis/`) | Processed data |
|---|---|---|---|
| 2 | `fig2_cc`, `fig2_iso_aniso` | `compare_map_correlations` (CC); `compare_map_statistics` (isotropic/anisotropic) | `mac1-east-west`: `als_20251015`, `als_20251125`, `chess_20250924`, `chess_20251105` |
| 3 | `fig3_slices`, `fig3_snr` | `compare_xtal_number_split` | `mac1-east-west/chess_20250924/combos` |
| 4 | `fig4_charts` | `gather_dials_stats` (humidity section) | DIALS logs of ALS crystals (`mac1-suboptimal/als_20250701` and `mac1-east-west`) |
| 5 | `fig5_cc`, `fig5_iso_aniso`, `fig5_slices` | `compare_dose_split` | `mac1-east-west/als_20251015` (50 ms) vs. `mac1-suboptimal/als_20251015` (100 ms), first and second halves (`mac1_merge_lo`, `mac1_merge_hi`) |
| 6 | `fig6_iso_aniso`, `fig6_slices`, `fig6_snr` | `compare_xtal_size` | `mac1-east-west/chess_20251105` vs. `mac1-suboptimal/als_20260423` |

## Usage

Install [DataLad](https://handbook.datalad.org/en/latest/intro/installation.html) (which includes git-annex) and conda/mamba, then:

```bash
datalad clone https://github.com/prism-science/diffuse-analysis-mac1-east-west.git
cd diffuse-analysis-mac1-east-west
mamba env create -f env.yaml -n diffuse-analysis
mamba activate diffuse-analysis
python -m ipykernel install --user --name=diffuse-analysis --display-name="Python (diffuse-analysis)"
```

**Regenerate the figures from the archived source data.** Fetch the source data and run the notebooks in `figures/`:

```bash
datalad get figures/source_data
```

**Reproduce the analysis from the processed maps.** Install the processing subdataset and fetch the files the notebooks read, then run the notebooks in `analysis/`:

```bash
datalad get -n processing
find processing/results \( -name "merged.nxs" -o -name "*.json" -o -name "dials.*.log" \) \
    -exec datalad get {} +
```

Notebooks load results using relative paths (`../processing/results`), so run them from their own directories. To recompute the processed data from the raw images, see the [processing repository](https://github.com/prism-science/diffuse-processing-mac1-east-west).

## License

MIT; see [LICENSE](LICENSE).
