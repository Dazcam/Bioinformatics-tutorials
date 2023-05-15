---
layout: page
title: Run basic snakemake process
description: Run basic snakemake process
---

#### 4 basic files

To run Snakemake you need 4 basic file types:

- Snakefile
- Shell script
- Config file
- Cluster config file

***

#### Create dummy fastq files

First navigate to the `/scratch/$whoami/snakemake-tutorial/workflow/` directory and create
some dummy fastq files.

```bash
mkdir -p ../resources/fq
for i in {1..5}; do echo "cortex_ExN_$i" > ../resources/fq/cortex_ExN_$i.fastq ; done
```

Which should create the following:

```bash
../resources/fq/
├── cortex_ExN_1.fastq
├── cortex_ExN_2.fastq
├── cortex_ExN_3.fastq
├── cortex_ExN_4.fastq
└── cortex_ExN_5.fastq
```

***

#### Snakefile

Here is the code for a basic Snakefile. It contains two rules `all` and `process_fastqs`.

```snakemake
rule all:
    expand("../results/01_process_fq/{sample}.fastq", sample = config['samples'])

rule process_fq:
    input:  "../resources/{sample}.fastq"
    output: "../results/01_process_fq/{sample}.fastq"
    shell:  "cat {input} > {output}; printf "\nfq processed.\n" >> {output}"
```

The `process_fastq` rule has 3 directives `input`, `output` and `shell`. The `input` directive
tells snakemake where the input files are, the `output` directive tells snakemake where
the ouput files will be when the `process_fastq` jobs are run and the `shell` directive contains 
the code (or script) that will be sent to the cluster for each instance of `process_fastq`.

The code adjacent to the shell directive in curly brackets are placeholders for the associated directives. 
For example, `{input}` will be replaced with `../resources/{sample}.fastq` when the jobs are run. 
(We'll get to what replaces `{sample}` shortly.)

Every Snakefile must have an `all` rule. We need to pass all **final** output files that we want to track to 
rule `all` for the snakemake process to function. At the moment, we only have 1 rule `process_fastq` so we
only need to pass the output files of that rule to rule `all` for now, but as the pipeline expands we will
have to manage how files are tracked from rule to rule and eventally to rule `all`.

***


#### Config file

Now let's populate our config file:

```bash
printf "samples:\n" > ../config/config.yaml
for i in {1..5}; do printf "    cortex_ExN_$i:\n" >> ../config/config.yaml  ; done
```

Which produces:

```bash
cat ../config/config.yaml 
samples:
    cortex_ExN_1:
    cortex_ExN_2:
    cortex_ExN_3:
    cortex_ExN_4:
    cortex_ExN_5:
```


Here we are creating a reference in our config file called `samples` which stores the names of our samples.
Snakemake can reference this to populate the `{samples}` wildcard in the `process_fastq` and `all` rules. 
Note that if we wish to add or remove samples from the analysis that we only need to alter the config file, 
there is no need to alter the rules in the Snakefile. If we design our pipelines in this way it means we can
reuse code for rules across projects. 

We tell snakemake to reference the config file in the `all` directive: `sample = config['samples']`. 

The expand function in rule `all` tells snakemake that there should be a final output file for every individual
sample under the `sample:` directive in the config file. The expand function resolves to:

```python
["../resources/fq/cortex_ExN_1", "../resources/fq/cortex_ExN_2", "../resources/fq/cortex_ExN_3", "../resources/fq/cortex_ExN_4", "../resources/fq/cortex_ExN_5"]
```

However, as we don't use the expand fuction in `process_fastq` snakemake will run a seprate instance of 
`process_fastq` for each unique sample. This allows us to run the jobs in `process_fastq` in parallel.

***

#### Cluster config file

Let's populate out cluster config file. Copy and paste the following into 
`/scratch/$whoami/snakemake-tutorial/config/cluster_config.yaml`:

```bash
__default__:
    num_cores: 1
    total_mem: 10G
    duration: 0-05:00:00

# ---------  CELL RANGER  ----------
process_fastqs:
    num_cores: 1
    total_mem: 1G
    duration: 0-01:00:00
```

In our cluster config file we can set the Slurm parameters we need for each stage of the process. We can set 
default parameters and / or rule specific parameters. If no rule specific parameters are set snakemake will 
automatically resort to the default.This allows us to have fine control over the resources we set for each
rule. 

***

#### snakemake.sh 

Finally we need to pull it all together with a shell script. This automatically manages the job scheduling 
for the pipeline and tells the cluster what resources to allocate (no need for anymore slurm directive headers).

```bash
snakemake --use-conda --use-envmodules \
--cluster-config ../config/cluster_config.yaml --keep-going \
--cluster "sbatch --qos={cluster.queue} --time={cluster.duration} --account={cluster.account} --job-name={cluster.jobname} --export=ALL --no-requeue --signal=2 --mem={cluster.total_mem} --output=smk.{rule}.%J.out --error=smk.{rule}.%J.err --ntasks={cluster.num_cores}" \
--keep-going \
-j 50 $@ 2> smk-"`date +"%d-%m-%Y"`".log 
mail -s "Snakemake has finished" camerond@cardiff.ac.uk < smk-"`date +"%d-%m-%Y"`".log
```

All code in curly brackets is referencing parameters we have set in the cluster config file. 

***

#### Dry run

Before we run our first pipeline we want to do a dry run using:

```bash
snakemake -np
``` 

This has to be run from the directory that contains the `Snakefile`. During the dry run snakekame looks
for the Snakefile then makes sure that all the input and output files in all rules are accounted for and 
check the code for errors. 


***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.html), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
