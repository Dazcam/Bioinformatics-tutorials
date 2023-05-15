

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
