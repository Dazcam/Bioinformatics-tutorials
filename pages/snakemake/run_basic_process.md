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

We will discuss these below.

***

#### Create dummy fastq files

First navigate to the `/scratch/$whoami/snakemake-tutorial/workflow/` directory and create
some dummy fastq files.

```bash
mkdir -p ../resources/fq
for i in {1..4}; do echo "cortex_ExN_$i" > ../resources/fq/cortex_ExN_$i.fastq ; done
```

Which should create the following:

```bash
tree ../resources/fq/
../resources/fq/
├── cortex_ExN_1.fastq
├── cortex_ExN_2.fastq
├── cortex_ExN_3.fastq
└── cortex_ExN_4.fastq
```

***

#### Snakefile

The first file that we require is the Snakefile. This file is the central file used by Snakemake to
run your pipeline. Copy the following code into the Snakefile in the current directory.

```python
configfile: '../config/config.yaml'

rule all:
    input:
	expand("../results/01_process_fq/{sample}.fastq", sample = config['samples'])

rule process_fastqs:
    input:  "../resources/fq/{sample}.fastq"
    output: "../results/01_process_fq/{sample}.fastq"
    shell:
            """

            cat {input} > {output}
            printf "\nfq processed.\n" >> {output}"

            """
```

Snakemake allows you to modularise the various steps of your pipline into discreet modules called 
**rules**. At the moment all the rules in our pipeline are contained within the Snakefile, but as our 
pipeline grows, we can store rules elsewhere and use our Snakefile as the hub for all our sub-processes. 
At the moment our pipeline contains two rules `all` and `process_fastqs`. 
Let's read the code from the bottom up.

The `process_fastq` rule has 3 directives `input`, `output` and `shell` which are fairly self-explanatory. 
The `input` and `output` directives tells snakemake where to look for the input and output files 
when the `process_fastq` jobs are run and the `shell` directive contains the code (or script) that will be 
run for each instance of `process_fastq`.

Code in curly brackets after the shell directive are placeholders for the associated named directive. 
For example, `{input}` will be replaced with `../resources/{sample}.fastq` when the process is run. 
(We'll get to what replaces `{sample}` shortly.)

Every Snakefile must have an `all` rule. We need to pass all **final** output files that we want to track to 
the input directive of rule `all` for the snakemake process to function properly. At the moment, we only have 1 rule 
`process_fastq` so we only need to pass the output files of that rule to rule `all` for now, but as the pipeline 
expands we will have to manage how files are tracked from rule to rule and eventally to rule `all`. We'll cover 
this in more detail later.

Finally the first line `configfile: '../config/config.yaml'` tells snakemake where your config file is.

***


#### Config file

A [config(uration) file](https://en.wikipedia.org/wiki/Configuration_file) is a file used to set certain 
parameters and settings for your pipeline. You can use your config file to abstract out project specific 
information such as that pertaining to samples or donors. In theory, this means that the code in each rule 
is project agnostic, that is as it only contains general processing information it can be used from project 
to project all that would need to be changed are the details in your config file. 

Run the following to populate your config file:

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

Here we are creating a reference in our config file called `samples` which stores sample names associated
with our fastq files. Snakemake can reference this to populate the `{samples}` code in the `process_fastq` 
and `all` rules. Any parameters in curly brackets in the input and output sections of the rule is called a 
[wildcard](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#wildcards). We use wildcards 
as a means to generalise code. They act as placeholders that we can populate with information stored
in our config file, in `.csv` files or that we have generated in a function. The `{samples}` wildcard 
with be populated with the sample names stored in cour config file.

Note that if we wish to add or remove samples from the analysis that we only need to alter the 
config file, there is no need to alter the rules in the Snakefile. If we design our pipelines in this way it 
means we can reuse code for rules across projects. 

We tell snakemake to reference the config file in the `all` directive: `sample = config['samples']`. This is
telling Snakemake to set whatever is stored in the samples directive in the config file to the variable sample.

The `expand` function in rule `all` uses this information to create a list containing every possible 
unique expansion of the code in the first parameter. So expanding `{sample}` in 
`"../results/01_process_fq/{sample}.fastq"` the following list. 

```python
["../resources/fq/cortex_ExN_1.fastq", "../resources/fq/cortex_ExN_2.fastq", "../resources/fq/cortex_ExN_3.fastq", "../resources/fq/cortex_ExN_4.fastq"]
```

As this function is passed to `input` in rule `all`, Snakemake will be checking that these 4 files
exist at the end of the process.

By contrast, we don't use the expand function in the `input` and `output` sections of the `process_fastq` rule, 
so how does Snakemake propagate the `{sample}` wildcard here? When a wildcard is present without the expand function
snakemake will sequentially propagate the the wildcard with each item in the list. In our case, snakemake will run 
4 separate instances of the `process_fastq` rule, one for each sample listed in the config file. This allows us to 
run the 4 `process_fastq` jobs in parallel.

***

So a couple of important concepts here:

- `wildcard` - act as placeholders to generalise code that we can populate with specific information
- `expand` - ouputs every possible wildcard expansion / no `expand` wildcards are populated sequentially


***

#### Cluster config file

A cluster config file is used to set the Slurm parameters we need for each stage of the process. We can set 
default parameters and / or rule specific parameters. If no rule specific parameters are set snakemake will 
automatically resort to the default. This allows us to have fine control over the resources we set for each
rule.


Let's populate out cluster config file. Copy and paste the following into 
`../config/cluster_config.yaml`:

```bash
__default__:
    num_cores: 1
    total_mem: 10G
    duration: 0-05:00:00

# ---------  FASTQS  ----------
process_fastqs:
    num_cores: 1
    total_mem: 1G
    duration: 0-01:00:00
```

These parameters are referenced in the following shell script.

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

All code in curly brackets here is referencing parameters we have set in the cluster config file. 

***

#### Dry run

Before we run our first pipeline we want to do a dry run using:

```bash
snakemake -np
``` 

This has to be run from the directory that contains the `Snakefile`. During the dry run snakekame looks
for the Snakefile then makes sure that all the input and output files in all rules are accounted for and 
checks the code for errors. 


***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.html), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
