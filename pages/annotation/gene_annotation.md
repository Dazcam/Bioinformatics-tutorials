
There are many tools we can use annotate genes. 

## Ensembl

### Biomart

Create a basic `biomaRt` lookup table for all genes in the Ensembl database. This can then
be cross-referenced with a list of gene IDs to convert them to a different encoding (e.g. 
from HGNC to Entrez IDs), or to add gene coordinates to them. 

```R
require(biomaRt)
require(tidyverse)

## Create lookup table 
mart <- useMart('ENSEMBL_MART_ENSEMBL')
mart <- useDataset('hsapiens_gene_ensembl', mart)

annot_lookup <- getBM(
  mart = mart,
  attributes = c(
    'chromosome_name', 
    'start_position', 
    'end_position',       
    'gene_biotype', 
    'hgnc_symbol',
    'entrezgene_id', 
    'ensembl_gene_id'),
  uniqueRows = TRUE)

as_tibble(annot_lookup)

# A tibble: 75,739 × 7
#   hgnc_symbol ensembl_gene_id gene_biotype   entrezgene_id chromosome_name start_position end_position
#   <chr>       <chr>           <chr>                  <int> <chr>                    <int>        <int>
# 1 MT-TF       ENSG00000210049 Mt_tRNA                   NA MT                         577          647
# 2 MT-RNR1     ENSG00000211459 Mt_rRNA                   NA MT                         648         1601
# 3 MT-TV       ENSG00000210077 Mt_tRNA                   NA MT                        1602         1670
# 4 MT-RNR2     ENSG00000210082 Mt_rRNA                   NA MT                        1671         3229
# 5 MT-TL1      ENSG00000209082 Mt_tRNA                   NA MT                        3230         3304
# 6 MT-ND1      ENSG00000198888 protein_coding          4535 MT                        3307         4262
# 7 MT-TI       ENSG00000210100 Mt_tRNA                   NA MT                        4263         4331
# 8 MT-TQ       ENSG00000210107 Mt_tRNA                   NA MT                        4329         4400
# 9 MT-TM       ENSG00000210112 Mt_tRNA                   NA MT                        4402         4469
#10 MT-ND2      ENSG00000198763 protein_coding          4536 MT                        4470         5511
```

So we pull down 75,737 'genes' in total. So to demonstrate how to get annotate a gene list of entrez IDs we can 
take a random sample of rows from the lookup table.

```R
set.seed(123) 
entrez_genes <- annot_lookup %>%
  slice_sample(n = 10)

#    hgnc_symbol ensembl_gene_id                     gene_biotype entrezgene_id     chromosome_name start_position end_position
# 1   HNRNPA1P26 ENSG00000239345             processed_pseudogene            NA                   X      100855288    100856379
# 2      SEPTIN4 ENSG00000108387                   protein_coding          5414                  17       58520250     58544368
# 3      FABP3P2 ENSG00000233259             processed_pseudogene            NA                  13       42369259     42369657
# 4       CCHCR1 ENSG00000206457                   protein_coding         54535 HSCHR6_MHC_QBL_CTG1        2400454      2416245
# 5     NDUFA3P1 ENSG00000267590             processed_pseudogene            NA                  19       44297104     44297355
# 6       ZNF846 ENSG00000196605                   protein_coding        162993                  19        9751993      9793180
# 7    RN7SL719P ENSG00000274632                         misc_RNA            NA                  15       28703554     28703790
# 8              ENSG00000289518                           lncRNA            NA                  11        9613503      9614048
# 9      UPF3AP3 ENSG00000234709             processed_pseudogene            NA                   9       99998301     99999069
# 10     FAM74A4 ENSG00000274583 transcribed_processed_pseudogene        401508                   9       61205258     61212373
```

Notice that:

- Not all genes in this list a protein coding genes, we also have RNA and pseudogenes
- Some of the entries are derived from chromosomal patches and scaffolds
- Although we have ensembl IDs for each entry we do not have HGNC and / or entrez IDs for some of the 'genes'

