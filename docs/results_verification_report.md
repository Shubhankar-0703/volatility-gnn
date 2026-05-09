# Results Verification Report

Generated: 2026-05-08


## Audit Scope And Status

- Current final-results notebook: `notebooks/08_final_results.ipynb`
- Final result artifacts refreshed: 2026-05-08
- Main source directory: `data/results/`
- Model test period in final metrics: 103 weekly observations, 2024-2025 test split
- Universe size: 465 stocks
- Correlation graph threshold: `|rho| >= 0.30`
- Transaction cost assumption in portfolio backtests: 10 bps per unit turnover

Notebook scan:

- All notebooks have saved code-cell outputs and no saved error outputs.
- `04c_rank_loss_models.ipynb` has one unexecuted code cell, consistent with the implementation plan showing mixed rank-MSE loss as incomplete.
- Full execution of `03_graphs.ipynb` is slow; the graph-density diagnostics cell was executed directly and its output was saved into the notebook.

Important scope caveat:

- Phases 8-12 in `docs/implementation_priority_plan.md` are not complete. In particular, mixed rank-MSE loss, sector-neutral portfolio construction, ex-sector robustness, and transaction-cost sensitivity grids are still pending. Do not describe the current portfolio results as final robustness results.

## Artifact Map

Use these files as primary sources for paper numbers:

- ML forecast metrics: `data/results/final_ml_metrics_table.csv`
- Ranking metrics: `data/results/final_rank_ic_table.csv`
- Calibration diagnostics: `data/results/final_calibration_summary.csv`
- Portfolio metrics:
  - `data/results/final_portfolio_inverse_vol_metrics.csv`
  - `data/results/final_portfolio_long_short_metrics.csv`
  - `data/results/final_portfolio_vol_target_metrics.csv`
  - `data/results/final_portfolio_minvar_metrics.csv`
- Portfolio turnover summary: `data/results/final_portfolio_turnover_by_strategy_group.csv`
- Graph density:
  - `data/results/correlation_graph_density.csv`
  - `data/results/correlation_graph_density_test_summary.csv`
  - `data/results/correlation_graph_density_summary_by_split.csv`
  - `data/results/correlation_graph_density_quantiles_by_split.csv`
  - `data/results/correlation_graph_density_paper_examples.csv`
- Universe reproducibility: `data/results/universe_reproducibility.csv`
- Within-sector rank IC: `data/results/within_sector_rank_ic_table.csv`
- Oversmoothing audit: `data/results/oversmoothing_audit.csv`

## Paper Model Roster

This is the recommended 14-model roster for the paper tables. The "Paper label" column is a compact display label; the "Saved artifact model name" column is the exact name used in the current `data/results/final_*` CSV files.

| # | Group | Paper label | Saved artifact model name | Notes |
|---:|---|---|---|---|
| 1 | Baseline | HAR per-stock | `HAR per-stock` | Classical HAR fit per stock. |
| 2 | Baseline | HAR pooled | `HAR pooled` | Pooled HAR baseline. |
| 3 | Baseline | LSTM | `LSTM` | Neural sequence baseline without graph or macro features. |
| 4 | Graph, no macro | GNN-Correlation | `GNN-Correlation` | Dynamic rolling correlation graph, 252-day lookback. |
| 5 | Graph, no macro | GNN-Sector | `GNN-Sector` | Canonical sector graph. |
| 6 | Graph, no macro | GNN-Granger | `GNN-Granger` | Return-based Granger graph. |
| 7 | Graph, no macro | GNN-Ensemble | `GNN-Ensemble` | Ensemble of no-macro graph variants. |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | `GNN-Correlation + Macro 21d Tuned` | Tuned macro correlation GNN with 21-day correlation window. |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | `GNN-Correlation + Macro 63d Tuned` | Tuned macro correlation GNN with 63-day correlation window. |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | `GNN-Correlation + Macro 126d Tuned` | Tuned macro correlation GNN with 126-day correlation window. |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | `GNN-Correlation + Macro Tuned` | This is the 252-day macro correlation GNN in final tables; saved artifact name does not include `252d`. |
| 12 | Graph + macro | GNN-Sector + Macro | `GNN-Sector + Macro` | Sector graph with macro/regime features. |
| 13 | Graph + macro | GNN-Granger + Macro | `GNN-Granger + Macro` | Granger graph with macro/regime features. |
| 14 | Graph + macro | GNN-Ensemble + Macro | `GNN-Ensemble + Macro` | Ensemble of macro graph variants. |

Roster verification:

- All 14 rows above are present in `final_ml_metrics_table.csv` and `final_rank_ic_table.csv`.
- The requested `GNN-Corr. + Macro 252d` row should be reported from the saved artifact row `GNN-Correlation + Macro Tuned`.
- The registry also contains exploratory or diagnostic models that are not in this paper roster, including `LSTM + Macro`, untuned `GNN-Correlation + Macro`, rank-loss GNNs, and volatility-Granger variants. Do not mix those into the main paper tables unless explicitly expanding the roster.

