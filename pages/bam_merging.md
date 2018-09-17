---
layout: page
title: Merging bam files
description: Merging bam files using samtools mergee
---

When running a ATAC-seq or ChIP-seq analysis there may be several points where you will have to merge files
to pool information. This merging is usually done to *bam files* or the *bed files*. 

The are two secenarios where I've needed to merge bam files.

1. When a sample has been sequenced over several lanes, meaning we have multiple files for a single replicate 
(this is over and above the two files produced for paired end sequencing).

2. When multiple replicates have been produced for a ChIP treatment and/or input sample each with their own
*bam file* and these needed to be pooled for peak calling.    

***NOTE:** This section is for additonal information only - our bam files do not need to be merged.

***

### Merging bam files using Samtools - ChIP-seq

Often during a ChIP-seq analysis we have individual *bam files* for multiple technical replicates covering
several ChIP modifications (i.e. H3K4Me1, H3K27ac, SPI1 etc.) as well as our INPUT control replicates.

In order to call peaks appropriately on our data we need to pool the information in our *bam files* for all our 
replicates. So if we have the following:

+ **H3K4Me1** - 3 replicates
+ **H3K27ac** - 3 replicates
+ **SPI1**    - 3 replicates
+ **INPUT**   - 5 replicates

We could use samtools merge to create a pooled **bam file** for each condition.

~~~bash
samtools merge  H3K4Me1.mrgd.bam H3K4Me1_RepA.bam H3K4Me1_RepB.bam H3K4Me1_RepC.bam 
~~

Notice that we state the output file before the input files. This is a specific feature of *samtools* and is a 
common gotcha.

This method does the job but it creates 4 additional large files. If storage is not an issue then this method is
suitable to take your merged files on for peak calling. However, depending on you reasons for merging,  there is 
a workaround in Macs2 that allows you to specify multiple input files to the `treatment` and `control` parameters. 

***

### MACS2 workaround for merging `.bam` files - ChIP-seq

To save storage space and circumvent the need to merge `.bam` files manually, you can just pass all the technical 
replicates for a particular condition directly to *macs2* and it will merge them for you as part of the peak calling 
process.

~~~bash
macs2 callpeak -t H3K4Me1_RepA.bam H3K4Me1_RepB.bam H3K4Me1_RepC.bam \
-c ChIPinput_RepA ChIPinput_RepB ChIPinput_RepC ChIPinput_RepD ChIPinput_RepE \ 
-f BAMPE -B -g hs --outdir peak_files/ -n H3K4Me1
~~~
 
The `\` is a means of taking a line break if your line of code is spread all over the page. 

Although you may need extra space my prefrence here is to use the *samtools* method, as it is far easier to code 
loops/larger scripts when dealing with single files.  

***

Move on to [Calling peaks]({{ site.baseurl }}/pages/calling_peaks.html),  or back
to [Converting sam files to bam files]({{ site.baseurl }}/pages/sam2bam.html).


