---
layout: page
title: Quality control - fastq files
description: Quality control of fastq files using FastQC and MultiQC
---

It is important to check the quality of the fastq files you have generated. It is possible, though
rare, that files are corrupted whilst downloading them from the public repository. Similarly the
group that uploaded the data may have been less/more stringent in their quality control 
considerations than your own group. 

### FastQC

FastQC provides an `html` document for each fastq file with key quality control metrics.

The details of what the specific metrics measure in the output `html` is explained extensively on 
the web, see this [video](https://www.youtube.com/watch?v=bz93ReOv87Y) made by the developers of
FastQC, so I wont cover this here.

The command to run fastqc is simple.

~~~bash
for file in `ls ~/fastq_files`
do

    fastqc ${file} -o ~/fastq_files/FastQC/

done
~~~

This is a fairly rapid process but can be parallelised by adding the `-t` parameter followed by an
integer for the number of fastq files in the folder.

***

### MultiQC

Often we have sereral fastqc `htmls` to inspect at once, the package MultiQC, condenses all the
information from all the FastQC reports into a single document making easier to quickly assess if 
all the fastq files pass the quality checks. 

First navigate to the fastq_file directory.

~~~bash
cd fastq_files
~~~

Then run `mutiqc`. We don't need a loop here as `multiqc` searches for all the fastqc `html` files 
in the current directory and adds them to the `multiqc` report.

~~~bash
multiqc .
~~~


The `.` is linux shorthand for current directory. We would get the same result using:

~~~bash
multiqc ~/fastq_files
~~~

If we are working on can then inspect the multiqc.html in your web browser. To open this from 
terminal on a mac use the `open` command.

~~~bash
open multiqc.html
~~~

However of you are running this on a cluster the easiest way to visualise the html report is to 
install a file trasfer package such as [Filezilla](https://filezilla-project.org) and transferring 
the html from the cluster to you machine.
 

***

### Common failures during QC

Often you will find that fastq files fail QCing as the reads still have adapters attached to them. Adapters
are essentially barcodes that are added to the reads in order that the sequencer can identify one read from 
the other. Most modern sequencers remove recognise adater sequences and remove them from the fastq files 
automatically, but for data generated on older machines these need to be removed programmatically. See the
[Trim Adapter](pages/trim_adapter.html) section for an explanation of how to do this.   

Move on to [Trim adapters]({{ site.baseurl }}/pages/trim_adapters.html), or back
to [SRA to fastq]({{ site.baseurl }}/pages/SRA_2_fastq.html).