## Fourteen-Model Roster Results

These tables report results for the exact 14-model paper roster above. Paper labels are used for readability; the roster section maps each paper label to its saved artifact model name. The `GNN-Corr. + Macro 252d` row is sourced from `GNN-Correlation + Macro Tuned`.

Reusable CSVs generated for these tables:

- `data/results/paper_roster_combined_results.csv`
- `data/results/paper_roster_ml_results.csv`
- `data/results/paper_roster_rank_results.csv`
- `data/results/paper_roster_calibration_results.csv`
- `data/results/paper_roster_inverse_vol_results.csv`
- `data/results/paper_roster_long_short_results.csv`
- `data/results/paper_roster_vol_target_results.csv`
- `data/results/paper_roster_minvar_results.csv`

### Compact Cross-Metric Table

| # | Group | Paper model | ML MSE | ML R2 | Rank Mean IC | Rank ICIR | Cal Calib Slope | Cal Pearson | InvVol Sharpe | LongShort Sharpe | VolTarget Sharpe | MinVar Sharpe |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.032858 | 0.130697 | 0.404936 | 3.472824 | 0.74522 | 0.384737 | 0.395608 | -1.349181 | -0.219266 | 0.634571 |
| 2 | Baseline | HAR pooled | 0.033104 | 0.124177 | 0.392338 | 3.43703 | 0.74293 | 0.375638 | 0.406001 | -1.5828 | -0.20772 | 0.728926 |
| 3 | Baseline | LSTM | 0.032424 | 0.142188 | 0.428819 | 4.362584 | 0.929696 | 0.379302 | 0.411143 | -1.251771 | -0.113978 | 0.563696 |
| 4 | Graph, no macro | GNN-Correlation | 0.032191 | 0.148354 | 0.416509 | 3.439945 | 1.182748 | 0.400731 | 0.386947 | -1.778586 | -0.192804 | 0.581368 |
| 5 | Graph, no macro | GNN-Sector | 0.033631 | 0.110246 | 0.382567 | 3.399371 | 0.93245 | 0.354987 | 0.409682 | -1.259421 | -0.21964 | 0.491816 |
| 6 | Graph, no macro | GNN-Granger | 0.033702 | 0.108378 | 0.374945 | 3.663008 | 0.873121 | 0.344765 | 0.42341 | -1.307475 | -0.199788 | 0.411864 |
| 7 | Graph, no macro | GNN-Ensemble | 0.032012 | 0.153091 | 0.416249 | 3.576932 | 1.158102 | 0.396414 | 0.41679 | -1.291277 | -0.19636 | 0.497707 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.031103 | 0.177119 | 0.412033 | 3.914523 | 0.90689 | 0.441936 | 0.451099 | -1.237288 | -0.085462 | 0.828444 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.029846 | 0.210379 | 0.425997 | 3.656861 | 1.029794 | 0.467461 | 0.429164 | -1.205652 | -0.171687 | 0.794471 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.032124 | 0.150106 | 0.414639 | 3.52464 | 0.700403 | 0.434279 | 0.415177 | -1.217205 | -0.364521 | 0.389468 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.030889 | 0.182801 | 0.428585 | 3.680267 | 0.767841 | 0.449488 | 0.425094 | -1.20161 | -0.358619 | 0.671057 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.031508 | 0.166424 | 0.427789 | 3.85019 | 1.025958 | 0.408898 | 0.468077 | -1.249788 | -0.188537 | 0.983839 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.031439 | 0.168248 | 0.429478 | 4.064242 | 1.478146 | 0.437714 | 0.4561 | -1.250965 | -0.136091 | 0.972609 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.031598 | 0.164034 | 0.437834 | 3.934901 | 1.440601 | 0.432863 | 0.464722 | -1.137323 | -0.142323 | 0.914225 |

### Forecast Metrics

| # | Group | Paper model | MSE | MAE | R2 | DA |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.032858 | 0.112913 | 0.130697 | 0.707005 |
| 2 | Baseline | HAR pooled | 0.033104 | 0.113265 | 0.124177 | 0.702954 |
| 3 | Baseline | LSTM | 0.032424 | 0.109583 | 0.142188 | 0.70878 |
| 4 | Graph, no macro | GNN-Correlation | 0.032191 | 0.107638 | 0.148354 | 0.712225 |
| 5 | Graph, no macro | GNN-Sector | 0.033631 | 0.120846 | 0.110246 | 0.682242 |
| 6 | Graph, no macro | GNN-Granger | 0.033702 | 0.119049 | 0.108378 | 0.687942 |
| 7 | Graph, no macro | GNN-Ensemble | 0.032012 | 0.112685 | 0.153091 | 0.700428 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.031103 | 0.104275 | 0.177119 | 0.724961 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.029846 | 0.104175 | 0.210379 | 0.721704 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.032124 | 0.107972 | 0.150106 | 0.71853 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.030889 | 0.106992 | 0.182801 | 0.71972 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.031508 | 0.11065 | 0.166424 | 0.70427 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.031439 | 0.107706 | 0.168248 | 0.713624 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.031598 | 0.107269 | 0.164034 | 0.714814 |

