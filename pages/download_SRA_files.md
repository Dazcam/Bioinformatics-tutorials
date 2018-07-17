---
layout: page
title: Downloading SRA files
description: How to download SRA files from GEO
---

Once we have collected all our SRA numbers in our text file we then need to download the files to our
local system or the cluster. We use the SRA tools tool called `prefetch` to download the SRA files.

First quickly check if prefetch is installed correctly and in you current path type:

~~~bash
prefetch 
~~~

If it produces a usage statement we're good to go if not, the package is either not on your system or 
not in your path. See [Installing required packages]({{ site.baseurl }}/pages/installing_required_packages.html)
page.

To download the SRA files we write a short for loop. 

~~~bash
for file in $(<sra_list.txt)
do
    prefetch ${file}
done
~~~

This effectively loops through the SRA files in the list we generated and runs one instance of prefetch
on each file separately. 

One slight annoyance with prefetch is there is not a simple way to sepcify the output folder for the SRA 
files they go to the following location `/home/[USER]/ncbi/public/sra/`. This can be changed but is a bit 
of a faff and beyond the scope of this tutorial. If you really want to change it see 
[here](https://www.biostars.org/p/175096/), but we can work around this in the next command.

***

Move on to [Get SRA file information]({{ site.baseurl }}/pages/get_sra_info.html), or back
to [SRA to fastq]({{ site.baseurl }}/pages/SRA_2_fastq.html).

