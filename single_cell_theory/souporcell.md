---
title: Souporcell
layout: mathjax
parent: Single cell theory
---

Souporcell is used to remove doublets in scRNA-seq experiments. Doublets are droplets that contain either more than one cell type from the same individual or the same cell type but from more than one individual. Without correcting for doublets, downstream analyses may be errorneous (identifying false novel cell types or false trajectory analyses).

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/7f8529f8-1780-4b61-a7b4-8593e37d92ec/6613593f-c238-4dbc-8641-14e26ee3075e/Untitled.png)

Souporcell consists of the following main steps:

- The reads are realigned on the reference genome using minimap2, instead of STAR used in CellRanger. The author claimed this alignment method improved identification of variants.
- SNPs were called using freebayes.
- For each cell, the number of SNP-specific alleles was counted using vartrix.

At this point, we have a count matrix in which each row represents a cell, each column represents a variant, and the element $$e_(ij)$$ determines the number of reads in that cell (or droplet, because we’re not sure whether that row only comes from one cell or a doublet), that belong to that variant. This resembles the transpose of the familiar gene count matrix, in which the columns are the cells and the rows are the genes.

The next steps involve clustering the cells based on their variant read features. The authors devised a novel algorithm for clustering method (sparse mixture model clustering). The reasoning is that, non-doublet droplets are concentrated in well-defined clusters, whereas droplets that linger in the middle of clusters are highly doublets (Figure 1d,e). The exact mathematical definitions can be a little technical, but it boils down to first calculating a prior probability of each cell belonging to each cluster, then using a variant of expectation maximization, calculating a posterior probability of each cell belonging to a pair of clusters, therefore a doublet.