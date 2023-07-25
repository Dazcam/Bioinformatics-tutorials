g:profiler
genekitr: https://github.com/GangLiLab/genekitr
vingette: https://www.genekitr.fun/index.html

What is the input format?

- Bed file is a binary file containing the genotype information. 
- Bim file contains the SNP names and map positions. Chromosome number, SNP names (i.e., rs ID), and physical positions based on hg19 should be specified. Note that for SNPs with rsID, the rsID should be specified as their SNP names. For SNPs with no rsID, their SNP names should be specified as “chrom:position”. For example, 1:10000 for a SNP at 10000 bp on chromosome 1. 
- Fam file contains the family structure, where the affection status for each individual in the 6th column should be specified. 


MAGMA 


NCBI / RefSeq - entrez
HGNC - gene symbol
Ensembl - ensembl ID


[RefSeq FAQ](https://www.ncbi.nlm.nih.gov/books/NBK50679/#RefSeqFAQ.what_is_a_reference_sequence_r)
[HGNC FAQ](https://www.genenames.org/help/faq/)
[Ensembl](https://www.ensembl.org/info/index.html)
[MANE](https://www.ncbi.nlm.nih.gov/refseq/MANE/)

[GENCODE](https://www.gencodegenes.org)


[Accessing Ensembl annotation with biomaRt](https://rdrr.io/bioc/biomaRt/f/vignettes/accessing_ensembl.Rmd)



[MANE and ]
[Genomic Annotation Resources](https://www.bioconductor.org/packages/release/workflows/vignettes/annotation/inst/doc/Annotation_Resources.html)
[Annotating Genes, Genomes, and Variants](https://www.bioconductor.org/help/course-materials/2014/SeattleOct2014/B02.4_Annotation.html) - old
[Annotation Resources](https://www.bioconductor.org/help/course-materials/2019/CSAMA/L1.5-bioc-annotation.html)


Different annotation services have slightly different criteria for how they define:

- what a gene is
- where those genes are in the genomes
- whether or not a given gene in the same relative position for both services is in fact the same gene


[AnnotationDb vingette](https://www.bioconductor.org/packages/devel/bioc/vignettes/AnnotationDbi/inst/doc/IntroToAnnotationPackages.pdf)

`AnnotationDB` is a wrapper that allows you to access bioconductor annotation packages
at gene or genomic centric levels of detail. Under the hood it utilises `SQL` relational
database code to access tables in databases from various sources.

The main commands are:

- **`columns()`**: Informs you what kinds of data are retriveable via the `select` command
- **`help()`**: Describes what data is stored in each colum  
- **`keytypes()`**: Defines which key is to be used to query the database
- **`select()`**: Is used to extract data from the database based on key and column values
- **``mapIds()**: Similar to `select()` but extracts just a single column of interest




- org.Hs.eg.db: - R object that contains mappings between Entrez Gene identifiers and GenBank accession numbers.
- TxDb.Hsapiens.UCSC.hg19.knownGene: - R object that connects a set of genomic coordinates to various transcript oriented features from UCSC builds
- EnsDb.Hsapiens.v75: provide genomic coordinates of gene models along with additional annotations (e.g. gene names, biotypes etc) from Ensembl database



- GenBank: the NIH genetic sequence database, an annotated collection of all publicly available DNA sequences. It is
updated every 2 months
- UniProt: resource of protein sequence and functional information
- UCSC: resource for genomic assembly and genomic annotations 


BiomaRt: Allows you to run queries on various ensembl databases. It can be used for ID mapping and feature extraction.

Pros:

- Fairly intuitive interface
- 

Negative:

- Need internet access


[annotables](https://github.com/stephenturner/annotables)
[Generating and using Ensembl based annotation packages](https://www.bioconductor.org/packages/devel/bioc/vignettes/ensembldb/inst/doc/ensembldb.html)


TODO:

Get container working
Update snakemake tutorial
Make tweaks to paper
Update gene annotation tutorial
Read papers
