---
layout: page
title: Getting started
description: Setting up your home folder for bioinformatics
---

Once you have installed all the required software and all your executable files are organised and included
in your path we next want to create a series of directories to store all the files we need to create.

I cannot overstate the importance of keeping your file hierarchy and file naming structure organised when
running a bioinformatics pipleine. As you generate multiple file types, usually for many samples across
several different studies it's impossible to rememember where you put things, and what a paricular file is
unless you do this.

***

### Making a series of directories

If we want to send new files that we generate to a specific output location that **location must exist**. 
If we try to output a file to a non-existant location most programs will throw an error.

To make	a directory we use the `mkdir` command.

~~~bash
mkdir bickmore2016_hMG_ChIP_study
~~~

This creates a file for our study with an informative name using the first author and the date of our study
of interest and some information about the cell type of interest and the functional genomics technique used
in the study.

Next move into the new directory and create a series of sub-directories to store all the files we are going 
generate.

~~~bash
cd bickmore2016_hMG_ChIP_study
~~~

~~~bash
mkdir -p sra_files fastq_files/FastQC sam_files bam_files bed_files scripts output
~~~

This creates individual directories for all the files we intend to generate. Most a self explanatory, however
the `output` directory is for any output generated our pipeline and I always include a scripts folder to store
any additional scripts I generate. Notice that we have created a nested folder structure in the fastq_files
folder. 

The `-p` flag tells the program to search for the file we wish to create and to only create a new one if the 
file does not already exist. 

***

### Renaming groups of files

### Renaming groups of files

It is important to rename all your SRA/fastq files at the start of a bioinformatics pipeline to something
concise and memorable so that you can see at a glance what replicate/sample each file relates to. This
naming structure will then be maintained through all the subsequent steps in the pipeline.

There are many ways to do this but a simple and easily understandable method is to create two text files,
one called *SRA_numbers.txt* and the other called *SRA_names.txt*. We add the SRA numbers one per line to
*SRA_numbers.txt*.

~~~bash
SRR11111111
SRR11111112
SRR11111113
SRR11111114
SRR11111115
~~~

Then add a memorable name, perhaps containing the lead author of the paper and some replicate information,
for each SRA file to the *SRA_names.txt*, ensuring the positions in each text file for SRA numbers and
corresponding new name match.

~~~bash
Sample1
Sample2
Sample3
Sample4
Sample5
~~~

We then use a simple `while` loop, along with the paste function, to read in both text files and pass the
contents SRA_numbers.txt to the variable `old` and the contents SRA_names.txt to the varible new, one line
at a time. The old and new variables are then passed to the `mv` function which rename the files accordingly.

~~~bash
while read -r old new; do
   echo mv "${old}.sra" "${new}.sra"
done < <(paste SRA_numbers.txt SRA_names.txt)
~~~

As the echo statement is included above instead of this loop actually renaming everything it just prints the
change that would be made for each iteration of the loop. This is a great way to check if the entries in both
files correspond before actually running the loop properly.

~~~bash
mv SRR11111111 Sample1
mv SRR11111112 Sample2
mv SRR11111113 Sample3
mv SRR11111114 Sample4
mv SRR11111115 Sample5
~~~

If we want to actually change the names of the files listed above we just remove the echo statement.

***


