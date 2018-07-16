---
layout: page
title: Trim adapters from sequencing reads in fastq file
description: Using Trim Galore! for adapter trimming
---

In order for DNA to be read by the sequencer, adapters are added to each end of the DNA sequence. Most 
modern sequencers recognise the adapters and remove these automatically but older sequencers do not, so
the adapters need to be removed manually. If adapters are left in place your read alignment percentages 
will be very low when you come to align the reads your fastq files.

I have only done this once so far, this was on  paired end data and I used the package Trim Galore! to 
do it.

~~~bash


trim_galore --nextera -q20 --paired -o 
~~~

 
***

![pretrim]({{ site.baseurl }}/assets/pretrim.png){:height="250px" width="500px" .center-image }

***


***

![posttrim]({{ site.baseurl }}/assets/posttrim.png){:height="100px" width="500px" .center-image }

***
