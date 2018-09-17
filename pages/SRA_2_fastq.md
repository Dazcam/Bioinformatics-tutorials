---
layout: page
title: Converting SRA files to fastq files
description: Using fastq-dump to convert SRA files to fastq files
---

### Converting SRA files to fastq files

Next we need to convert our SRA files to fastq files.

A fastq file is a text based file which contains seqeuence information (A,T,C,G) for all the short reads
produced by a high throughput sequencer. Each read contains 4 lines of information in the fastq file and
each individual base in a particular read has an associated quality score. For more information see
[here](https://en.wikipedia.org/wiki/FASTQ_format).

To do this serially we use `fastq-dump`, which is also part of the SRA tools package.

#### Single ended data

Our data is paired ended, but for the sake of completeness we can extract single ended files using a for 
loop to pass that passed the sra files we downloaded to `fastq-dump` one at a time.

~~~bash
for file in $(<sra_list.txt)
do

fastq-dump ~/ncbi/public/sra/${file}.sra --outdir ~/fastq_files --gzip

done
~~~

Note that we add the location that the files were downloaded to `~/ncbi/public/sra/` and the `.sra` to suffix
to each *sra number* we read in. We specify the output directory using the `--outdir` flag and the `--gzip`
flag tells the program to **zip**, or compress, the *fastq files* to save storage space.

***

#### Paired ended data

For paired ended data we only add one extra parameter `--split-reads` to the above line of code:

~~~bash
for file in $(<sra_list.txt)
do

fastq-dump ~/ncbi/public/sra/${file}.sra --outdir ~/fastq_files --split-files --readids --gzip

done
~~~

This extracts the paired ended data into separate files. Both the `--split-files` and `--readids` flags are 
essential here otherwise paired end information is not annotated correctly. the documentation for the 
`fastq-dump` package is rubbish so it not always immediately obvious which flags are required and why, this
[site](https://edwards.sdsu.edu/research/fastq-dump/) provides a much better description of what is needed. 

This part of the pipeline can be a bit of a bottleneck so there is another package called `parallel-fastq-dump` 
that can speed up the process. 

***

#### Parallelisation

To speed up this process we effectively use the same parameters but add a `--threads` parameter. I set this
to 8, which allocates 8 processors to the job but beware when parallelising jobs, you don't want to allocate
all the processors you have at once as nothing will be left to run essential background processes and your
computer will crash. Noramally you retain one processor for background processes and assign the rest to the
job.

The cluster parameters I set for this are `#$ -pe smp 8`, `#$ -l h_vmem=5G` so thats 40Gs of memory allocated
in total for the fastq file conversion. We also add the `--sra-id`` parameter before the input file when
using parallel-fastq-dump.

~~~bash
for file in $(<sra_list.txt)
do

   parallel-fastq-dump --sra-id ~/ncbi/public/sra/${file}.sra --threads 8 \
   --outdir /c8000xd3/big-c1477909/PhD/sra_files/ --split-files --gzip --readids

done
~~~

The cluster parameters I set for this are `#$ -pe smp 8`, `#$ -l h_vmem=5G` so thats 40Gs of memory allocated in
total for the fastq file conversion.

***

### Rename your files

Open a text file in nano called **fastq_names.txt**:

~~~bash
nano fastq_names.txt
~~~

Then type memorable names for each of the fastq files you have generated in the text file.

~~~bash
Mono_1
Mono_2
Tcell_1
Tcell_2
~~~

Save the file by pressing ctrl **x**, then **y**, then **enter**.

Then as decribed earlier we use a while loop to remove the old sra numbers from our fastq files into 
something more meaningful.

~~~bash
while read -r old new; do
   echo mv "${old}.fastq.gz" "${new}.fastq.gz"
done < <(paste SRA_list.txt fastq_names.txt)
~~~

We are now ready to do some simple quality control on the fastq files.

***

Move on to [Quality control - FastQC/MultiQC]({{ site.baseurl }}/pages/fastqc.html), or back
to [Download SRA files]({{ site.baseurl }}/pages/download_SRA_files.html).
