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

First navigate to the `/scratch/$(whoami)/smk-tutorial/workflow/` directory and create
some dummy fastq files:

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
            printf "\nfq processed.\n" >> {output}

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
    account:
    jobname: smk_tutorial
    queue: 
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
All you need to change is the email address here to you own so Snakemake can send you a completion or error 
report when the pipeline completes or and error is thrown.

```bash
snakemake --cluster-config ../config/cluster_config.yaml \
--cluster "sbatch --qos={cluster.queue} --time={cluster.duration} --account={cluster.account} --job-name={cluster.jobname} --mem={cluster.total_mem} --ntasks={cluster.num_cores} --output=smk.{rule}.%J.out --error=smk.{rule}.%J.err" \
-j 50 $@ 2> smk-"`date +"%d-%m-%Y"`".log
mail -s "Snakemake has finished" camerond@cardiff.ac.uk < smk-"`date +"%d-%m-%Y"`".log
```

All code in curly brackets here is referencing parameters we have set in the cluster config file. 

***

#### Dry run

Before we run our first pipeline we want to do a dry run. This has to be run from the directory 
that contains the `Snakefile`. During the dry run snakekame looks for the Snakefile then makes 
sure that all the input and output files in all rules are accounted for and checks the code for errors. 
To run the dry run type the following:

```bash
snakemake -np
``` 

A report is produced listing all the jobs that will be run with the wildcards filled in for
each separate job. Notice that the last job in the list is the rule `all` job.

```bash
# Only job stats shown here for brevity. You should see a list of individual jobs too.
Job stats:
job               count    min threads    max threads
--------------  -------  -------------  -------------
all                   1              1              1
process_fastqs        4              1              1
total                 5              1              1
```

***

#### Run the job

All we need to do now is run the snakemake script. However, as we need to run the script interactively
we need to set up a terminal multiplexer to run the script in the background. These are described in 
more detail [here](). On Hawk `screen` is pre-installed so we can use that.

To set up a new screen session by typing:

```bash
screen -S smk
```

This will open a new window which allows you to run your snakemake pipeline in the background. Note
that this is not the same environment that you were in previously so you need to reactivate the
smk-tutorial environment:

```bash
/scratch/$(whoami)/smk-tutorial/workflow/
```

Now you are set. Run the pipeline by typing:

```bash
./snakemake.sh
```

This will send the 4 `process_fastqs` to the appropriate Hawk queue:

```bash
squeue -u $USER
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
56885128       htc smk_tuto c.c14779 PD       0:00      1 (Priority)
56885127       htc smk_tuto c.c14779 PD       0:00      1 (Priority)
56885126       htc smk_tuto c.c14779 PD       0:00      1 (Priority)
56885125       htc smk_tuto c.c14779 PD       0:00      1 (Priority)
```

To detach from the screen type `ctrl+A` simultaneously then type `D`. The `snakemake.sh` 
script will continue running in the background. To reattach the screen you type:

```bash
screen -x smk
```

For a run down of the most common screen commands type `screen --help` or look [here](https://gist.github.com/jctosta/af918e1618682638aa82).

***

#### Output

Snakemake generates several log files. After the all the jobs have run your folder should look like this:

```bash
ls -1
envs
reports
rules
scripts
smk-24-05-2023.log
smk.process_fastqs.56885322.err
smk.process_fastqs.56885322.out
smk.process_fastqs.56885323.err
smk.process_fastqs.56885323.out
smk.process_fastqs.56885324.err
smk.process_fastqs.56885324.out
smk.process_fastqs.56885325.err
smk.process_fastqs.56885325.out
Snakefile
snakemake.sh
```

The `smk-24-05-2023.log` this is the general log file for the snakemake process. If you are running 
a pipeline with thousands of jobs, this log keeps you up to date with the submission and completions 
statuses of those jobs:

```bash
Submitted job 4 with external jobid 'Submitted batch job 56885324'.

[Wed May 24 15:38:07 2023]
rule process_fastqs:
    input: ../resources/fq/cortex_ExN_1.fastq
    output: ../results/01_process_fq/cortex_ExN_1.fastq
    jobid: 1
    wildcards: sample=cortex_ExN_1
    resources: tmpdir=/tmp

Submitted job 1 with external jobid 'Submitted batch job 56885325'.
[Wed May 24 15:39:06 2023]
Finished job 2.
1 of 5 steps (20%) done
[Wed May 24 15:39:06 2023]
Finished job 3.
2 of 5 steps (40%) done
[Wed May 24 15:39:06 2023]
Finished job 4.
3 of 5 steps (60%) done
[Wed May 24 15:39:06 2023]
Finished job 1.
4 of 5 steps (80%) done
Select jobs to execute...

[Wed May 24 15:39:06 2023]
localrule all:
    input: ../results/01_process_fq/cortex_ExN_1.fastq, ../results/01_process_fq/cortex_ExN_2.fastq, ../results/01_process_fq/cortex_ExN_3.fastq, ../results/01_process_fq/cortex_ExN_4.fastq
    jobid: 0
    resources: tmpdir=/tmp

[Wed May 24 15:39:06 2023]
Finished job 0.
5 of 5 steps (100%) done
```

If any job fails for any reason this is the first thing you would check to try to acertain at 
which point the pipeline failed. A copy of this log file is sent to your email addresss. 

The individual log files, e.g. `smk.process_fastqs.56885325.out` are the std out and error logs 
generated by Slurm for each individual `process_fastqs` job. Note that the name contains both the 
Slurm ID for the job and the rule the job came from so that you can cross-reference these when 
necessary. 

Lastly, lets quickly check that the ouput files were generated and their content is correct.

```bash
ls -1 ../results/01_process_fq/ 
cortex_ExN_1.fastq
cortex_ExN_2.fastq
cortex_ExN_3.fastq
cortex_ExN_4.fastq

cat ../results/01_process_fq/cortex_ExN_*
cortex_ExN_1
fq processed.
cortex_ExN_2
fq processed.
cortex_ExN_3
fq processed.
cortex_ExN_4
fq processed.
```

Great. All we have done here is copy the fastq files from the resources to results directory and 
added the text `fq processed.` to each file. At this stage, it doesn't really matter what each job 
is actually doing. It is improtant to understand that  snakemake is not interested in is what each 
job actually does. It is only interested in tracking files and setting parameters in order to 
communicate with Hawk, schedule your jobs and run your pipeline efficently. 

***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.html), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
