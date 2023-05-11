
---
layout: page
title: MAGMA gene set analysis
description: MAGMA gene set analysis
--- 

### MAGMA gene set analysis - essentially a 3 step process. 

+ [MAGMA Docs](https://ctg.cncr.nl/software/magma)
+ [Paper](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004219)

**1. Annotation - map SNPs (from GWAS sumstats file) to genes**

```bash
magma --annotate --window=35,10 --snp-loc [SNPLOC_FILE] --gene-loc [GENELOC_FILE] --out [OUTPUT_PREFIX] 
```

+ `SNPLOC_FILE` = A file containing SNPs/SNP locations used in the analysis. You need your GWAS sumstats file here. This needs to 
have the first 3 columns in the following format (including headers as shown and just chromosome numbers, no chr prefixes in the `CHR` column):

![fileList]({{ site.baseurl }}/assets/fileList.png)

+ `GENE_LOC_FILE` = A file containing gene locations used in the analysis. Use the `.bim` file provided in the reference data for this
(make sure it matches the reference build used for the GWAS) i.e. `g1000_eur.bim`

+ `--window` = The annotation window around the gene you want to set. As of May 2022 seems that 35kb upstream and 10kb downstream is a common choice (shown).

NOTE: The only columns you need in your GWAS file for this entire analysis are SNP, CHR, BP, P and N (if not using the `ncol` flag in step 2 below), 
so you can ignore the others. 

***


**2. Calculate gene level trait association statistics**

```bash
magma --bfile [DATA] --gene-annot [ANNOT].genes.annot --pval [PVAL_FILE] N=[N]
```

+ `DATA` = You need the prefix for the plink reference files here i.e. `g1000_eur`

+ `ANNOT` = SNP to gene mappings. Output file from 1.

+ `PVAL_FILE` = SNP level p-values. You need your GWAS sumstats file again. P value column needs to be labelled P. 

+ `N` = This is the sample size of your GWAS. You can manually enter the sample size here or replace the N flag with `ncol=[N_COL]`
where `N_COL` is the column in the GWAS file with sample size per SNP. 
 
***

**3. Run the gene set analysis**

```bash 
magma --gene-results [GENE_PREFIX].genes.raw --set-annot [SET_FILE] --out [OUTPUT_PREFIX]
```

+ `GENE_PREFIX` = Gene level trait statistics. Output from step 2.
+ `SET_FILE` = Your gene set file. By default MAGMA expects row-based input,where each row in the input file corresponds to a gene set. 
The first value on each row is the name of the gene set, followed by the IDs of genes (Entrez IDs required) in that gene set (whitespace separated). 
Any values that do not match IDs of genes in the .genes.raw file will be ignored.

![fileList]({{ site.baseurl }}/assets/fileList.png)

If you need to run a conditional analysis you need to add the model and condition flags to command 3. For example to run a conditional analysis 
all gene sets in the gene set file above conditioning on `FC_ExN_2`, you would add the following: `--model condition=FC_ExN_2`. 
