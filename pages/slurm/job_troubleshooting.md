---
layout: page
title: Job troubleshooting in slurm
description: Job troubleshooting in slurm
---

#### Assessing resource usage

There are two common commands to use to check resource usage on Slurm:

+ `seff` - to obtain detailed job inormation on a single job
+ `sacct` - to obtain minimal data on a list of jobs

When using `seff <slurmjobid>` the ouput will look something like this:

```bash

```

<details>

<summary> Common seff commands  </summary> 

Running `sacct` shows the jobs you have run on the current date (i.e. since last midnight). 
To specify a time interval (must be within 3 months):

```bash
sacct -S YYYY-MM-DD
```

To print out all available data on a completed job:

```bash
sacct -l -j <slurmjobid> 
```

Use the `-o` option to specify specific output For example. The following shows
job name, job ID, used memory, job state and elapsed wall-clock time:

```bash
sacct -o jobname,jobid,maxrss,state,elapsed -j <slurmjobid> 
```

To print a list of all available data `sacct` data fields:

```bash
sacct -e
```

</details>

<details>

<summary> Common sacct commands  </summary> 
To print out all available data on a completed job:

```bash
sacct -l -j <slurmjobid> 
```

Use the `-o` option to specify specific output For example. The following shows
job name, job ID, used memory, job state and elapsed wall-clock time:

```bash
sacct -o jobname,jobid,maxrss,state,elapsed -j <slurmjobid> 
```

To print a list of all available data `sacct` data fields:

```bash
sacct -e
```

</details>

***

#### Parallelisation with Seurat

In order to parallelise a job on Hawk with Seurat you need to use a package called 
`future`. If working with snakemake, at the head of your file you need to run the 
following:

```r
future('multicore', workers = snakemake@threads)
options(future.globals.maxSize = 3e+09, future.seed = T)
plan()

#> multicore:
#> - args: function (..., future.seed = TRUE, workers = 19, envir = parent.frame())
#> - tweaked: TRUE
#> - call: future::plan("multicore", future.seed = T)
```

This sets up a multi-core job (SMP) (check this) on Hawk. The `future.seed` command is 
necessary to avoid the following warning:

<details>

<summary>Future seed warning</summary>

```r
Warning message:

UNRELIABLE VALUE: One of the 'future.apply' iterations ('future_lapply-1') unexpectedly
generated random numbers without declaring so. There is a risk that those random
numbers are not statistically sound and the overall results might be invalid. To fix this,
 specify 'future.seed=TRUE'. This ensures that proper, parallel-safe random numbers are
produced via the L'Ecuyer-CMRG method. To disable this check, use 'future.seed = NULL',
or set option 'future.rng.onMisuse' to "ignore".
```
</details>

***


</details>
