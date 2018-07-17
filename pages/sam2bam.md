---
layout: page
title: Coverting Sam files to bam files
description: Sam to bam file conversion using samtools
---

The alignment process has converted all our *fastq files* to *sam files*.

SAM stands for sequence alignment/map format and a *sam file* contains read information from a 
*fastq file* along with the alignment cordinates for each read that was added during the read alignment.
A detailed description of the *sam file* format can be found [here](http://samtools.github.io/hts-specs/SAMv1.pdf).

### Converting sam to bam files - Samtools

Samtools is an excellent package for converting *sam files* and for general manipulation of *sam* and 
*bam* files. *Sam files* are very large and are in tab-delimited format, not an ideal format for
manipulating programmatically. We therefore need to convert *sam files* into binary format (i.e. 
from text to 1s and 0s) which makes it easier for the computer to process. For this we use `samtools 
view`.

~~~bash
for file in ls `~/sam_files`
do

prefix=$(basename ${sample} ".sam")
echo ${prefix}

  
samtools view -b ${file} > ${prefix}.bam
done
~~~

The `-b` parameter specifies taht we want the output to be in `.bam` format. As the default output 
is printed to standard output (i.e. just printed to the screen) we write this to a file using `>`
followed by the name of the output file.  


*** 

### Sorting and indexing bam files

Many of the packages we use downstream require the *bam files* to be sorted and indexed to speed up
processing. We also use samtools for this. We can write a script that uses two simple `for` loops 
for this.

~~~bash
echo "===Sorting and indexing bam_files with samtools==="
echo "Started sorting bam files: " 
/bin/date
echo " "

for bam in *.bam
do
    name=$(echo $bam | awk -F"/" '{print $NF}' | awk -F".bam" '{print $1}')
    echo "Sorting: "$bam
    samtools sort $bam -o ${name}.srtd.bam
done

echo "Bam files sorted: " 
/bin/date
echo " "

echo "Started indexing sorted bam files: " 
/bin/date

for bam in *.srtd.bam
do
    name=$(echo $bam | awk -F"/" '{print $NF}' | awk -F".bam" '{print $1}')
    echo "Indexing: "$bam
    samtools index $bam ${name}.bam.bai
done 
    
echo "Sorted bam files indexed: " 
/bin/date
echo " "

echo "Sorting and Indexing DONE: " 
/bin/date
echo "=================================================="
~~~

Essentially all we need here are the two `for` loops. The additional `echo` statements are there to
introduce basic scripting and to demonstrate how you can control the output of your script to tell
you where you are in the process. The `bin/date` statements print the time and date to the screen and
gives you an idea of how long each process takes. This is useful for memory assignment on the cluster,
as you can assign more memory to memory intensive processeses.

Also notice that as we move through each process we alter the suffix of each file slightly. After 
sorting we add `.srtd.` to the new files and after indexing the sorted *.bam files* we add `.bai`. 
It is important to control your file names in this manner so you know at a glance how your files have 
been altered. 

Eventually you will want to remove superfluous files to save space but this method retains all files.    

***

Move on to [Alignment]({{ site.baseurl }}/pages/alignment.html), or back
to [Merging bam files]({{ site.baseurl }}/pages/bam_merging.html).
