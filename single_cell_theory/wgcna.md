---
title: WCGNA
layout: mathjax
parent: Single cell theory
---

WGCNA is a method for identifying co-expressed genes in a high-dimensional dataset. It was built in 2008, meaning its target input is microarray and bulk RNA-seq datasets. The overall idea of finding co-expressed genes and clustering them into groups may sound straightforward, but the package contains many different steps of analysis.

The input to WGCNA is a matrix of gene expression with size n x m, in which the number of rows (n) denotes the genes (WGCNA terminology, nodes), and the number of columns denotes the samples or measurements. In the original paper, the author divides WGCNA into several major stages:

- **Functions for network construction**: A correlation matrix of size n x n is constructed from the original gene expression matrix, in which each element i,j defines the correlation coefficient of gene i and j: $${s_{ij} = \lvert \text{cor}(x_i, x_j) \rvert }$$. The $$cor()$$ function outputs values from 0 to 1, but other definitions of correlation can be used. An adjacency matrix can be created from the correlation matrix by setting a hard or soft threshold on $$s_{ij}$$: $$a_{ij} = 1 \; \text{if} \; s_{ij} > \tau \; a_{ij} = 0 \; \text{otherwise}$$.
- **Functions for module detection**: WCGNA terminology, modules are defined as clusters of densely interconnected genes. A measure of network interconnectedness is defined using topological overlap measure with the following definition:

WCGNA then uses hierarchical clustering to cluster things together and identifies gene modules. The resolution of the clustering is defined using Dynamic Tree Cut. There are several summaries for each module: 

- a module eigengene, or the first principal component of the module, that best summarizes the expression in a module
- after modules are determined, each node is assigned a module. But some nodes may lie in the border of two modules, making assigning membership difficult. A more fuzzy, continuous membership for each nodes to all modules attempts to solve this.
- different datasets produce different networks. Therefore, WCGNA provides a function to identify consensus modules across different datasets.
- **Functions for module and gene selection**: After the modules are identified, the next question is what are the most biologically interesting modules and genes? To answer this, a new metric called gene significance is defined for each gene i:

$$x_i$$ is the expression of gene i, and T is a specific trait. High GS score indicates pathways associated with that trait. The module significance for trait T is simply defined as the average of gene significance scores of all module genes. 

- **Functions for studying topological properties**: Since the output of WCGNA is a weighted adjacency matrix of the gene network, different network statistics can be utilized to understand the underlying gene network.