### Ranking Metrics

| # | Group | Paper model | Mean IC | ICIR | Top Q Hit | Pairwise Acc |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.404936 | 3.472824 | 0.485855 | 0.640652 |
| 2 | Baseline | HAR pooled | 0.392338 | 3.43703 | 0.478406 | 0.635517 |
| 3 | Baseline | LSTM | 0.428819 | 4.362584 | 0.498996 | 0.648435 |
| 4 | Graph, no macro | GNN-Correlation | 0.416509 | 3.439945 | 0.491881 | 0.64485 |
| 5 | Graph, no macro | GNN-Sector | 0.382567 | 3.399371 | 0.478658 | 0.633046 |
| 6 | Graph, no macro | GNN-Granger | 0.374945 | 3.663008 | 0.472631 | 0.629316 |
| 7 | Graph, no macro | GNN-Ensemble | 0.416249 | 3.576932 | 0.493639 | 0.645078 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.412033 | 3.914523 | 0.495815 | 0.642938 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.425997 | 3.656861 | 0.502092 | 0.648073 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.414639 | 3.52464 | 0.490124 | 0.643814 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.428585 | 3.680267 | 0.50226 | 0.649058 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.427789 | 3.85019 | 0.496987 | 0.646565 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.429478 | 4.064242 | 0.493555 | 0.648191 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.437834 | 3.934901 | 0.501255 | 0.65282 |

### Calibration Metrics

| # | Group | Paper model | Calib Slope | Pearson | Avg Weekly Spread |
| --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.74522 | 0.384737 | 0.201685 |
| 2 | Baseline | HAR pooled | 0.74293 | 0.375638 | 0.179925 |
| 3 | Baseline | LSTM | 0.929696 | 0.379302 | 0.167289 |
| 4 | Graph, no macro | GNN-Correlation | 1.182748 | 0.400731 | 0.132478 |
| 5 | Graph, no macro | GNN-Sector | 0.93245 | 0.354987 | 0.164246 |
| 6 | Graph, no macro | GNN-Granger | 0.873121 | 0.344765 | 0.156416 |
| 7 | Graph, no macro | GNN-Ensemble | 1.158102 | 0.396414 | 0.141022 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.90689 | 0.441936 | 0.128612 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 1.029794 | 0.467461 | 0.143083 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.700403 | 0.434279 | 0.164435 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.767841 | 0.449488 | 0.164431 |
| 12 | Graph + macro | GNN-Sector + Macro | 1.025958 | 0.408898 | 0.122484 |
| 13 | Graph + macro | GNN-Granger + Macro | 1.478146 | 0.437714 | 0.102986 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 1.440601 | 0.432863 | 0.100902 |

### Inverse-Volatility Portfolio Metrics

| # | Group | Paper model | Ann Ret | Ann Vol | Sharpe | Turnover |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.099884 | 0.126095 | 0.395608 | 0.16343 |
| 2 | Baseline | HAR pooled | 0.101814 | 0.12762 | 0.406001 | 0.152096 |
| 3 | Baseline | LSTM | 0.102099 | 0.126718 | 0.411143 | 0.075773 |
| 4 | Graph, no macro | GNN-Correlation | 0.099432 | 0.127748 | 0.386947 | 0.078741 |
| 5 | Graph, no macro | GNN-Sector | 0.102729 | 0.128707 | 0.409682 | 0.07185 |
| 6 | Graph, no macro | GNN-Granger | 0.104603 | 0.12896 | 0.42341 | 0.081782 |
| 7 | Graph, no macro | GNN-Ensemble | 0.103489 | 0.128337 | 0.41679 | 0.058086 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.107716 | 0.127945 | 0.451099 | 0.091293 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.104067 | 0.125982 | 0.429164 | 0.0781 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.102192 | 0.12571 | 0.415177 | 0.113454 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.103083 | 0.124872 | 0.425094 | 0.090204 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.110341 | 0.128913 | 0.468077 | 0.05036 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.109673 | 0.130834 | 0.4561 | 0.052625 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.110346 | 0.129855 | 0.464722 | 0.037182 |

### Long-Short Portfolio Metrics

