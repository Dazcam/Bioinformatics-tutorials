---
layout: page
title: Genome annotation
description: Genome annotation introduction
---

Two key terms are a `genome assembly` and genome `annotation`.

## Genome Assembly

A genome assembly (aka reference genome or reference build) is a compuational representation 
of a genome sequence that is used as a common standard for a particular species. As it is not 
yet possible (but may be in the future with WGS) to sequence sequence along the complete length 
of a chromosome, each chromosome assembly is made up of short stretches of sequenced DNA pasted 
together. This always creates some gaps and errors. In the simplest case, any individual genome 
can be used as a reference genome. However, the quality and sensitivity of analysis is increased 
when the reference genome is more representative of the widest group of individuals we might want 
to study. So each segment of the genome reference should feature the sequence most commonly 
observed across available individual genomes. The resulting reference genome is therefore a 
synthetic hybrid that serves as archetype but whose sequence is not actually observed wholesale 
in any particular individual genome. Genome assemblies for model organisms, such as human, 
are derived from multiple individuals, while some for some less well studies species 
reference builds are derived from a single individual. Sources: 
[here](https://www.ensembl.org/info/genome/genebuild/assembly.html) and 
[here](https://gatk.broadinstitute.org/hc/en-us/articles/360035891071).

Genome assembly data is curated by the 
[Genome Reference Consortium](https://www.ncbi.nlm.nih.gov/grc) and occasionally updates are 
publically released.

- `Major assembly updates`: are released infrequently and may shift the coordinates of certain loci
  based on new information.
- `Patches`: are released more frequently and are regional fixes for the current major assembly. 
These are intended to improve representation or add information to the assembly without disrupting
the chromosome coordinates. There are two types of patches:
    - `Fix patches`: represent sequences that will replace primary assembly sequence in the next
      major assembly release. When interpreting data, fix patches should take precedence over the
      chromosomes
    - `Novel patches`: represent alternate loci. When interpreting data, treat novel patches as
      population sequence variants.

Beyond the major assembled chromsomes (chr 1-22, X, Y, M etc) on the human genome build there
are some other identifiers you should be aware of:

- `_random`: unlocalized sequences (known to belong on a specific chr but with unknown order or orientation)b
- `chrU_`: unplaced sequences (chromosome of origin unknown)
- `_alt`: an alternate contig (patch) of the primary assembly (sometimes very similar to primary sequence, sometimes highly divergent)

Genome assemblies are represented in various genome browsers and databases including Ensembl, NCBI and UCSC Genome Browser.

## Genome Annotation


`Genome annotation` is the process of identifying the location of genes and functional 
elements within raw coding sequence of a species of interest. There are three main 
international grops that provide genome annotation / browser services:

- Ensembl
- NCBI
- UCSC

[MANE paper](https://www.nature.com/articles/s41586-022-04558-8)

This information is 
stored in, and can be extracted from, large databases fairly trivially. 

As annotation data is constantly being refined and improved, genome builds are being 
constantly updated to add new information on genomic regions that were not previously
mapped / annotated, or to correct errors from previous builds. Moreover, as
annotation data is collated independently by different international bodies, one 
needs to be aware that there can be differences between each bodies set of 
annotations. For example, Different annotation services have slightly different criteria 
for how they define:

- what a gene is
- where those genes are in the genomes
- whether or not a given gene in the same relative position for both services is in fact the same gene




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
- GenBank: the NIH genetic sequence database, an annotated collection of all publicly available DNA sequences. It is
updated every 2 months
- UniProt: resource of protein sequence and functional information
- UCSC: resource for genomic assembly and genomic annotations 

[GENCODE](https://www.gencodegenes.org)


[Accessing Ensembl annotation with biomaRt](https://rdrr.io/bioc/biomaRt/f/vignettes/accessing_ensembl.Rmd)



[MANE and ]
[Genomic Annotation Resources](https://www.bioconductor.org/packages/release/workflows/vignettes/annotation/inst/doc/Annotation_Resources.html)
[Annotating Genes, Genomes, and Variants](https://www.bioconductor.org/help/course-materials/2014/SeattleOct2014/B02.4_Annotation.html) - old
[Annotation Resources](https://www.bioconductor.org/help/course-materials/2019/CSAMA/L1.5-bioc-annotation.html)





[AnnotationDb vingette](https://www.bioconductor.org/packages/devel/bioc/vignettes/AnnotationDbi/inst/doc/IntroToAnnotationPackages.pdf)

`AnnotationDB` is a wrapper that allows you to access bioconductor annotation packages
at gene or genomic centric levels of detail. Under the hood it utilises `SQL` relational
database code to access tables in databases from various sources.

The main commands are:

- **`columns()`**: Informs you what kinds of data are retriveable via the `select` command
- **`help()`**: Describes what data is stored in each colum  
- **`keytypes()`**: Defines which key is to be used to query the database
- **`select()`**: Is used to extract data from the database based on key and column values
- **`mapIds()`**: Similar to `select()` but extracts just a single column of interest




- org.Hs.eg.db: - R object that contains mappings between Entrez Gene identifiers and GenBank accession numbers.
- TxDb.Hsapiens.UCSC.hg19.knownGene: - R object that connects a set of genomic coordinates to various transcript oriented features from UCSC builds
- EnsDb.Hsapiens.v75: provide genomic coordinates of gene models along with additional annotations (e.g. gene names, biotypes etc) from Ensembl database






BiomaRt: Allows you to run queries on various ensembl databases. It can be used for ID mapping and feature extraction.

Pros:

- Fairly intuitive interface
- 

Negative:

- Need internet access


[annotables](https://github.com/stephenturner/annotables)
[Generating and using Ensembl based annotation packages](https://www.bioconductor.org/packages/devel/bioc/vignettes/ensembldb/inst/doc/ensembldb.html)


----

[Haplotypes and patches](https://www.ensembl.org/info/genome/genebuild/haplotypes_patches.html)
[GRC terminology](https://www.ncbi.nlm.nih.gov/grc/help/definitions/)
[NCBI assembly primer](https://www.ncbi.nlm.nih.gov/assembly/basics/)
[NCBI assembly model](https://www.ncbi.nlm.nih.gov/assembly/model/#model)
[UCSC genome annotation database](http://hgdownload.soe.ucsc.edu/goldenPath/hg38/database/)
[Ensembl tools video](https://www.youtube.com/watch?v=PavUJIU42Js)


All genome assemblies in Ensembl are haploid and for most species there is only a single path through the genome.

Currently, human and mouse are the only genome assemblies where there is more than one path through the genome. They include haplotypes, which are different versions of particular regions of the genome that are found in different individuals. They can include small differences in sequence, or larger differences such as different genes or gene order compared to the primary assembly. Since they are sequences found in real individuals, they are equally valid compared to the primary assembly.

The human genome assembly (GRCh38) and the mouse genome assembly (GRCm38) are maintained by the Genome Reference Consortium (GRC). In addition to the primary assembly, the GRCh38 major assembly release of the human genome included 261 alternate loci including eight haplotypes on the MHC region of chromosome six.

Novel patches: provide alternate alleles. coloured in red in Ensembl browser and will become haplotypes in next major release.
These are equally valid compared to the primary assembly.

Fix patches: provide improved sequence for known assembly errors. These patches will be incorporated into the primary assembly in the next major assembly release. They are coloured green in the Chromosome summary page and Region in detail page. They are improvements on the primary assembly and should be used preferentially over the primary assembly.

Minor assembly releases have the following naming convention: GRCm38.p3 for the third patch release of GRCm38


In order to sequence the genome, DNA is first fragmented into small sections that
are sequenced. After sequencing, contiguous sections of DNA, `contigs`, are assembled
which are, in turn, assembled into scaffolds and then into chromosomes. Gaps are left
where no sequence is available. A primary assembly is a single version of the genome,
and this procdeure is overseen and maintained for human and mouse by the Genome
Reference Consortium.

There are some important points to be aware of when using a particular genome build:

- Primary assemblies have gaps and errors
- Certain genomic regions have know haplotypes that cannot be narrowed down to a single sequence 

Due to this, genome assmblies are constantly being revised and updated by the GRC who release
information on known alternative sequence and corrections to the primary assembly:

- Haplotypes: are included with the last major release of the primary assembly
- Genome patch: A contig sequence that is released outside of the full assembly (as a minor release)

In relation to this there are two types of [patches](https://www.ncbi.nlm.nih.gov/grc/help/patches/): 

- Novel patches: provide new alternate loci info to the assembly (coloured in red in Ensembl browser) and will become haplotypes in next major release.
These are equally valid compared to the primary assembly.

- Fix patches: provide improved sequence for known assembly errors. These patches will be incorporated into the primary assembly in the next major assembly release. They are coloured green in Ensembl. They are improvements on the primary assembly and should be used preferentially over the primary assembly.