So we'll first need to get rid of the `NA`s in the `entrezgene_id` column to generate a our sample. Well also
strip out all the other columns so we only have a gene list of entrez IDs.

```R
set.seed(123) 
entrez_genes <- annot_lookup %>%
  drop_na(entrezgene_id) %>%
  slice_sample(n = 50) %>%
  select(entrezgene_id) 
  
entrez_genes %>%
  slice_head(n = 6)

#   entrezgene_id
# 1     124908513
# 2     100616220
# 3          1293
# 4     124908381
# 5        343563
# 6     124905759
```

We then just need to use `dplyr::left_join` to annotate our gene list with the information in the lookup
table. Let's convert the entrez IDs to HGNC IDs and pull out `chr`, `start` and `end` for each gene.

```R
#  Joining with `by = join_by(entrezgene_id)`
#  A tibble: 182 × 7
#    entrezgene_id hgnc_symbol ensembl_gene_id gene_biotype chromosome_name start_position end_position
#            <int> <chr>       <chr>           <chr>        <chr>                    <int>        <int>
#  1     124908513 "RNA5-8SN5" ENSG00000274917 rRNA         GL000220.1              112025       112177
#  2     124908513 ""          ENSG00000273730 rRNA         GL000220.1              155997       156149
#  3     124908513 "RNA5-8SN4" ENSG00000276700 rRNA         KI270733.1              128877       129029
#  4     124908513 ""          ENSG00000275757 rRNA         KI270733.1              173956       174108
#  5     124908513 ""          ENSG00000277739 rRNA         21                     8256781      8256933
#  6     124908513 "RNA5-8SN3" ENSG00000275215 rRNA         21                     8395607      8395759
#  7     124908513 "RNA5-8SN1" ENSG00000278189 rRNA         21                     8439823      8439975
#  8     124908513 "RNA5-8SN2" ENSG00000278233 rRNA         21                     8212572      8212724
#  9     124908513 "RNA5-8SN3" ENSG00000288387 rRNA         HG2513_PATCH            442732       442884
# 10     124908513 "RNA5-8SN1" ENSG00000288326 rRNA         HG2513_PATCH            486948       487100
```

Now as we had an initial gene list of 50 genes, we would expect 50 entries to be returned. However, as
more than one 'gene' entry can map to the same entrez id, in the case shown above several ribosomal RNA
'genes' map to `124908513`, we have a 1:many correspondence. We also have no HGNC ID for some of these 
entries.

Before we go any further let's drill into the lookup table a bit.

Lets check for missing values in the 3 gene ID cols:

```R
annot_lookup %>%
  summarise(across(everything(), ~ sum(is.na(.))))

#   hgnc_symbol ensembl_gene_id gene_biotype entrezgene_id chromosome_name start_position end_position
# 1           0               0            0         39715               0              0            0

annot_lookup %>%
  filter(hgnc_symbol == "") %>%
  count()

#       n
# 1 24981

annot_lookup %>%
  filter(ensembl_gene_id == "") %>%
  count()

#   n
# 1 0
```

So we have ensembl IDs for all entries, 39,715 missing entrez IDs and 24,981 missing HGNC IDs. What 
are the numbers if we focus on protein coding genes:

```R
annot_lookup %>%
  filter(gene_biotype == "protein_coding") %>%
  summarise(across(everything(), ~ sum(is.na(.))))

#   hgnc_symbol ensembl_gene_id gene_biotype entrezgene_id chromosome_name start_position end_position
# 1           0               0            0           810               0              0            0

annot_lookup %>%
  filter(hgnc_symbol == "") %>%
  filter(gene_biotype == "protein_coding") %>%
  count()

#     n
#  1147

annot_lookup %>%
  filter(gene_biotype == "protein_coding") %>%
  filter(hgnc_symbol == "" & is.na(entrezgene_id)) %>%
  count()

#     n
# 1 693

```

So we have:

- 810 protein coding genes with no entrez ID
- 1147 protein coding genes with no HGNC ID
- 693 protein coding genes with no entrez and no HGNC ID.
- 



