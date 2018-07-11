---
layout: page
title: Required packages
description: Required packages for high throughput sequencing walkthrough
---

* SRA files
 * [SRA toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std))
  * prefetch 
  * fastq_dump 
  * [parallel-fastq-dump](https://github.com/rvalieris/parallel-fastq-dump)

* Quality control
  * fastqc
  * multiqc

* Alignment
  * [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) 

* `.sam` file to `.bam` file, and sorting and indexing `.bam` files
  * [Samtools](http://samtools.sourceforge.net)

* Peak calling
  * [Macs2](https://github.com/taoliu/MACS)

* Remove blacklisted regions, merge peaks, extend peaks
  * [Bedtools](http://bedtools.readthedocs.io/en/latest/)

* Remove mitochodrial reads
  * [Bedops](http://bedops.readthedocs.io/en/latest/)

* Motif analysis
  * [Homer](http://homer.ucsd.edu/homer/)

Move onto [Get SRA files](pages/get_sra_info.html).
