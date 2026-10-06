# Honest validation of machine-learning models for biofilters

Code and data for the paper

> **How reliable are machine-learning models of biofilters? Validation leakage, naive baselines and physics-based alternatives for VOC and H₂S biofiltration**
> Behrad Mashadi Zamani Jalayer and Alireza Rezaei, submitted to *Journal of Environmental Chemical Engineering* (2026).

The notebook reproduces every number, table and figure in the paper and supplementary material from two public experimental datasets, in a single run.

## What the study shows

- **Random splits inflate accuracy.** Random train/test splits of biofilter data make machine-learning (ML) models look much better than they are: when whole operating phases, reactors or factor levels are held out, their errors become about 2–15 times larger.
- **Simple mechanistic models transfer better.** A two-parameter inhibition model carries over to a new reactor better than tree ensembles do.
- **Nothing beats persistence for forecasting.** For 5- to 15-day-ahead forecasts, no model or mechanistic–ML hybrid beats simply repeating the last measurement.
- **Tuning doesn't change this.** All conclusions hold under nested hyperparameter tuning, and for every start day and forecast horizon tested.

## Contents

| File | Description |
|---|---|
| `biofilter_full_analysis.ipynb` | The complete analysis (Kaggle / Jupyter notebook) |
| `data/toluene_cofeed_clean.csv` | Dataset A: toluene removal with ethyl acetate or n-hexane in two biotrickling filters, 72 samples, days 20–195. Transcribed from Supporting Information S1 of Xue et al. (2024). |
| `results/` | All tables (CSV) and figures (PNG) written by the notebook |
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

The notebook finds the input files automatically, wherever they are mounted. Outputs are written to `/kaggle/working/results`. A full run takes about 15–20 minutes on a standard CPU; the nested-tuning section is the slowest.

**Locally:**

```bash
git clone https://github.com/YOUR-USERNAME/biofilter-ml-honest-validation.git
cd biofilter-ml-honest-validation
pip install -r requirements.txt
# download DataSet.xlsx from the Figshare link above into data/
jupyter nbconvert --to notebook --execute biofilter_full_analysis.ipynb --output executed.ipynb
```

The notebook looks for the data files in `data/` (or the notebook folder). Outputs go to `results/`.

Software used: Python 3.11, NumPy, pandas, SciPy, scikit-learn 1.x, XGBoost 2.x, matplotlib. All random seeds are fixed.

## Notebook sections → paper

| Section | Content | Paper |
|---|---|---|
| A1–A2 | Dataset A; mechanistic model with competitive inhibition | Fig. 2 |
| A3 | Random 5-fold vs leave-one-phase-out vs cross-reactor validation, with block-bootstrap statistics | Table 3, Fig. 3a |
| A4–A5 | Forward-in-time forecasting, hybrids, adaptive mechanistic model | Table 6, Figs. 4–5 |
| B1–B2 | Dataset B: random vs leave-one-condition / moisture-level / EBRT-level-out | Table 4, Figs. 3b and 6 |
| C1 | Inflation ratios | Fig. 3 |
| D1 | Nested hyperparameter tuning | Section 3.4, Table 5, Table S2 |
| D2 | Forecast sensitivity to start day and horizon | Table S1, Fig. S1 |
| D3 | Random-split variability, drop-one-block robustness, fitted parameters | Tables S4–S6 |

## Citation

If you use this code, please cite the paper (details to be added on publication) and the two original data sources:

- Xue, X., Wang, H., Zhai, J., Nan, X. (2024). Biofiltration of toluene in the presence of ethyl acetate or n-hexane: Performance and microbial community. *PLOS ONE* 19(5): e0302487. https://doi.org/10.1371/journal.pone.0302487
- Zarei, M., Bayati, M.R., Rohani, A., Ebrahimi-Nik, M., Hejazi, B. (2025). Modeling and experimental evaluation of biochar-mediated biofiltration for hydrogen sulfide capture from biogas. *PLOS ONE* 20(12): e0339352. https://doi.org/10.1371/journal.pone.0339352

We thank both groups for publishing their raw data, which made this re-analysis possible.
