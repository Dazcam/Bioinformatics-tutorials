---
layout: page
title: SRA to Peak file walkthough
description: Basic high thoughput sequencing walkthrough
---

This tutorial is intended to show you how to download publically available ChIP-sea and ATAC-seq SRA files from a public repository and process them to produce BAM and peak files for downstream analyses. 

Whilst there are more efficent tools available to automate the processes described here,  I think it is important to run each step individullay when starting out to get a better handle what is going on and what each format that is generated is designed to do.  

### What is an [SRA file](https://en.wikipedia.org/wiki/Sequence_Read_Archive)?

SRA stands for sequence read archive is a bioinformatics database that provides a public repository for DNA sequencing data. An SRA file can come in many formats `.bam`, `.bed` etc. but we are interested in `.fastq` files.

### Downloading SRA files

The [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/) (GEO) is a public functional genomics data repository containing array- and sequencd based data. Although other repositories exist, some which require permission to access, many use GEO. 

We start by opening a text file in our home folder.

     nano sra_files.txt

This opens a `.txt` file in the text editor called `nano`. `Nano` has quite basic functionality, many use `vim`, but nano is fine for our purposes here. We need to copy all the SRA numbers of the SRA files we want to donwload into this file. 