| # | Group | Paper model | Ann Ret | Ann Vol | Sharpe | Turnover |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | -0.168636 | 0.162051 | -1.349181 | 1.329762 |
| 2 | Baseline | HAR pooled | -0.191918 | 0.152842 | -1.5828 | 1.477402 |
| 3 | Baseline | LSTM | -0.15655 | 0.165006 | -1.251771 | 0.764814 |
| 4 | Graph, no macro | GNN-Correlation | -0.264412 | 0.176776 | -1.778586 | 0.967359 |
| 5 | Graph, no macro | GNN-Sector | -0.170451 | 0.175042 | -1.259421 | 0.786073 |
| 6 | Graph, no macro | GNN-Granger | -0.155005 | 0.156795 | -1.307475 | 0.982926 |
| 7 | Graph, no macro | GNN-Ensemble | -0.18493 | 0.181936 | -1.291277 | 0.720121 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | -0.161095 | 0.170611 | -1.237288 | 1.089555 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | -0.174333 | 0.186068 | -1.205652 | 0.902913 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | -0.174401 | 0.184357 | -1.217205 | 0.959659 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | -0.179431 | 0.190936 | -1.20161 | 0.841982 |
| 12 | Graph + macro | GNN-Sector + Macro | -0.165965 | 0.172801 | -1.249788 | 0.641948 |
| 13 | Graph + macro | GNN-Granger + Macro | -0.159735 | 0.167658 | -1.250965 | 0.656177 |
| 14 | Graph + macro | GNN-Ensemble + Macro | -0.149402 | 0.175326 | -1.137323 | 0.551724 |

### Volatility-Targeted Portfolio Metrics

| # | Group | Paper model | Ann Ret | Ann Vol | Sharpe | Turnover | Avg Equity Wt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.038381 | 0.052989 | -0.219266 | 0.076289 | 0.43333 |
| 2 | Baseline | HAR pooled | 0.038825 | 0.053796 | -0.20772 | 0.07338 | 0.436449 |
| 3 | Baseline | LSTM | 0.043681 | 0.055436 | -0.113978 | 0.033529 | 0.437256 |
| 4 | Graph, no macro | GNN-Correlation | 0.03855 | 0.059389 | -0.192804 | 0.052283 | 0.464591 |
| 5 | Graph, no macro | GNN-Sector | 0.039249 | 0.048948 | -0.21964 | 0.028083 | 0.381303 |
| 6 | Graph, no macro | GNN-Granger | 0.039746 | 0.051324 | -0.199788 | 0.036923 | 0.399605 |
| 7 | Graph, no macro | GNN-Ensemble | 0.039624 | 0.052843 | -0.19636 | 0.029493 | 0.413009 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.044876 | 0.059958 | -0.085462 | 0.060411 | 0.485881 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.040364 | 0.056126 | -0.171687 | 0.053148 | 0.46753 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.029476 | 0.056305 | -0.364521 | 0.082925 | 0.458838 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.030044 | 0.055646 | -0.358619 | 0.066119 | 0.449406 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.039856 | 0.053806 | -0.188537 | 0.055731 | 0.445725 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.042588 | 0.054463 | -0.136091 | 0.035823 | 0.435933 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.042194 | 0.054846 | -0.142323 | 0.037153 | 0.4474 |

### Minimum-Variance Portfolio Metrics

| # | Group | Paper model | Ann Ret | Ann Vol | Sharpe | Turnover |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Baseline | HAR per-stock | 0.112598 | 0.098647 | 0.634571 | 1.012136 |
| 2 | Baseline | HAR pooled | 0.124454 | 0.102143 | 0.728926 | 0.913472 |
| 3 | Baseline | LSTM | 0.111802 | 0.109637 | 0.563696 | 0.5914 |
| 4 | Graph, no macro | GNN-Correlation | 0.118337 | 0.117545 | 0.581368 | 0.606021 |
| 5 | Graph, no macro | GNN-Sector | 0.104297 | 0.110402 | 0.491816 | 0.532922 |
| 6 | Graph, no macro | GNN-Granger | 0.096253 | 0.112301 | 0.411864 | 0.675954 |
| 7 | Graph, no macro | GNN-Ensemble | 0.105861 | 0.112236 | 0.497707 | 0.491514 |
| 8 | Graph + macro | GNN-Corr. + Macro 21d | 0.137151 | 0.105198 | 0.828444 | 0.720373 |
| 9 | Graph + macro | GNN-Corr. + Macro 63d | 0.133364 | 0.10493 | 0.794471 | 0.670372 |
| 10 | Graph + macro | GNN-Corr. + Macro 126d | 0.091314 | 0.106079 | 0.389468 | 0.884192 |
| 11 | Graph + macro | GNN-Corr. + Macro 252d | 0.117471 | 0.100544 | 0.671057 | 0.769996 |
| 12 | Graph + macro | GNN-Sector + Macro | 0.152717 | 0.104404 | 0.983839 | 0.406072 |
| 13 | Graph + macro | GNN-Granger + Macro | 0.155692 | 0.108668 | 0.972609 | 0.499408 |
| 14 | Graph + macro | GNN-Ensemble + Macro | 0.149406 | 0.108733 | 0.914225 | 0.357494 |

## Universe And Splits

Universe construction currently uses current Wikipedia S&P 500 constituents, converted to yfinance-compatible symbols, then filtered for at least `config.MIN_COVERAGE`.

From `universe_reproducibility.csv`:

