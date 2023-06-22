---
layout: page
title: Snakemake introduction
description: What is snakemake and workflow management?
---

### What is snakemake?

![snakemake overview](https://snakemake.readthedocs.io/en/stable/_images/idea.png)

Snakemake is a workflow management and job scheduling tool which allows you to produce reproducible and 
scaleable bioinformatics workflows / pipelines. Workflow management tools allow you to automate multi-step
bioinformatics analyses (e.g. data collection, QC, processing and visualisation) and organise complex pipelines 
in a human readable manner. 

### Why should I use snakemake?

- **Automation**: Allows you to automate mundane steps of bioinformatics process to focus more on data analysis
- **Reproducibility**: Promotes scientifc reproducibility 
- **Error tracking**: Errors are easier to track reducing effort required to find and correct them
- **Portability**: Rule based system for subprocess organisation makes it easy to reuse / adapt workflow across projects / systems
- **Readability**: Snakemake uses python
- **File creation is tracked**: 
    - By completion: If a process crashes unexpectedly snakemake will automatically delete corrupted / incomplete files
    - By date: so if you change a file at stage 2 of the process snakemake will infom you to rerun all subsequent processes
- **Pipeline management and modularisation**: Promotes the modularisation of bioinformatics processes into digestable chunks

***

### Benefits of using snakemake on a computer cluster

- **Makes parellisation simple**: Manages scheduling of job submission to [cluster](http://snakemake.readthedocs.io/en/stable/executable.html#cluster-execution) (or to the [cloud](http://snakemake.readthedocs.io/en/stable/executable.html#cloud-support))
- **Easily assign default, and / or subprocess specific, resources**: No need for multiple shell scripts or SGE / SLURM headers i.e. parameters like `pe smp` and `h_vmem` can be specified in a [snakemake profile](https://snakemake.readthedocs.io/en/stable/executing/cli.html#profiles)
- **Supports all languages**: Any type of script can be run, and shell and python commands can be executed directly
- **Intermediate files can be [automatically removed](http://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-6-temporary-and-protected-files)**: temporary directory / file removal is simple 
- **Supports [benchmarking](http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#benchmarking)**: For example, to report CPU and memory usage
- **Supports [logging](http://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-5-logging)**: control messages/errors
- **Supports [config files](http://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html)** to abstract project specific details like filenames from the pipeline to promote code resuability and portability
  **Supports use of [environment modules](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#using-environment-modules)**: Environment modules on your local cluster can be pre-loaded in rule specific manner
- **Supports [Conda environments and package management](http://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#integrated-package-management)**
- **Supports [containers](https://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#running-jobs-in-containers)**
- **Many [pre-written wrappers](https://snakemake-wrappers.readthedocs.io/en/stable/) for common bioinformatics tasks**: No need to reinvent the wheel

### Alternatives to Snakemake

[Nextflow](https://www.nextflow.io)

***

Move on to [snakemake environment setup]({{ site.baseurl }}/pages/snakemake/02_snakemake_env_setup.html), or back 
to [index page]({{ site.baseurl }}/index.html).


