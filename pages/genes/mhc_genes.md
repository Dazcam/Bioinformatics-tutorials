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

### Ensembl

There are several ways to do this using ensembl.

#### REST API

The function below generates a dataframe containing all MHC regions genes and 
their associated metadata for either genome build.

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