| Field | Value |
|---|---:|
| Final stock count | 465 |
| Minimum coverage threshold | 0.95 |
| Candidate count | Not persisted |

Paper caveat:

- You can report the final universe size as 465.
- Do not report a candidate count unless you regenerate and persist it; the current artifact explicitly says the candidate count was not persisted.

## Forecast Accuracy

Source: `final_ml_metrics_table.csv`

Top 5 models by test MSE:

| Rank | Model | MSE | MAE | R2 | Directional Accuracy |
|---:|---|---:|---:|---:|---:|
| 1 | GNN-Correlation + Macro 63d Tuned | 0.029846 | 0.104175 | 0.210379 | 0.721704 |
| 2 | GNN-Correlation + Macro Tuned | 0.030889 | 0.106992 | 0.182801 | 0.719720 |
| 3 | GNN-Correlation + Macro 21d Tuned | 0.031103 | 0.104275 | 0.177119 | 0.724961 |
| 4 | GNN-Granger + Macro | 0.031439 | 0.107706 | 0.168248 | 0.713624 |
| 5 | GNN-Sector + Macro | 0.031508 | 0.110650 | 0.166424 | 0.704270 |

Baseline comparisons:

| Model | MSE | R2 |
|---|---:|---:|
| HAR per-stock | 0.032858 | 0.130697 |
| HAR pooled | 0.033104 | 0.124177 |
| LSTM | 0.032424 | 0.142188 |
| GNN-Correlation | 0.032191 | 0.148354 |
| GNN-Ensemble | 0.032012 | 0.153091 |

Paper-safe accuracy claim:

> The best point-estimate test MSE is achieved by `GNN-Correlation + Macro 63d Tuned` with MSE `0.029846`, improving on `GNN-Correlation` (`0.032191`), `GNN-Ensemble` (`0.032012`), `LSTM` (`0.032424`), and HAR baselines.


- Do not imply every macro/tuned GNN improvement is broadly supported by all evaluation views.

## Ranking Metrics

Source: `final_rank_ic_table.csv`

Top 5 by mean Rank IC:

| Rank | Model | Mean IC | ICIR | Top-Quartile Hit Rate | Pairwise Accuracy |
|---:|---|---:|---:|---:|---:|
| 1 | GNN-Ensemble + Macro | 0.437834 | 3.934901 | 0.501255 | 0.652820 |
| 2 | GNN-Granger + Macro | 0.429478 | 4.064242 | 0.493555 | 0.648191 |
| 3 | LSTM | 0.428819 | 4.362584 | 0.498996 | 0.648435 |
| 4 | GNN-Correlation + Macro Tuned | 0.428585 | 3.680267 | 0.502260 | 0.649058 |
| 5 | GNN-Sector + Macro | 0.427789 | 3.850190 | 0.496987 | 0.646565 |

Paper-safe ranking claim:

> The strongest Rank IC point estimate is `GNN-Ensemble + Macro` (`0.437834`), but LSTM remains highly competitive (`0.428819`) and has the highest ICIR among the top models (`4.362584`).

Do not claim:

- Do not say GNNs uniformly dominate LSTM on ranking metrics. LSTM is third by mean IC and first among the listed top models by ICIR.

## Calibration

Source: `final_calibration_summary.csv`

Top calibration Pearson correlations:

| Rank | Model | Pearson Corr | Calibration Slope | Avg Weekly Pred Spread p90-p10 |
|---:|---|---:|---:|---:|
| 1 | GNN-Correlation + Macro 63d Tuned | 0.467461 | 1.029794 | 0.143083 |
| 2 | GNN-Correlation + Macro Tuned | 0.449488 | 0.767841 | 0.164431 |
| 3 | GNN-Correlation + Macro 21d Tuned | 0.441936 | 0.906890 | 0.128612 |
| 4 | GNN-Granger + Macro | 0.437714 | 1.478146 | 0.102986 |
| 5 | GNN-Correlation + Macro 126d Tuned | 0.434279 | 0.700403 | 0.164435 |

Calibration interpretation:

- `GNN-Correlation + Macro 63d Tuned` is the best calibration/forecast candidate by combined evidence: best MSE, best Pearson correlation, and slope close to 1 (`1.029794`).
- `GNN-Granger + Macro` has high Pearson correlation but slope `1.478146`, suggesting stronger scale distortion.
- Baseline `GNN-Correlation` slope is `1.182748`; `LSTM` slope is `0.929696`.

Paper-safe calibration claim:

> Macro-enhanced correlation GNNs improve both point accuracy and calibration diagnostics, with `GNN-Correlation + Macro 63d Tuned` showing the highest Pearson correlation (`0.467461`) and a calibration slope close to one (`1.029794`).


- Test statistic operates on weekly model error series.
- `n_weeks = 103`.
- One-sided lower-loss alternative.

Full DM artifact:


