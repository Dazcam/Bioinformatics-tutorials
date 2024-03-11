---
layout: page
title: Working with Seurat 5
description: Working with Seurat 5
--- 

I've been working with Seurat 5 recently which has introduced new functionality 
to deal with large datasets. It incorporates functionaility from the `BPCells` package
to store a reduced dataset in working memory whilst keeping the bulk of the data
off line. It also introduces the concept of the `sketch` object which is subset of
cells (~5000) taken from the entire data set, that preserves the overall pattern of
the data, preserving small cell populations. This allows you to run processes on the 
data subset, which signifcantly speeds up interactive analyses.

***

#### Changes to the seurat object data structure

Seurat 5 has introduced an additional strata for it's expression data called layers. 
This can be split by individual sample to run qc steps such as normalisation on the
seprately. As you run all the different steps of an analysis in Seurat, and try to 
identify the optimum parameters to opt for with your data, the number of slots that you 
fill in the Seurat object gradually increases, and it can get quite confusing where
everything is stored and what version of the data is being processed at each step. This
section will document this to make it easier to keep track of everything.

#### Basic analysis

<details>

<summary>Commands for the basic seurat analysis</summary>

```r
seurat_small <- Seurat::FindVariableFeatures(seurat_small, verbose = F) %>%
    Seurat::ScaleData(verbose = F) %>%
    Seurat::RunPCA(verbose = F) %>%
    Seurat::FindNeighbors(dims = 1:dims) %>%
    Seurat::FindClusters() %>%
    Seurat::RunUMAP(dims = 1:dims)

seurat_small

#> An object of class Seurat 
#> 230 features across 80 samples within 1 assay 
#> Active assay: RNA (230 features, 230 variable features)
#>  3 layers present: counts, data, scale.data
#>  3 dimensional reductions calculated: pca, tsne, umap
```

</details>

The layers for this data are:

- counts: raw gene expression matrix
- data: normalised counts slot
- scale.data: scaled data slot

***

#### Adding SCT normalisation

Seurat supports several normalisation procedures. SCT is a normalisation feature that stablises
the varianace across the data, which can be XXX in scRNAseq data. This can take a while to run,
it is [recommeneded](https://github.com/satijalab/seurat/issues/3354) to this using
`conserve.memory=TRUE` and even reducing `ncells=3000` if necessary to make it run more quickley.

<details>

<summary>SCT normalistion steps</summary>

```r
seurat_small <- Seurat::FindVariableFeatures(seurat_small, verbose = F) %>%
    Seurat::RunPCA(verbose = F) %>%
    Seurat::FindNeighbors(dims = 1:dims) %>%
    Seurat::FindClusters() %>%
    Seurat::RunUMAP(dims = 1:dims)

seurat_small

#> An object of class Seurat 
#> 230 features across 80 samples within 1 assay 
#> Active assay: RNA (230 features, 230 variable features)
#>  3 layers present: counts, data, scale.data
#>  3 dimensional reductions calculated: pca, tsne, umap
```

The layers for this data are:

- counts: raw gene expression matrix
- data: normalised counts slot
- scale.data: scaled data slot

</details>
