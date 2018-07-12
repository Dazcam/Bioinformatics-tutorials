---
layout: page
title: Converting SRA files to fastq files
description: Using fastq-dump to convert SRA files to fastq files
---


Next we need to convert our SRA files to fastq files. If we want to do this individually we use `fastq-dump`,
which is also part of the SRA tools package. However, this can be a bit of a bottleneck in the pipeline so
there is another package called `parallel-fastq-dump` that can speed up the process.

### Using default processing - single ended data

For single ended files you can simply use a for loop to pass the files we downloaded to `fastq-dump` one at a
time.

~~bash
for file in $(<sra_list.txt)
do

fastq-dump ~/ncbi/public/sra/${file}.sra --outdir ~/fastq_files --gzip
done
~~

Note that the we add the location the files were downloaded to `~/ncbi/public/sra/` and the `.sra` to each

### Paired ended data

For paired ended data we only add on extra paratmeter `--split-reads` to the above line of code:

~~bash
fastq-dump ~/ncbi/public/sra/${file}.sra --outdir ~/fastq_files --split-files --gzip
~~

This extracts the paired ended data into separate files.

### Parallelisation

To speed up this process we effectively use the same parameters but add a `--threads` parameter and set this
to 8, which allocates 8 processors to the job rather than whatever your default is. The cluster parameters I
set for this are `#$ -pe smp 8`, `#$ -l h_vmem=5G` so thats 40Gs of memory allocated in total for the fastq
file conversion. We also add the `--sra-id`` parameter before the input file.

~~bash
for file in $(<sra_list.txt)
do

   parallel-fastq-dump --sra-id ~/ncbi/public/sra/${file}.sra --threads 8 \
   --outdir /c8000xd3/big-c1477909/PhD/sra_files/ --split-files --gzip
done
~~

The cluster parameters I set for this are `#$ -pe smp 8`, `#$ -l h_vmem=5G` so thats 40Gs of memory allocated in
total for the fastq file conversion.

