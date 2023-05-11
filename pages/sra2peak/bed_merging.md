---
layout: page
title: Merging bed files
description: Merging bed files using bedtools merge
---

Often after peak calling we find that we have generated several bed files for replicates that are related
and we wish to merge these files to create one consensus peakset to take forward for dowstream analyses.

For most processes where we need to manipulate bed files Bedtools is the go to package.



~~~bash
for file in `ls *.blrm.narrowPeak` 
do

prefix=$(basename ${sample} ".fastq.gz")
echo ${prefix}

cat ${prefix}.blrm.narrowPeak | bedtools sort -i stdin | bedtools merge -i stdin > {output}

done
~~~

FINISH THIS!!

***

Move on to [Additional tools]({{ site.baseurl }}/pages/sra2peak/extra_tools.html),
or back to [Removing blacklisted regions]({{ site.baseurl }}/pages/sra2peak/remove_blacklist_regions.html).
