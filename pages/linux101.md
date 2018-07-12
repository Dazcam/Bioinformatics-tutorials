---
layout: page
title: Linux 101
description: Some useful linux tips for bioinformatics
---

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
change that it would make to the screen. If we want to actually chnage the names of the files lisetd above we 
just remove the echo statement.

### Subsetting fastq files

Often when debugging a script we want to subset our fastq files so that we can significantly reduce the 
processing time for every step in our script rather than run all the data through at once. 

### Subsetting fastq files for testing

~~~bash
# read the compressed forward fasq file, 
# piping the result in sed to extract the 
# first 4 million lines (equivalent to 1M sequences, 
# as one sequence entry always span 4 lines)
# and finally compressing it as a new file
zcat pair_1.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_1.fastq.gz
# same for read 2 (the reverse read) if your data is Paired-End
zcat pair_2.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_2.fastq.gz
~~~

