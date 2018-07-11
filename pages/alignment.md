---
layout: page
title: Aligning reads to reference genome
description: How to download align fastq file reads to genome
---

### Downloading reference genomes

To align the short reads in our fastq files to a reference genome we use the package Bowtie2, however,
we must first download a reference genome to align our reads to. We only need to do this once for each 
genome we intend to use. These genome files also need to be indexed. 

Our data were generated from human tissue using the hg19 build. So we need to download the **hg19** 
build of the human genome. Be aware that this is not the most up to date genome, however many groups
still align to this genome as most publicaly available data is aligned to hg19.

We download the reference from UCSC. 

From your home folder navigate to the `genomes_and_index_files` folder:

    cd genomes_and_index_files

Then type the following:

   wget http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz

The `wget` command is a means of downloading files from the web using linux. This is quite a big files
so may take a wee while. Once complete, we have downloaded a fasta file containing the entire human 
genome.

### Indexing the reference genome

Before aligning our reads we need to first index our reference genome. This allows Bowtie2 to access 
the genome in a more efficient manner. Indexing only needs to be done once per genome.

   bowtie2-build hg19.fa hg38
  
This creates a few additional files in your `genomes_and_index_files` folder.

### Running Bowtie2

To align the reads in our fastq files we write a for loop to run through all the files in our 
`fastq_files` folder, one at a time.  

    for file in `ls ~/fastq_files`
    do

        prefix=$(basename ${sample} ".fastq.gz")
        echo ${prefix}

        bowtie2 -p 7 -x ~/genomes_and_index_files/hg19 \
        -U ${file}  -S ~/sam_files/${prefix}_hg19.sam
    done

The `-p` parameter is for parallelisation and assigns **7 processors** to the job (only do this on the
cluster - unless you know how many processors you have on your machine). This will generate `.sam` files
in the `sam_files` folder (specified using the `-S` parameter). 

***

### Additonal info

There are a couple of useful extra tips to introduce here.

Notice the use of the `basename` function in the for loop. This is a concise way to strip the directory
tree and suffixes from filenames. 

    basename /home/David_Bowie/fastq_files/fastq_1.fastq.gz "fastq.gz"

The above command returns only `fastq1`, removing everything before the rightmost forward slash, and 
whatever suffix you specify in the quotes.

This is useful for two reasons it means you can assign this abbrieviation to a variable which you can 
use in the naming stucture of subsequent output files to preserve nameing continuity between different 
file formats. Here we assign the abbriviation to the variable `prefix`.

Secondly, as many programs do not produce output as they process files, we need to manually add `echo`
statements to inform us of what processing has been done and what is left to do. This is important when 
running loops that take time to complete, and even more important if your writing a script to automate
multiple processes. Occasionally, session will crash for unexplainable reasons, without simple 
signposts to let us know what was completed safely before the crash, it can be difficult to indentify 
those files that may have been corrupted.      

 


  
