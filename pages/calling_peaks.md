---
layout: page
title: Peak calling with Macs2 - bed files
description: Calling peaks from BAM files using Macs2
---



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

    for bam in *.srtd.bam
    do
         name=$(echo $bam | awk -F"/" '{print $NF}' | awk -F".bam" '{print $1}')
         echo "Calling Peaks for: "$bam
         echo " "
         macs2 callpeak -t $bam -f BAMPE -n ${name} -g hs --outdir /$root/$main/$peakf/
         echo " "
    done

    echo "Peak calling DONE: " 
    /bin/date
    echo "================================================"
