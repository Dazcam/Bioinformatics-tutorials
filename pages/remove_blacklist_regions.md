---
layout: page
title: Removing blacklisted regions
description: Removing blackisted regions from .bed files
---

Black listed regions are genomic regions that ENCODE found commonly provide false positive peaks across 
multiple ChIP/INPUT samples. These tend to be repetitive regions of the genome, or centromere/telmomeres. 
Details can be found [here](https://personal.broadinstitute.org/anshul/projects/encode/rawdata/blacklists/hg19-blacklist-README.pdf). 
BEDTOOLS is used to remove these regions. The required bed files are posted
[here](http://mitra.stanford.edu/kundaje/akundaje/release/blacklists/).
Bedtools documentation is excellent and is found [here](http://bedtools.readthedocs.io/en/latest/).

### Download blacklisted files for hg19

We use `wget` to download the blacklisted regions for hg19 from the brod institute. Regions for hg38 are 
also available in the link above.

~~~bash
wget http://mitra.stanford.edu/kundaje/akundaje/release/blacklists/hg19-human/Anshul_Hg19UltraHighSignalArtifactRegions.bed.gz
wget http://mitra.stanford.edu/kundaje/akundaje/release/blacklists/hg19-human/Duke_Hg19SignalRepeatArtifactRegions.bed.gz
~~~

As we are provided two separate files it is convient to intesect these to create a single file containing 
all the hg19 blacklisted regions for downsteam analyses. So we intesect the files using `bedtools intersect

~~~bash
bedtools intersect -v -a ${study}_peaks.narrowPeak \
-b Anshul_Hg19UltraHighSignalArtifactRegions.bed Duke_Hg19SignalRepeatArtifactRegions.bed \
 > ${study}_peaks.narrowPeak.blacklistRemoved.bed. 
~~~
