---
layout: page
title: SRA to Bed file walkthough
description: Basic high thoughput sequencing walkthrough
---

This tutorial is intended to show you how to download publicaly available ChIP-sea and ATAC-seq SRA files 
from a public repository and process them to produce `.bam` and `.bed` files for downstream analyses. 

Whilst there are more efficent tools available to automate the processes described here, I think it is 
important to run each step individually when starting out to get a better handle what is going on and 
what each format that is generated is designed to do.  

### What is an [SRA file](https://en.wikipedia.org/wiki/Sequence_Read_Archive)?

SRA stands for sequence read archive is a bioinformatics database that provides a public repository 
for DNA sequencing data. An SRA file can come in many formats `.bam`, `.bed` etc. but we are interested 
in `.fastq` files.

### Downloading SRA files

The [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/) (GEO) is a public functional genomics data 
repository containing array- and sequenced based data. Although other repositories exist, some which require 
access permissions, but many groups post their data on GEO. 

We start by opening a text file in our home folder on ROCKS.

     nano sra_files.txt

This opens a `.txt` file in the text editor called `nano`. `Nano` has basic functionality, many use `vim`, 
but nano is fine for our purposes here. We need to copy all the SRA numbers of the SRA files we want to 
download into this file. 

Leave the `nano` window open, and in your web browser navigate to this GEO page in a separtae tab. 

Obtaining SRA files from GEO repositories can be quite confusing. See [here](https://www.biostars.org/p/111040/) 
for tips. The best method I've found so far is described below. This is probably the most important part of the 
process as you can easily make a typo somewhere and download a compleletly different file. 

Normally you'll be directed to a GEO repository from a paper you are interested in. This will provide you with 
a GEO accession number like **GSEXXXXX** which is effectively the barcode for the central page for all the data
in the repository associated with the study.

In main repo page (given in paper) go to Query DataSets for GSEXXXXX under GEO accession number near top of the page.

![useful image]({{ site.baseurl }}/assets/geo_screenshot1.png)

This takes you to a list of all the replicates in the repo - choose SRA Run Selector to get to page containing SRA number for each replicate
SRA number is in row titled run:SRRXXXXXXX
Put all SRR numbers relating to your files of interest in a file called sra_list.txt, one file per line.
Download sra files using SRA toolkit prefetch
Then use parallel-fastq-dump to change SRA files to fastq files (see below)
