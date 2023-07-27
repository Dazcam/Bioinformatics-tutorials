---
layout: page
title: Genome annotation
description: SNP annotation
---

## Bioconductor
Annotating SNPs in R can be done using the BSgenome and SNPlocs packages.


The first thing you will have to do is install and load the genome sequence and SNP database
libraries into R.

```R
# Install and load packages
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install("SNPlocs.Hsapiens.dbSNP144.GRCh37")
BiocManager::install("SNPlocs.Hsapiens.dbSNP155.GRCh38")
BiocManager::install("BSgenome.Hsapiens.UCSC.hg19")
BiocManager::install("BSgenome.Hsapiens.UCSC.hg38")

library(BSgenome.Hsapiens.UCSC.hg19)
library(SNPlocs.Hsapiens.dbSNP144.GRCh37)
library(dplyr)

# Set more concise variable names for the database libraries
genome_lib <- BSgenome.Hsapiens.UCSC.hg19
snp_lib <- SNPlocs.Hsapiens.dbSNP144.GRCh37

# Set convention for chr names
seqlevelsStyle(genome) <- "NCBI" # If chr = '1'
#seqlevelsStyle(genome) <- "UCSC" # If chr = 'chr1'
```

***

A common task is to obatin rsIDs for a set of SNPs that you only have base position and 
chromosome information for:

```R
snp_df <- dplyr::tibble(chr = c("8", "8", "8", "8", "8"),
                        start = c('101592213', '106973048', '108690829', '102569817', '108580746'),
                        end = c('101592213', '106973048', '108690829', '102569817', '108580746'),
                        rsID = c('rs62513865', 'rs6994300', 'rs79643588', 'rs138449472', 'rs17396518'))

# Create a genomic positions class to store a set of genomic positions to query
snp_pos <- GenomicRanges::GPos(seqnames = snp_df$chr, pos = snp_df$start)

# Run the query using base positions
my_snps <- BSgenome::snpsByOverlaps(snp_lib, snp_pos, genome = genome_lib)
```

Alternatively, you can pull out the SNP positions for a given set of rsIDs:

```R
# Run the query using rsIDs
my_snps <- BSgenome::snpsById(snp_lib, snp_df$rsID, genome = genome_lib)
```

***

## SQL

If you have mysql installed on your system you can query the UCSC database fairly trivially.


```sql
# Print to screen
# Add -B flag for tsv
mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A -D hg19 -e '
SELECT chrom, chromStart, chromEnd, name 
FROM snp147 
WHERE name 
IN ("rs371194064","rs779258992","rs26","rs25")'
```

To check the columns avaiable in the database:

```sql
mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A -D hg19 -e 'show columns from snp147'
```

***
