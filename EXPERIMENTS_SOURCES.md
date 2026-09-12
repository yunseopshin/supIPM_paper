# Experiments section: figure and table sources

Written 2026-09-12. Every figure/table in `iclr2027/iclr2027_conference.tex` Sections 5, C (synthetic), D (real data) and E (ablation) comes from `Fair representation/sup_IPM/results_summary/` (built by `scripts/make_main_figs.py`, `scripts/make_appendix_figs.py`, `scripts/make_tables.py`; loader `scripts/summary_data.py`). Paper terms: "prediction-level L∞-GDP" = results_summary "own-head"; "worst-case downstream L∞-GDP over F_c" = results_summary "worst-head R_c"; "norm bound c" = "gain budget c".

## Main text (figures in `figures/main/`)

| Paper | File | Source |
|---|---|---|
| Fig. simulation (5.1) | `main/fig1_simulation.pdf` | `results_summary/main/fig1_simulation.pdf` (SynthD, c=1, training bandwidth) |
| Fig. adult (5.2) top / bottom | `main/fig2_own_head_adult.pdf`, `main/fig3_worst_head_adult.pdf` | `results_summary/main/` same names |
| Fig. crime-acs (5.2) | `main/fig3_worst_head_crime.pdf`, `main/fig3_worst_head_acs.pdf` | `results_summary/main/` same names |

## Appendix figures (`figures/appendix/`)

| Paper label | Files | Source |
|---|---|---|
| fig:app-prediction-level | `fig2_own_head_crime.pdf`, `fig2_own_head_acs.pdf` | `results_summary/main/` |
| fig:app-c4, fig:app-c16 | `figA2_worst_head_budgets_{adult,crime,acs}_c{4,16}.pdf` | `results_summary/appendix/` |
| fig:app-norm-bound | `figA1_gain_budget_curve.pdf` | `results_summary/appendix/` (utility floors Adult 0.840, Crime 0.855, ACSIncome 0.775, Simulation 0.900) |
| fig:app-gdp / fig:app-hgr / fig:app-supipm | `figA6_other_measures_{adult,crime,acs}_{gdp_w_kernel,hgr,sup_ipm}.pdf` | `results_summary/appendix/` (mi_y_s, mi_z_s, gdp_wo_kernel not used) |
| fig:app-graph | `figA7_graph_own_head.pdf`, `figA7_graph_worst_head.pdf` | `results_summary/appendix/` |
| fig:app-discriminator | `figA4_critic_ablation.pdf` | `results_summary/appendix/` (SynthC) |
| fig:app-bandwidth | `figA5_bandwidth.pdf` | `results_summary/appendix/` |

All figures in the paper are re-rendered at paper size (three-panel figures 7.4×2.4 in, two-panel 6.4×2.5 in, fonts 6.5–8 pt, no dataset suptitle) with paper wording in the labels, by `sup_IPM/scripts/make_main_figs_paper.py` → `results_summary/main_paper/` and `sup_IPM/scripts/make_appendix_figs_paper.py` → `results_summary/appendix_paper/`, then copied here. The originals in `results_summary/main/` and `results_summary/appendix/` are untouched. Not used: `figA3_axis_variants` (removed from the repo).

## Appendix tables (four, FREM-style)

| Label | Content | Source |
|---|---|---|
| tab:datasets | dataset table | `src/data.py`, `FRL-GDP-full/src/base/datasets.py` (input dim 102/122), run logs (Adult 32,561/12,661; Crime fold-based 1,794–1,795/199–200; ACSIncome 12,800/4,000 train/test after 20,000 subsample, input dim 9); Pokec sizes from Kong et al. Table 3 |
| tab:hyperparameters | training settings per dataset | `configs/default.yaml`, `configs/graph.yaml`, as-run `config.yaml` under `results/critic20/{Adult-age,Crime-racepctblack}/supipm`, `results/ACSIncome-age/supipm` (J_v = 2, α_v = 1e-3), `results/e1d-tau/...`, `results/pokec_z-GCN-AGE/supipm-cs10` (batch 1024, 100 epochs, wd 0, J_v = 10) |
| tab:matched | both disparities at one matched utility level, Adult 0.842 and ACSIncome 0.780, best in bold | regenerated `T1_headline.csv` (`results_summary/appendix_paper/`), rows "Adult,0.842" and "ACSIncome,0.780"; Crime omitted because FREM has no row at any Crime level |
| tab:discriminator-ablation | J_v = 2 vs 20 on the first generator (SynthC) | `T4_critic_ablation.csv` |

Numbers quoted in prose instead of tables: λ grids and run counts (synthetic: `results/e1d-tau/...` dirs, T5), runs remaining per dataset (T5, regenerated), worst-case ascent settings (`EXPERIMENT_SPEC.md` §4.2, `tools/stress_head.py`), the LAFTR/ADV penalty diagnostics (T6, regenerated: LAFTR ρ −0.33…0.00 on Adult/Crime with 13–32/50 runs; ADV on ACSIncome min/max 0.0614/0.0641; every other tabular method ρ ≤ −0.84).

Not used: `T2_ranking_inversion.csv`, `T3_gain_budget.csv` (read at a different utility level than figA1), the full T1 (89 rows), T5 and T6 as tables — cut on 2026-09-12 at the author's request to keep only a few important tables, as in FREM's appendix.

**Table regeneration.** The committed `results_summary/appendix/T*.csv` (2026-09-11 22:16) predate the last figure rebuild (2026-09-12 12:4x) and are stale for Adult (supIPM-FRL now has 14 λ values, 58/75 runs; T1 Adult rows at levels 0.835/0.838 changed; T5 Adult runs kept 545; T6 Adult supIPM-FRL row ρ = −0.84, seed CV 0.15). `make_tables.py` was re-run on 2026-09-12 and the fresh CSV/MD files are in `results_summary/appendix_paper/`; tab:ranking, tab:coverage and tab:penalty-diagnostics use them. The originals in `results_summary/appendix/` were left untouched.

## Caveats

- ACSIncome supIPM-FRL runs are the J_v = 2 (weak discriminator) runs; the retrained J_v = 20 runs exist in `results/critic20/ACSIncome-age/supipm` but have no worst-case measurement, so the figures use the first runs. Stated in Appendix D.2 and E.1.
- No TeX toolchain on ideaserver2: the document was not compiled here. Check page count on Overleaf; the main-text Experiments section was written to about 2.1 pages (Sections 1–4 end at ≈6.2 pages).
