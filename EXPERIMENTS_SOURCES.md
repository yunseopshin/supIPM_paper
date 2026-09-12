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

Re-rendered with paper wording (no "worst head"/"budget"/"critic"/"bump" in labels) into `results_summary/appendix_paper/` and copied here; the originals in `results_summary/appendix/` are untouched. Not used: `figA3_axis_variants` (removed from the repo).

## Appendix tables

| Label | Content | Source |
|---|---|---|
| tab:synth-settings | simulation settings | `EXPERIMENT_SPEC.md` §1.2, §3; run dirs `results/e1d-tau/tau-0.30/SynthD-synth_s/{supipm,frem-gr1.0-gs*}` (λ grids), `T5_coverage.csv` (run counts) |
| tab:datasets | dataset table | `src/data.py`, `FRL-GDP-full/src/base/datasets.py` (d = 102/122), run logs (Adult 32,561/12,661; Crime fold-based 1,794–1,795/199–200; ACSIncome 12,800/4,000 train/test after 20,000 subsample, d = 9); Pokec sizes from Kong et al. Table 3 |
| tab:hyperparameters | training settings per dataset | `configs/default.yaml`, `configs/graph.yaml`, as-run `config.yaml` under `results/critic20/{Adult-age,Crime-racepctblack}/supipm`, `results/ACSIncome-age/supipm` (J_v = 2, α_v = 1e-3), `results/e1d-tau/...`, `results/pokec_z-GCN-AGE/supipm-cs10` (batch 1024, 100 epochs, wd 0, J_v = 10) |
| tab:penalty-grids | λ values per method/dataset | `lmda_f-*` directory names under the trees `scripts/summary_data.py` loads |
| tab:baselines | fairness term per method | written from the method papers; no numbers |
| tab:worst-case-settings | worst-case measurement settings | `EXPERIMENT_SPEC.md` §4, `tools/stress_head.py`, `tools/run_final_c1.sh`, `tools/run_cgrid.sh` (λ_h ladder {0,10,1000} for c>1), `src/evaluate.py:compute_inf_gdp` |
| tab:ranking | both disparities at three utility levels, with ranks | `T1_headline.csv` (all rows except Simulation); ranks computed per dataset/level among the methods present |
| tab:discriminator-ablation | J_v = 2 vs 20 on SynthC | `T4_critic_ablation.csv` |
| tab:coverage | runs kept / measured | `T5_coverage.csv` |
| tab:penalty-diagnostics | ρ, seed CV, min/max per method | `T6_penalty_diagnostics.csv` (the two internal "rank" S-variant rows for supIPM-FRL on Pokec dropped) |

Not used: `T2_ranking_inversion.csv` (single utility level; superseded by the T1-based table), `T3_gain_budget.csv` (read at a different utility level than figA1 and covering only Adult/ACSIncome/Pokec; the c-dependence is shown by figA1 instead).

**Table regeneration.** The committed `results_summary/appendix/T*.csv` (2026-09-11 22:16) predate the last figure rebuild (2026-09-12 12:4x) and are stale for Adult (supIPM-FRL now has 14 λ values, 58/75 runs; T1 Adult rows at levels 0.835/0.838 changed; T5 Adult runs kept 545; T6 Adult supIPM-FRL row ρ = −0.84, seed CV 0.15). `make_tables.py` was re-run on 2026-09-12 and the fresh CSV/MD files are in `results_summary/appendix_paper/`; tab:ranking, tab:coverage and tab:penalty-diagnostics use them. The originals in `results_summary/appendix/` were left untouched.

## Caveats

- ACSIncome supIPM-FRL runs are the J_v = 2 (weak discriminator) runs; the retrained J_v = 20 runs exist in `results/critic20/ACSIncome-age/supipm` but have no worst-case measurement, so the figures use the first runs. Stated in Appendix D.2 and E.1.
- No TeX toolchain on ideaserver2: the document was not compiled here. Check page count on Overleaf; the main-text Experiments section was written to about 2.1 pages (Sections 1–4 end at ≈6.2 pages).