| Model | Baseline | DM Stat | p-value | BH p-value | Mean Loss Diff |
|---|---|---:|---:|---:|---:|
| GNN-Correlation + Macro Tuned | HAR per-stock | 3.220623 | 0.000858 | 0.018024 | 0.001969 |
| GNN-Correlation + Macro Tuned | HAR pooled | 3.594487 | 0.000251 | 0.005277 | 0.002216 |

Final displayed summary:


Macro-vs-matched baseline tests:

- Minimum BH-adjusted p-value: 0.284869.


Do not claim:


## Portfolio Results

Sources:

- `final_portfolio_inverse_vol_metrics.csv`
- `final_portfolio_long_short_metrics.csv`
- `final_portfolio_vol_target_metrics.csv`
- `final_portfolio_minvar_metrics.csv`

### Inverse-Volatility Long-Only

Best Sharpe rows:

| Rank | Model | Ann Return | Ann Vol | Sharpe | Max Drawdown | Avg Turnover |
|---:|---|---:|---:|---:|---:|---:|
| 1 | Equal-weight | 0.120309 | 0.137006 | 0.513179 | -0.170673 | 0.000000 |
| 2 | GNN-Sector + Macro | 0.110341 | 0.128913 | 0.468077 | -0.159423 | 0.050360 |
| 3 | GNN-Ensemble + Macro | 0.110346 | 0.129855 | 0.464722 | -0.160978 | 0.037182 |
| 4 | GNN-Granger + Macro | 0.109673 | 0.130834 | 0.456100 | -0.163888 | 0.052625 |
| 5 | GNN-Correlation + Macro 21d Tuned | 0.107716 | 0.127945 | 0.451099 | -0.156197 | 0.091293 |

Paper-safe claim:

> Equal-weight remains the strongest inverse-volatility benchmark by Sharpe (`0.513179`), while the best model-driven inverse-volatility portfolios are macro GNN variants.

### Long-Short

Best Sharpe rows, all negative:

| Rank | Model | Ann Return | Ann Vol | Sharpe | Max Drawdown | Avg Turnover |
|---:|---|---:|---:|---:|---:|---:|
| 1 | GNN-Ensemble + Macro | -0.149402 | 0.175326 | -1.137323 | -0.373281 | 0.551724 |
| 2 | GNN-Correlation + Macro Tuned | -0.179431 | 0.190936 | -1.201610 | -0.370251 | 0.841982 |
| 3 | GNN-Correlation + Macro 63d Tuned | -0.174333 | 0.186068 | -1.205652 | -0.372495 | 0.902913 |

Paper-safe claim:

> Dollar-neutral long-short portfolios perform poorly across all models; even the best long-short Sharpe is negative (`-1.137323` for `GNN-Ensemble + Macro`).

### Volatility-Targeted

Best Sharpe rows, all negative:

| Rank | Model | Ann Return | Ann Vol | Sharpe | Avg Equity Weight | Avg Turnover |
|---:|---|---:|---:|---:|---:|---:|
| 1 | GNN-Correlation + Macro 21d Tuned | 0.044876 | 0.059958 | -0.085462 | 0.485881 | 0.060411 |
| 2 | LSTM | 0.043681 | 0.055436 | -0.113978 | 0.437256 | 0.033529 |
| 3 | GNN-Granger + Macro | 0.042588 | 0.054463 | -0.136091 | 0.435933 | 0.035823 |

Paper-safe claim:

> Volatility targeting reduces volatility but produces negative Sharpe ratios after the risk-free adjustment in the saved metrics.

### Minimum Variance

Best Sharpe rows:

| Rank | Model | Ann Return | Ann Vol | Sharpe | Max Drawdown | Avg Turnover |
|---:|---|---:|---:|---:|---:|---:|
| 1 | GNN-Sector + Macro | 0.152717 | 0.104404 | 0.983839 | -0.097414 | 0.406072 |
| 2 | GNN-Granger + Macro | 0.155692 | 0.108668 | 0.972609 | -0.100859 | 0.499408 |
| 3 | GNN-Ensemble + Macro | 0.149406 | 0.108733 | 0.914225 | -0.098848 | 0.357494 |
| 4 | GNN-Correlation + Macro 21d Tuned | 0.137151 | 0.105198 | 0.828444 | -0.103085 | 0.720373 |
| 5 | GNN-Correlation + Macro 63d Tuned | 0.133364 | 0.104930 | 0.794471 | -0.092194 | 0.670372 |

Concentration caveat:

- Every minimum-variance model has `max_single_stock_weight = 0.05`, the configured cap.
- This should be reported as evidence that the optimizer regularly hits the maximum-weight constraint.

Paper-safe claim:

> Minimum-variance portfolios are the strongest portfolio construction in the current results, led by `GNN-Sector + Macro` with Sharpe `0.983839`; however, all minimum-variance portfolios hit the 5% single-name weight cap, so these results should be interpreted alongside concentration and robustness checks.

## Portfolio Turnover

Source: `final_portfolio_turnover_by_strategy_group.csv`

Average weekly turnover by strategy/model group:

