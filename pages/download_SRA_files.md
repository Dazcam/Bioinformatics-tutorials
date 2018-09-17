---
layout: page
title: Downloading SRA files
description: How to download SRA files from GEO
---

Once we have collected all our SRA numbers in our text file we then need to download the files to our
local system or the cluster. We use the SRA tools tool called `prefetch` to download the SRA files.

Firstly load the sra2peak virtual environment, we need to remain in this environment until the peak 
calling step.

~~~bash
source activate sra2peak
~~~

Then qquickly check if prefetch is installed correctly and in you current path type:

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

### Quick check to see if SRA files are **actually** type specifed in repo

On occasion files uploaded to the SRA repo will be annotated incorrectly, i.e. the will say the are single ended
when in fact the are paired ended. It may not be immediately obvious that this has occured so you can do a quick
sanity check by running this short bash script.

~~bash
#!bin/bash

for file in $(<sra_list.txt)
do

srr="SRR2920543.sra"
numLines=$(fastq-dump -X 1 -Z --split-spot $srr | wc -l)
if [ $numLines -eq 4 ]
then
  echo "$srr is single-end"
else
  echo "$srr is paired-end"
fi

done
~~ 

This will run one read (takes milliseconds) and quickly tells you if the SRA file is single or paired ended. See
[here](https://www.biostars.org/p/139422/) for more info/options.

***

Move on to [SRA to fastq]({{ site.baseurl }}/pages/SRA_2_fastq.html), or back
to [Get SRA file information]({{ site.baseurl }}/pages/get_sra_info.html).

