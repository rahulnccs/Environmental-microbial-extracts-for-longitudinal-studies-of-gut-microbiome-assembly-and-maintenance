# Environmental microbial extracts for longitudinal studies of gut microbiome assembly and maintenance

Supporting data and analysis code for the manuscript titled as Environmental microbial extracts for longitudinal studies of gut microbiome assembly
and maintenance by Rahul Bodkhe, Rebecca Choi, and Michael Shapira.

Raw sequencing reads are deposited in the NCBI SRA under accession **PRJNA1462967**.
This repository provides everything needed to go from the processed ASV tables to the
figures and statistics reported in the manuscript: phyloseq objects (ASV counts, taxonomy,
phylogenetic trees, sample metadata), the full analysis script, and the supplementary tables.

**[View the full analysis notebook (rendered HTML)](https://htmlpreview.github.io/?https://github.com/rahulnccs/Environmental-microbial-extracts-for-longitudinal-studies-of-gut-microbiome-assembly-and-maintenance/blob/main/analysis/CME_MethodPaper_Analysis.html)**
— code, figures, and statistics inline, rendered directly in the browser.

## Contents

```
analysis/
    CME_MethodPaper_Analysis.Rmd    R Markdown source — every figure and statistic reported in
                                     the manuscript, run directly from the phyloseq objects below.
    CME_MethodPaper_Analysis.html   Knitted output of the above (code + figures + results inline).

phyloseq_objects/
    Figure01_CEP_Age_pseq.rds       Figure 1 — CME stock / seeded-plate stability dataset.
    Figure_02_CME_Aging_pseq.rds    Figure 2 — CME-1 aging-experiment dataset.
    Figure_03_BCME_Aging_pseq.rds   Figure 2 — CME-2 aging-experiment dataset.
    CEPBCEPAging.rds                Merged CME-1 + CME-2 dataset used for the combined analyses.

supplementary_tables/
    Supplementary_Tables_1-2.xlsx        One workbook, two sheets:
                                            - Table1_Reads_Per_Sample: DADA2 read-tracking
                                              (input -> filtered -> denoised -> merged ->
                                              non-chimeric) for every retained sample, with
                                              full sample metadata.
                                            - Table2_ASV_Taxonomy_Counts: one merged ASV count +
                                              taxonomy table spanning all three datasets, with
                                              CME/Day/SampleType/Replicate as header rows above
                                              each sample's column.
    Supplementary_Tree_Figure1.nwk       Phylogenetic tree, Figure 1 dataset (Newick).
    Supplementary_Tree_Figure2_CME1.nwk  Phylogenetic tree, Figure 2 CME-1 dataset (Newick).
    Supplementary_Tree_Figure2_CME2.nwk  Phylogenetic tree, Figure 2 CME-2 dataset (Newick).
```

## About the phyloseq objects

Each `.rds` file is a single self-contained [phyloseq](https://joey711.github.io/phyloseq/)
object (R package `phyloseq`) that bundles, with ASV identity preserved across all four:

- **ASV count table** (`otu_table`)
- **Representative sequences** — ASVs are identified by their full amplicon sequence
  (the DADA2 convention used throughout), so no separate FASTA file is needed; the
  sequence *is* the ASV identifier in every table.
- **Taxonomy** (`tax_table`)
- **Phylogenetic tree** (`phy_tree`), pruned and rooted to match the retained ASVs
- **Sample metadata** (`sample_data`), with sample IDs matching the count table's sample rows.
  Metadata is limited to the columns used in the analysis: `SampleID`, `metadata`, `Category`,
  `Day`, `Experiment`, `Replicate`, `SampleType`, `CME` (the last identifies which figure/dataset
  a sample belongs to — "CME for Figure 1", "CME-1", or "CME-2"). Only the samples that are
  actually analyzed in the manuscript are present in each object — no additional sample
  filtering happens anywhere in `analysis/CME_MethodPaper_Analysis.Rmd`.

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
and statistical result reported in the manuscript. It runs entirely from the four phyloseq
objects in `phyloseq_objects/` above — clone the repo, open the script from inside `analysis/`,
and knit. Required R packages and session details are listed at the end of the knitted HTML
(`analysis/CME_MethodPaper_Analysis.html`, "Session Information" section).

## Citing

If you use this data or code, please cite the manuscript (citation to be added upon publication).
