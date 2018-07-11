---
layout: page
title: Peak calling with Macs2 - bed files
description: Calling peaks from BAM files using Macs2
---


~~~ bash
# -t = treatment (i.e. ChIP) file
# -c = control/background (i.e. INPUT) file if you have one
# -f = file format (BAMPE for paired end data)
# -n = Name you want to call output files 
# -g = The mappable genome size - hs for human
# MACS2 removes duplicates as default

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
