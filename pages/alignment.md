---
layout: page
title: Aligning reads to reference genome
description: How to download align fastq file reads to genome
---

To align the short reads in our fastq files to a reference genome we use the alignement package Bowtie2, 
however, we must first download a reference genome to align our reads to. We only need to do this once for 
each genome we intend to use. These genome files also need to be indexed. 

### Downloading reference genomes

Most relevant human data currently in the literature is aligned to hg19 but as hg38 is the most up to 
date genome build it will eventually become the standard as groups move across to it. Note the *hg* and 
*GRCh* notations are used interchangebly.  

+ hg19 = GRCh37 from 2009
+ hg38 = GRCh38 from 2013

Our data were generated from human tissue using the hg19 build. So we need to download the **hg19** 
build of the human genome. 

We download the reference from UCSC. 

From your home folder navigate to the `genomes_and_index_files` folder:

~~~bash
cd genomes_and_index_files
~~~

Then type the following:

~~~bash
wget http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz
~~~

The `wget` command is a means of downloading files from the web using linux. This is quite a big file
so may take a wee while. Once complete, we have downloaded a fasta file containing the entire human 
genome.

***

### Indexing the reference genome

Before aligning our reads we need to first index our reference genome. This allows Bowtie2 to access 
the genome in a more efficient manner. Indexing only needs to be done once per genome.

~~~bash
bowtie2-build hg19.fa hg38
~~~
  
This creates a few additional files in your `genomes_and_index_files` folder.

### Aligning reads to hg19 with  Bowtie2

To align the reads in our fastq files we write a `for` loop to run through all the files in our 
`fastq_files` folder, one at a time.  

~~~bash
for file in `ls ~/fastq_files`
do

    prefix=$(basename ${sample} ".fastq.gz")
    echo ${prefix}

    bowtie2 -p 7 -x ~/genomes_and_index_files/hg19 \
    -U ${file}  -S ~/sam_files/${prefix}_hg19.sam

done
~~~~

The `-p` parameter is for parallelisation and assigns **7 processors** to the job (only do this on the
cluster - unless you know how many processors you have on your machine). This will generate a *sam 
file* for each of your *fastq files* in the *sam_files* folder (specified using the `-S` parameter). 

This command is for single ended data, and the single input file is passed to the `-U` flag. If your 
data is paired ended, the flags are `-1` and `-2`. Regardless of whether your reads are single or 
paired ended a **single *sam file*** is always produced by Bowtie2.


**Remember**: If you had to trim adapters you need to change the input folder in the first line of the
for loop to `~/trimmed_fastq_files`

***

### Additonal tips

There are a couple of useful extra tips to introduce here.

Notice the use of the `basename` function in the `for` loop. This is a concise way to strip the directory
tree and suffixes from filenames. 

~~~bash
basename /home/David_Bowie/fastq_files/fastq_1.fastq.gz ".fastq.gz"
~~~

The above command returns only `fastq1`, removing everything before the rightmost forward slash, and 
whatever suffix you specify in the quotes.

This is extremely useful can assign this abbreviation to a variable which you can call to preserve naming 
continuity when generating different file formats. Here we assign the abbreviation to the variable *prefix*.

***

### Signposting - echo statements

Many programs do not produce output as they process files, so when scripting in bash it helps to 
manually add *echo* statements to inform us of what processing has been completed and what there is still 
to do. 

This is important when running loops on multiple files that takes considerable to complete, and even more 
important if you're writing a script to automate multiple processes. Occasionally, sessions will crash for 
unexplainable reasons, so without simple signposts to let us know what was completed safely before the crash, 
it can be difficult to indentify those files that may have been corrupted.      

***


  
