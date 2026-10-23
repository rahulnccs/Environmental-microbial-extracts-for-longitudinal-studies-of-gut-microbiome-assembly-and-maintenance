# Environmental microbial extracts for longitudinal studies of gut microbiome assembly and maintenance

Supporting data and analysis code for the manuscript (ISME Communications, manuscript ID
ISMECOMMUN-D-26-00396) by Rahul Bodkhe, Rebecca Choi, and Michael Shapira.

Raw sequencing reads are deposited in the NCBI SRA under accession **PRJNA1462967**.
This repository provides everything needed to go from those reads to the figures and
statistics reported in the manuscript: the processed ASV tables, taxonomy, phylogenetic
trees, sample metadata, and the full analysis script.

## Contents

```
analysis/
    CME_MethodPaper_Analysis.Rmd    R Markdown source — complete analysis for Figures 1 and 2,
                                     including all statistics reported in the manuscript.
    CME_MethodPaper_Analysis.html   Knitted output of the above (code + figures + results inline).

phyloseq_objects/
    Figure01_CEP_Age_pseq.rds       Figure 1 — CME stock / seeded-plate stability dataset.
    Figure_02_CME_Aging_pseq.rds    Figure 2 — CME1 aging-experiment dataset.
    Figure_03_BCME_Aging_pseq.rds   Figure 2 — CME2 aging-experiment dataset.
    CEPBCEPAging.rds                Merged CME1 + CME2 dataset used for the combined analyses.

supplementary_tables/
    Supplementary_OTU_Table_*.csv        ASV count tables (human-readable export of the
                                          otu_table in the corresponding phyloseq object).
    Supplementary_Taxonomy_Table_*.csv   Taxonomy assignments (export of the tax_table).
```

## About the phyloseq objects

Each `.rds` file is a single self-contained [phyloseq](https://joey711.github.io/phyloseq/)
object (R package `phyloseq`) that bundles, with ASV identity preserved across all four:

- **ASV count table** (`otu_table`)
- **Representative sequences** — ASVs are identified by their full amplicon sequence
  (the DADA2 convention used throughout), so no separate FASTA file is needed; the
  sequence *is* the ASV identifier in every table below.
- **Taxonomy** (`tax_table`)
- **Phylogenetic tree** (`phy_tree`), pruned and rooted to match the retained ASVs
- **Sample metadata** (`sample_data`), with sample IDs matching the count table's sample rows

Load one in R with:

```r
library(phyloseq)
pseq <- readRDS("phyloseq_objects/Figure01_CEP_Age_pseq.rds")
pseq
otu_table(pseq)[1:5, 1:5]
tax_table(pseq)[1:5, ]
sample_data(pseq)
phy_tree(pseq)
```

A tree in Newick format can be exported from any object with:

```r
ape::write.tree(phy_tree(pseq), "tree.nwk")
```

## Reproducing the analysis

`analysis/CME_MethodPaper_Analysis.Rmd` is the complete, reproducible source for every figure
and statistical result reported in the manuscript. It reads the same upstream DADA2 outputs
used to build the phyloseq objects above and regenerates both the figures and this repository's
supplementary tables. Required R packages and session details are listed at the end of the
knitted HTML (`analysis/CME_MethodPaper_Analysis.html`, "Session Information" section).

## Citing

If you use this data or code, please cite the manuscript (citation to be added upon publication).
