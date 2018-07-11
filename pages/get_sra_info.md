---
layout: page
title: Collecting SRA file information from a public repository
description: Where to get SRA files, and what SRA files are
---

### What is an [SRA file](https://en.wikipedia.org/wiki/Sequence_Read_Archive)?

SRA stands for sequence read archive and the archive is a bioinformatics database for DNA sequencing data. 
SRA files are stored in the archive and can come in many formats `.bam`, `.bed` etc. but, in this tutorial,
we are interested in downloading `.fastq` SRA files.

### Where are the files stored?

The [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/) (GEO) is a public functional genomics data 
repository containing array- and sequenced based data. Although other repositories exist, some which require 
access permissions, many groups opt to post their data on GEO. 

Obtaining SRA files from GEO repositories can be quite confusing. See [here](https://www.biostars.org/p/111040/)
for tips. The best method I've found so far is described below. 

We need to find the SRA numbers for the files we are interested in and annotate them in a sensible way so we 
can keep track of the files as they move through the pipeline and understand what they are if we come back to 
2 years from now. TThis is probably the most important part of the whole process as you can easily make a 
typo somewhere and download a completely different file from a different study!

***

### Open a text editor

We start by opening a text file in our home folder on ROCKS.

~~~bash
nano sra_files.txt
~~~

This opens a new `.txt` file called '*sra_files*' in the text editor called `nano`. `Nano` has basic functionality, 
many use `vim`, but nano is fine for our purposes here. We want to copy all the SRA numbers of the SRA files 
we're interested in into this file. 

Leave the `nano` window open, and in your web browser navigate to this GEO page in a separtae tab (use the GEO 
link above). 

***

### Obtain SRA files from  GEO repository for study of interest

Normally you'll be directed to a GEO repository from a paper you are interested in. This will provide you with 
a GEO accession number like **GSEXXXXX** which is the code for the central page containing the data associated 
with the study.

The accession number for our study is **GSE63137**. Type this into the box that says '*Keyword or GEO Accession*'
and hit 'SEARCH'.

This takes you to the main page for our study of interest. This page provides a brief summary of the study, 
contact details for the PIs and, if you scroll down to the 'Samples' section and hit 'More', you can see what 
types of data are stored in the repo. We can see there are data for 31 samples in total with 6 ATAC-seq samples 
and 11 ChIP-seq samples.

Go back to the top of the page and click the `Query DataSets for GSE63137` link.

![useful image]({{ site.url }}/blob/gh-pages/assets/get_QueryDataset.png)

This takes you to another list of all the replicates in the repo. We want the SRA numbers for files 10-26 inclusive. 
Navigate to file `10`, which is the first ChIP-seq dataset, and select the 'SRA Run Selector' link. 

![useful image]({{ site.url }}/assets/getSRA_SRARunSelector.png)

This takes us to the page containing the SRA file number/s for that replicate. The SRA number we need is listed next to `Run:` and is 
SRR1647895.

![useful image]({{ site.url }}/blob/gh-pages/assets/getSRA_SRAnumber.png)

Note: Occasionally there are more than one SRA files per replicate, this means that the data was generated over 
multiple lanes in the sequencer and the files would need to be merged in the in the pipeline.

There is some other important information here. Notice under `LibraryLayout:` it says `SINGLE` this means the data 
is single ended rather than paired ended, we need to know this as it will influence some of the parameters we need to 
specify later on.

For now copy an paste the SRA numbers for all 17 replicates into the text file one file per line.

As alluded to earlier, it is important to keep an accurate record of the replicate information each SRA 
number refers to. This study is annotated fairly clearly, but this is not always the case. At this stage I usually
create a table containing all the replicate information using abbriviated IDs. These IDs are substitued into the file
names instead of the SRA numbers to make it easier to see what files are affected during the pipeline. 
 
Move on to [Download SRA files](pages/download_SRA_files.html), or back to [Required packages](pages/required_packages.html).
 