| Strategy | Group | Avg Weekly Turnover | Implied Weekly Cost at 10 bps |
|---|---|---:|---:|
| Inverse-Volatility | Equal-weight | 0.000000 | 0.000000 |
| Inverse-Volatility | HAR | 0.157763 | 0.000158 |
| Inverse-Volatility | LSTM | 0.075773 | 0.000076 |
| Inverse-Volatility | Baseline GNN | 0.072615 | 0.000073 |
| Inverse-Volatility | Macro GNN | 0.073317 | 0.000073 |
| Long-Short | HAR | 1.403582 | 0.001404 |
| Long-Short | LSTM | 0.764814 | 0.000765 |
| Long-Short | Baseline GNN | 0.864120 | 0.000864 |
| Long-Short | Macro GNN | 0.806280 | 0.000806 |
| Volatility-Targeted | HAR | 0.074835 | 0.000075 |
| Volatility-Targeted | LSTM | 0.033529 | 0.000034 |
| Volatility-Targeted | Baseline GNN | 0.036696 | 0.000037 |
| Volatility-Targeted | Macro GNN | 0.055901 | 0.000056 |
| Minimum-Variance | HAR | 0.962804 | 0.000963 |
| Minimum-Variance | LSTM | 0.591400 | 0.000591 |
| Minimum-Variance | Baseline GNN | 0.576603 | 0.000577 |
| Minimum-Variance | Macro GNN | 0.615415 | 0.000615 |

Paper-safe turnover claim:

> Trading intensity differs materially by construction. Inverse-volatility and volatility-targeted portfolios have low average weekly turnover, while long-short and minimum-variance portfolios trade substantially more. This is important when interpreting net returns under the 10 bps transaction-cost assumption.

## Sharpe Bootstrap Intervals


- Baseline/final bootstrap section: 0 positive Sharpe-difference intervals vs available benchmark.
- Macro bootstrap comparisons: 11 positive Sharpe-difference intervals out of 36 matched Sharpe-difference intervals.
- Bootstrap settings in table: `n_bootstrap = 5000`, `block_size = 8`, `n_weeks = 103`.

Interpretation:

- There is some bootstrap support for macro-feature improvements in matched Sharpe comparisons.
- This does not override the mixed portfolio point-estimate story: long-short remains negative, equal-weight beats inverse-vol model portfolios, and minimum-variance needs robustness/concentration checks.

## Graph Density

Sources:

- `correlation_graph_density.csv`
- `correlation_graph_density_test_summary.csv`
- `correlation_graph_density_summary_by_split.csv`
- `correlation_graph_density_quantiles_by_split.csv`
- `correlation_graph_density_paper_examples.csv`

Graph setup:

- Nodes: 465 stocks
- Possible undirected edges: 107,880
- Threshold: `|rho| >= 0.30`

Test-period summary:

| Metric | Mean | Median | Min | Max |
|---|---:|---:|---:|---:|
| Edges | 36,048.8447 | 28,719 | 18,775 | 55,566 |
| Density | 0.3342 | 0.2662 | 0.1740 | 0.5151 |
| Mean degree | 155.0488 | 123.5226 | 80.7527 | 238.9935 |
| Max degree | 327.6117 | 305 | 225 | 417 |
| Avg abs correlation | 0.2469 | 0.2239 | 0.1873 | 0.3104 |

Split-level density:

| Split | Mean | Median | Min | Max |
|---|---:|---:|---:|---:|
| Train | 0.5408 | 0.5306 | 0.0915 | 0.9386 |
| Validation | 0.6637 | 0.6966 | 0.3796 | 0.8109 |
| Test | 0.3342 | 0.2662 | 0.1740 | 0.5151 |

Selected examples:

| Example | Week | Split | Edges | Density | Mean Degree | Avg Abs Corr |
|---|---:|---|---:|---:|---:|---:|
| Sparse 2018 week | 2018-01-22 | train | 9,876 | 0.0915 | 42.4774 | 0.1471 |
| COVID crisis week | 2020-03-23 | train | 100,609 | 0.9326 | 432.7269 | 0.5465 |
| Calm 2024 test week | 2024-12-09 | test | 18,775 | 0.1740 | 80.7527 | 0.1873 |

Paper-safe graph-density claim:

> The rolling correlation graph is highly regime-sensitive. In the 2024-2025 test period, graph density averaged `0.3342` and ranged from `0.1740` to `0.5151`. During the COVID shock, density reached `0.9326` on 2020-03-23 and peaked at `0.9386` in April 2020, making the graph nearly complete.

Oversmoothing link:

> Dense graph regimes increase the number of neighbors aggregated by each node. The COVID example has mean degree `432.7`, versus `42.5` in a sparse 2018 week and `80.8` in a calm 2024 test week. This supports the oversmoothing limitation: message passing may wash out stock-specific information when correlations become market-wide.

## Oversmoothing Diagnostics

Source: `oversmoothing_audit.csv`

