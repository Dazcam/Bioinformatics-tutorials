---
layout: page
title: Peak calling with Macs2 - bed files
description: Peak Calling
---

Now we have our *bam files* the next thing to do is to call peaks on them. Effectively we are looking for
genomic regions in our *bam files* that are enriched for aligned reads. We call these enriched regions
*peaks* as they contain read pile ups that form peaks. We can observe the peaks in our bam file using 
a genome browser such as IGV. However only those peaks that surpass a statistical threshold are retained
for downstream analysis. We can set this threshold manually. 

In ChIP-seq experiments we use the input sample as our background control, so only peaks that are in the
treatment sample but not in the background sample are retained. 

In ATAC-seq ??

*** 

### What are bed files?

When calling peaks on *bam files* we are generating a new type of file called a *bed file*. A *bed file* 
contains a list of genome wide peak coordinates. Each peak in the file has additional infomation associated 
with it such as statistical scores. A general description of the bed format can be found 
[here](https://genome.ucsc.edu/FAQ/FAQformat.html). Macs2 outputs three files as default. The one we are 
interested is the *.narrowPeak* file. This has a 6+4 bed format called and is described in the Macs2
documentation [here](https://github.com/taoliu/MACS), under the *output files* section. 

Note that the default statistical threshold for peaks identified by Macs2 is **FDR 0.05**.

***
 
### Calling peaks with Macs2

To call peaks on our *sorted* bam files we use the `macs2 callpeak` function. Rather than spell out what 
each flag does I have annotated this directly into the code block using the `#` sign. Any line of code
that starts with the hash is ignored, so we use this sign to annotate scripts.   

~~~bash
# -t = treatment (i.e. ChIP) file
# -c = control/background (i.e. INPUT) file if you have one
# -f = file format (BAMPE for paired end data)
# -n = Name you want to call output files 
# -g = The mappable genome size - hs for human
# MACS2 removes duplicates as default
# Default statistical threshold is **FDR 0,05**.

cd /$root/$main/$bamf/
echo "Moved to "/$root/$main/$bamf/
echo " "

echo "==================Peak calling==================="
echo "Peak calling started at: " 
/bin/date
echo " "

for file in *.srtd.bam
do
    prefix=$(basename ${file} ".srtd.bam")
    echo "Calling Peaks for: "${file}
    echo " "
    
    macs2 callpeak -t ${file} -f BAMPE -n ${prefix} -g hs --outdir ~/peak_files/
    echo " "
 
done

echo "Peak calling DONE: " 
/bin/date
echo "================================================"
~~~

*Bed files* are tab delimited and can be visualised using the head function.

~~~bash
head -50 myBedFile.narrowPeak 
~~~

Macs2 also produces an excel `.xls` file which contains almost identical information to the `.narrowPeak` 
file but it also contains a header which tell you all the paramters that were run when the file was 
generated which is very useful.

***

### Downstream applications

Now that you have *bam files* and *bed files* you are ready to move on to downstream apllications such as 
motif analysis or differential expression analysis. 

*** 



