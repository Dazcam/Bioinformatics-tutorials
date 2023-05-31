---
layout: page
title: Generate gene list that overlap a specific set of genome coordinates
description: Get a gene list for MHC region
---

This post describes various methods to generate a gene list from a set of specified
coordinates. We will focus on the MHC region on chromosome 6 on genome build hg19 
and hg38.

***

### MHC region

The coordinates for the MHC region can be viewed on the Genome Reference consortium 
website for [hg19](https://www.ncbi.nlm.nih.gov/grc/human/regions/MHC?asm=GRCh37) and 
[hg38](https://www.ncbi.nlm.nih.gov/grc/human/regions/MHC). 

These are:

- **hg19** - chr6:28,477,797-33,448,354
- **hg38** - chr6:28,510,120-33,480,577

There is also an extended MHC region (xMHC) described in this 
[Nature review](https://www.nature.com/articles/nrg1489) paper which 

***

### REST API - Ensembl

The function below generates a dataframe containing all MHC regions genes and 
their associated metadata for either genome build using Ensembl's REST API.

```R
library(httr)
library(jsonlite)
library(xml2)
library(tidyverse)
  
get_mhc_region <- function(build = 'hg38') {
  
  # Get MHC genes using Ensembl REST API
  if (build == 'hg19') {
    
    server <- "https://grch37.rest.ensembl.org"
    coord <- "chr6:28,477,797-33,448,354"
    
  } else {
    
    server <- "https://rest.ensembl.org"
    coord <- "chr6:28,510,120-33,480,577"
    
  }
  
  ext <- paste0("/overlap/region/human/", coord, "?feature=gene")
  r <- GET(paste(server, ext, sep = ""), content_type("text/csv"))
  stop_for_status(r)
  
  gene_list_df <- print(content(r)) %>% 
    map(unlist) %>% 
    map(t) %>% 
    map(as_tibble) %>% 
    bind_rows() 
  
  return(gene_list_df)
  
}

mhc_hg38_df <- get_mhc_region()
mhc_hg19_df <- get_mhc_region(build = 'hg19')
```

Check the differences between hg19 and hg38. There are 43 fewer gene entries for hg19
however only a subset of these are protein coding genes with 183 protein coding genes 
in hg38 compared to 181 in hg19.

```R
dim(mhc_hg38_df)
[1] 425  15
dim(mhc_hg19_df)
[1] 382  15

hg38_gene_types <- mhc_hg38_df %>% 
  group_by(biotype) %>% 
  summarise(hg38_cnt = n())

mhc_hg19_df %>% 
  group_by(biotype) %>% 
  summarise(hg19_cnt = n()) %>%
  full_join(hg38_gene_types)
  
Joining with `by = join_by(biotype)`
# A tibble: 22 × 3
   biotype                  hg19_cnt hg38_cnt
   <chr>                       <int>    <int>
 1 3prime_overlapping_ncRNA        1       NA
 2 antisense                      26       NA
 3 lincRNA                        18       NA
 4 miRNA                          15       13
 5 misc_RNA                       12       14
 6 polymorphic_pseudogene          1       NA
 7 processed_transcript            5       NA
 8 protein_coding                181      183
 9 pseudogene                    106       NA
10 rRNA                            1       NA
```

***

### BiomaRt - Ensembl

BiomaRt does very much the same thing as the REST API.

```R
#' Get vector of hg38 genes overlapping MHC region using biomaRt
#'
#' @return A vector of unique genes that fall within MHC region of chr6.
#' @export
#'
#' @param start Start position of MHC region.
#' @param end End position of MHC region.
#' @param server Ensembl server to use to call genes.
#'
#' @examples get_mhc_genes(build = 'hg38')
get_mhc_genes <- function(build = 'hg38') {
  
  if (build == 'hg19') {
    
    server <- "https://grch37.ensembl.org"
    start <- "28477797"
    end <- "33448354"
    
    
  } else {
    
    server <- "https://www.ensembl.org"
    start = "28510120"
    end = "33480577"
    
  }

  cat('\n\nConnecting to Ensembl server ...')
  mart <- useEnsembl("ensembl", "hsapiens_gene_ensembl", host = server)

  cat('\nCalling MHC region genes ...\n')
  mhc_genes <- biomaRt::getBM(attributes = c("hgnc_symbol", "chromosome_name", "start_position", "end_position", 
                                             "gene_biotype", "source", "version", "ensembl_gene_id"),
                     filters = c("chromosome_name", "start", "end"),
                     values = list(chromosome = "6", start = start, end = end),
                     mart = mart)
  #mhc_genes_uniq <- stringi::stri_remove_empty(unique(mhc_genes$hgnc_symbol), na_empty = FALSE)
  cat('\n\nMHC genes detected:', length(mhc_genes_uniq), '\n\n')

  return(mhc_genes)

}

mhc_hg19_mart_df <- get_mhc_genes(build = 'hg19')
mhc_hg38_mart_df <- get_mhc_genes(build = 'hg38')
```

It looks as if we are pulling out identical information using both methods.

```R
dim(mhc_hg19_mart_df)
[1] 382   8
dim(mhc_hg38_mart_df)
[1] 425   8
hg38_gene_types <- mhc_hg38_mart_df %>% 
  group_by(biotype) %>% 
  summarise(hg38_cnt = n())

hg38_gene_mart_types <- mhc_hg38_mart_df %>% 
  group_by(gene_biotype) %>% 
  summarise(hg38_cnt = n())

mhc_hg19_mart_df %>% 
  group_by(gene_biotype) %>% 
  summarise(hg19_cnt = n()) %>%
  full_join(hg38_gene_mart_types)
  
Joining with `by = join_by(gene_biotype)`
# A tibble: 22 × 3
   gene_biotype             hg19_cnt hg38_cnt
   <chr>                       <int>    <int>
 1 3prime_overlapping_ncRNA        1       NA
 2 antisense                      26       NA
 3 lincRNA                        18       NA
 4 miRNA                          15       13
 5 misc_RNA                       12       14
 6 polymorphic_pseudogene          1       NA
 7 processed_transcript            5       NA
 8 protein_coding                181      183
 9 pseudogene                    106       NA
10 rRNA                            1       NA
```
***

### Benchmarking

The BiomaRt method is on average ~3x slower than the REST API method.

```R
microbenchmark::microbenchmark(
  
  mhc_hg38_mart_df <- get_mhc_genes(),
  mhc_hg38_df <- get_mhc_region(),
  times = 5
  
)                                expr      min       lq     mean   median       uq      max neval
 mhc_hg38_mart_df <- get_mhc_genes() 1.575486 2.457424 6.710770 2.755378 9.689827 17.07574     5
     mhc_hg38_df <- get_mhc_region() 1.595706 1.832278 2.138574 1.938852 1.991592  3.33444     5
```

***
