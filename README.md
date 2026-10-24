# Honest validation of machine-learning models for biofilters

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23614289.svg)](https://doi.org/10.5281/zenodo.23614289)

Code and data for the paper

> **How reliable are machine-learning models of biofilters? Validation leakage, naive baselines and physics-based alternatives for VOC and H₂S biofiltration**
> Behrad Mashadi Zamani Jalayer, Alireza Rezaei, Arshia Borazjani and Alireza Shamsi Saeedabad.
> Submitted to *Environmental Modelling & Software* (2026).

One notebook reproduces every number, table and figure in the paper and the supplementary material from two public experimental datasets, in a single run.

## What the study shows

- **Random splits inflate accuracy.** When whole operating phases, reactors or factor levels are held out, the errors of flexible ML models become about 2–15 times larger than under random splits.
- **Simple mechanistic models transfer better.** A two-parameter inhibition model carries over to a new reactor better than tree ensembles do.
- **Process knowledge helps only in the model structure.** A physics model with a Gaussian-process residual correction interpolates best and is never less accurate than the physics model alone; a monotonicity constraint on XGBoost does not fix extrapolation.
- **Persistence is hard to beat.** For 5- to 15-day-ahead forecasts, no mechanistic, ML or hybrid model beats repeating the last measurement; only exponential smoothing matches it.
- **Leakage also breaks uncertainty estimates.** Nominal 90 % conformal intervals calibrated by random cross-validation cover as few as 11 % of withheld observations; grouped calibration restores coverage.
- **The inflation is predictable.** A simulation study shows it is negligible without replication and grows with the number of replicates per condition.

All conclusions hold under nested hyperparameter tuning and for every forecast start day and horizon tested.

## Contents

| File | Description |
|---|---|
| `biofilter_full_analysis.ipynb` | The complete analysis (Kaggle / Jupyter notebook), with outputs |
| `data/toluene_cofeed_clean.csv` | Dataset A: toluene removal with ethyl acetate or n-hexane in two biotrickling filters, 72 samples, days 20–195. Transcribed from Supporting Information S1 of Xue et al. (2024). |
| `results/` | All tables (CSV) and figures (PNG) written by the notebook |
| `CITATION.cff`, `.zenodo.json` | Citation and archive metadata |
| `LICENSE` | MIT licence for the code |

**Dataset B is not redistributed here.** It is the H₂S biochar biofilter dataset, 54 runs (18 conditions × 3 replicates). Download `DataSet.xlsx` from the original authors' repository and place it in `data/`:
Zarei et al. (2025), Figshare, https://doi.org/10.6084/m9.figshare.30266404

### Columns of `toluene_cofeed_clean.csv`

| Column | Meaning |
|---|---|
| `reactor` | A (co-fed ethyl acetate) or B (co-fed n-hexane) |
| `cofeed` | Name of the co-pollutant |
| `phase` | Operating phase I–IV |
| `day` | Day of operation |
| `EBRT_s` | Empty-bed residence time (s) |
| `toluene_in`, `toluene_out` | Toluene inlet and outlet concentration (mg/m³) |
| `toluene_RE_reported`, `toluene_RE_calc` | Removal efficiency (%): as reported, and recalculated from the concentrations |
| `cofeed_in`, `cofeed_out` | Co-pollutant inlet and outlet concentration (mg/m³) |
| `cofeed_RE_reported`, `cofeed_RE_calc` | Co-pollutant removal efficiency (%) |
| `source` | Sheet of the original S1 file |

An outlet concentration of 0 for ethyl acetate in reactor A, phase II, means the value was below the detection limit.

## How to run

**On Kaggle (as used for the paper):**

1. Create a Kaggle dataset containing `toluene_cofeed_clean.csv` and `DataSet.xlsx`.
2. Open the notebook and add that dataset as an input.
3. Choose **Run → Run all**.

The notebook finds the input files automatically. Outputs are written to `/kaggle/working/results`, and the last cells zip them for download and print the software versions. A full run takes about 1 hour on a standard CPU; the simulation study (Part E) and nested tuning (Part D) are the slowest parts.

**Locally:**

```bash
git clone https://github.com/behrad2003/biofilter-ml-honest-validation.git
cd biofilter-ml-honest-validation
pip install -r requirements.txt
# download DataSet.xlsx from the Figshare link above into data/
jupyter nbconvert --to notebook --execute biofilter_full_analysis.ipynb --output executed.ipynb --ExecutePreprocessor.timeout=-1
```

The notebook looks for the data files in `data/` (or the notebook folder); outputs go to `results/`.
Software used for the paper: Python 3.12 (Kaggle), NumPy, pandas, SciPy, scikit-learn, XGBoost, matplotlib, openpyxl (exact versions are printed by the last notebook cell). All random seeds are fixed.

## Notebook sections → paper

| Section | Content | Paper |
|---|---|---|
| A1–A2 | Dataset A; mechanistic model with competitive inhibition | Fig. 2 |
| A3 | Random 5-fold vs leave-one-phase-out vs cross-reactor validation, block-bootstrap statistics | Table 3, Fig. 3a |
| A4–A5 | Forward-in-time forecasting, hybrids, adaptive mechanistic model | Table 7, Figs. 5–6 |
| B1–B2 | Dataset B: random vs leave-one-condition / moisture-level / EBRT-level-out | Table 4, Figs. 3b and 4 |
| C1 | Inflation ratios | Section 3.2 |
| D1 | Nested hyperparameter tuning | Section 3.4, Table 5, Table S2 |
| D2 | Forecast sensitivity to start day and horizon | Table S1, Fig. S1 |
| D3 | Random-split variability, drop-one-block robustness, fitted parameters | Tables S4–S6 |
| E1 | Statistical forecast benchmarks (AR(1), exponential smoothing, drift) | Table 7, Table S7 |
| E2 | Monotone XGBoost and physics + Gaussian-process hybrid | Table 6, Table S8 |
| E3 | Split-conformal prediction intervals, random vs grouped calibration | Fig. 7, Table S9 |
| E4 | Simulation study (replication and autocorrelation) | Fig. 8 |

Figure files → figure numbers in the paper: `Fig2_A_mechanistic_trajectories.png` = Fig. 2, `Fig3_random_vs_honest_both.png` = Fig. 3, `Fig6_B_error_by_level.png` = Fig. 4, `Fig4_A_forecasts.png` = Fig. 5, `Fig5_A_adaptive_model.png` = Fig. 6, `Fig7_conformal_coverage.png` = Fig. 7, `Fig8_simulation_inflation.png` = Fig. 8, `FigS1_forecast_sensitivity.png` = Fig. S1. (Fig. 1 is the study-design diagram drawn in the manuscript.)

## Citation

Please cite the paper (details to be added on publication) and this archive:

- Mashadi Zamani Jalayer, B., Rezaei, A., Borazjani, A., Shamsi Saeedabad, A. (2026). Code, data and results for: How reliable are machine-learning models of biofilters? (version 1.2.1). Zenodo. https://doi.org/10.5281/zenodo.23183080

and the two original data sources:

- Xue, X., Wang, H., Zhai, J., Nan, X. (2024). Biofiltration of toluene in the presence of ethyl acetate or n-hexane: Performance and microbial community. *PLOS ONE* 19(5): e0302487. https://doi.org/10.1371/journal.pone.0302487
- Zarei, M., Bayati, M.R., Rohani, A., Ebrahimi-Nik, M., Hejazi, B. (2025). Modeling and experimental evaluation of biochar-mediated biofiltration for hydrogen sulfide capture from biogas. *PLOS ONE* 20(12): e0339352. https://doi.org/10.1371/journal.pone.0339352

We thank both groups for publishing their raw data, which made this re-analysis possible.
