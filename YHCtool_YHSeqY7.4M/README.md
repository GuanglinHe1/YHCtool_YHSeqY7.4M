# YHCtool_patched_modern

One-click population genetics analysis for Y-chromosome haplogroup data: genetic distances, diversity indices, PCA/CA/MDS, Fisher enrichment, Sankey/alluvial plots, and bootstrap significance testing — across multiple haplogroup-resolution levels (HH = truncation length of the haplogroup name).

Core analysis logic is unchanged from the original script. Only the I/O layer was adapted for this upload: absolute paths removed, input switched from xlsx to tsv, example data de-identified.

## Structure
```
YHCtool_upload/
├── script/YHCtool_patched_modern.R
├── data/Population.tsv      # example input, de-identified, ~530 samples
├── conf/group_meta.csv      # population metadata (region, lat/lon)
└── output/                  # results, one folder per HH level
```

## Input format
`data/Population.tsv` — 3 columns, name-matched case-insensitively, order-independent:

| Column | Aliases |
|---|---|
| sample ID | `sample_id` / `sample` / `id` |
| haplogroup | `haplogroup_full` / `haplogroup` / `hap` |
| population | `population` / `pop` / `group` |

`conf/group_meta.csv` (optional): `population` + `group`/`region` (coloring, PERMANOVA) or `Lat`/`Lon` (Mantel test).

## Run
```bash
cd script
Rscript YHCtool_patched_modern.R
```
Default `HH_range` is `1:10`, each level with full bootstrap (10,000 draws x 1,000 reps) — slow. To limit levels without editing the script:
```bash
Rscript -e 'HH_range <- 1:3; source("YHCtool_patched_modern.R")'
```
Other overridable params (`input_table`, `meta_csv`, `root_out`, `bootstrap_n_reps`, ...) are set near the top of the script.

## Output
Each `output/out_HH{NN}/`:
- `tables/`: frequency & count matrices, diversity indices, Fisher enrichment, TVD/JSD/Nei's D/DA distances, PCA/CA coordinates
- `figs/`: heatmaps, dendrograms (+Newick), NJ trees, MDS/PCA/CA, stacked bars, Sankey, alluvial plots
- `bootstrap/`: frequency bootstrap, pairwise Wilcoxon (BH-corrected), diversity bootstrap, length-faceted frequency plots

Verified end-to-end with `HH_range <- 1:3` on the example data — completed successfully.