Key observed pattern:

- For `GNN-Correlation`, representative layer distances shrink sharply from layer 1 to layer 3.
- Example `calm_2017`, baseline correlation GNN:
  - Layer 1 mean pairwise embedding distance: `4.468418`
  - Layer 2: `3.058154`
  - Layer 3: `0.580244`
  - Final-to-layer1 distance ratio: `0.129854`
- Example `covid_2020`, baseline correlation GNN:
  - Layer 1: `2.505151`
  - Layer 2: `1.243011`
  - Layer 3: `0.675119`
  - Final-to-layer1 distance ratio: `0.269492`

Paper-safe claim:

> The embedding audit shows substantial compression across GNN layers, consistent with oversmoothing risk. This is a diagnostic limitation rather than proof that oversmoothing alone explains all performance differences.

## Within-Sector Rank IC

Source: `within_sector_rank_ic_table.csv`

Top rows:

| Rank | Model | Mean Within-Sector Rank IC | Std | Sector-Weeks |
|---:|---|---:|---:|---:|
| 1 | LSTM + Macro | 0.405431 | 0.199262 | 1133 |
| 2 | GNN-Ensemble + Macro | 0.396954 | 0.201794 | 1133 |
| 3 | GNN-Correlation + Macro | 0.390949 | 0.204766 | 1133 |
| 4 | GNN-Sector + Macro | 0.389621 | 0.202634 | 1133 |
| 5 | LSTM | 0.388212 | 0.198508 | 1133 |

Caveat:

- This table includes models not in the final paper roster, including `LSTM + Macro` and non-tuned macro variants.
- If using within-sector IC in the paper, filter to the final model roster or explain that this is a broader diagnostic table.

Paper-safe claim:

> Within-sector rank diagnostics show that ranking signal is not purely sector-level, but LSTM and macro variants remain competitive. This supports a nuanced graph-value story rather than unconditional GNN dominance.

## Macro Feature Claims

Sources:

- `final_macro_vs_baseline_deltas.csv`

Point-estimate ML deltas:

- `GNN-Correlation + Macro 63d Tuned` vs `GNN-Correlation`: delta MSE `-0.002344`, delta R2 `+0.062025`, delta directional accuracy `+0.009479`.
- `GNN-Correlation + Macro Tuned` vs `GNN-Correlation`: delta MSE `-0.001302`, delta R2 `+0.034448`.
- `GNN-Granger + Macro` vs `GNN-Granger`: delta MSE `-0.002263`, delta R2 `+0.059870`.
- `GNN-Sector + Macro` vs `GNN-Sector`: delta MSE `-0.002123`, delta R2 `+0.056179`.
- `LSTM + Macro` vs `LSTM`: delta MSE `+0.002421`, delta R2 `-0.064044`.


Paper-safe macro claim:


## Recommended Claims To Use

Strongest supported claims:

1. `GNN-Correlation + Macro 63d Tuned` has the best point-estimate test MSE (`0.029846`).
2. `GNN-Ensemble + Macro` has the best mean Rank IC (`0.437834`).
4. Equal-weight beats model-driven inverse-volatility portfolios by Sharpe.
5. Minimum-variance portfolios led by macro GNNs have the best portfolio Sharpe point estimates, but they hit the 5% max-weight cap.
6. Long-short portfolios are negative across all models.
7. Graph density is regime-sensitive and becomes nearly complete during COVID.
8. Turnover differs materially by strategy and should be reported with portfolio tables.

Claims to avoid or soften:

1. Avoid: "GNNs dominate all baselines."
   Better: "Macro-enhanced graph models produce the strongest point-estimate forecast metrics, while portfolio results are construction-dependent."
2. Avoid: "Macro features improve every model."
3. Avoid: "Better forecasts automatically produce better portfolios."
   Better: "Forecast accuracy, ranking, calibration, turnover, and portfolio construction do not move one-for-one."
4. Avoid: "Final portfolio robustness is complete."
   Better: "Portfolio robustness checks remain future/current-scope work, including sector-neutral and ex-sector tests."

## Open Verification Gaps

- Candidate universe count is missing from persisted artifacts.
- Mixed rank-MSE loss has not been implemented/evaluated.
- Sector-neutral and ex-sector portfolio robustness checks are not complete.
- Transaction cost sensitivity grid `{0, 5, 10, 25, 50}` is not complete.
- Minimum-variance concentration diagnostics need more than max-weight if used heavily in paper: effective number of holdings, HHI, sector weights.
- Current results appear single-seed for neural models; multi-seed uncertainty should be acknowledged if not added.

## Recommended Next Task

Per `docs/implementation_priority_plan.md`, the next unchecked task is:

> 8. Implement mixed rank-MSE loss.

For paper verification specifically, the highest-value next audit additions are:

1. Add sector-neutral and ex-sector portfolio robustness checks.
2. Add transaction-cost sensitivity.
3. Add concentration diagnostics for minimum-variance portfolios.
