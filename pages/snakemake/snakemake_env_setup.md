---
layout: page
title: Snakemake environment setup
description: Set up snakemake environment?
---

The following script sets up up a virtual environment called `snakemake_tutorial` in your scratch area and installs snakemake 
and it's dependencies. As several dependency packages are downloaded to run snakemake, Mamba is downloaded first and used
to parallelise the process. All packages and and env info is stored in you scratch directory rather than home 
to avoid hitting the strict file number linits in the home directory. 

```bash
#! /usr/bin/env bash
# Requires a version of conda to be installed

PYDIR=/scratch/$(whoami)/snakemake_tutorial/

# Stop home area getting full of install files
conda config --prepend pkgs_dirs /scratch/$(whoami)/.conda/pkgs
conda config --prepend envs_dirs /scratch/$(whoami)/.conda/envs

mkdir -p /scratch/$(whoami)/snakemake_tutorial/

conda create -n $PYDIR python=3.9 mamba

conda activate $PYDIR

conda install -c conda-forge mamba
mamba create -c conda-forge -c bioconda -n snakemake snakemake
```

The following code sets up a directory tree that follows the . 
The [Gist](https://gist.github.com/Dazcam/6284597ad17f4da278f948893007b731) for this code is here.

```bash
#!/bin/bash

# Create basic smk dir tree - requires root dir as 1st param

HEAD_DIR=$1

mkdir -p ${HEAD_DIR}/{workflow/{envs,reports,rules,scripts},config,resources,results}

touch ${HEAD_DIR}/workflow/{Snakefile,snakemake.sh} \
${HEAD_DIR}/config/{config.yaml,cluster_config.yaml} \
${HEAD_DIR}/{README.md,.gitignore}

chmod 755 ${HEAD_DIR}/workflow/snakemake.sh
```

[test](https://gist.github.com/Dazcam/6284597ad17f4da278f948893007b731.js":include")

<script src="https://gist.github.com/Dazcam/6284597ad17f4da278f948893007b731.js"></script>

***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.md), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
