---
layout: page
title: Configuring R in a Conda environment on HAWK
description: Configuring R in a Conda environment on HAWK
---

This can be a bit of a minefield. Running R and many of it's packages is 
fairly straightforward on Hawk using the base version of R on the system 
or one of the later versions by loading the associated module i.e.
`module load R/4.2.0`.  

My initial approach was to use the same local `R_library` for all my projects 
saved in `/scratch/$(whoami)/R/library` rather than having a bespoke 
R_library for each one to save having multiple copies of the same package 
in multiple directories. 

I had also been using `install.packages()` and `BiocManager::install()` to
install the majority of my packages but found that in some instances, such 
as packages that depended on the use of `git`, if `install.packages('devtools')`
failed it usually worked if I pre-loaded the module `libgit2` the same
command would now work. 

However, I noticed that when trying to update packages some base-R packages
could not be updated. And when checking the R library paths from within R,
the directory that these base-R packages were stored in was always present,
regardless of how I tried to set my prefered library path:


```R
.libPaths()
#> [1] "/scratch/$(whoami)/R/library"                             
#> [2] "/apps/languages/R/4.0.3/el7/AVX512/gnu-8.1/lib64/R/library"
```

In some ways this is unsurprising as to run a Hawk installed version of R
we will need the basic packages, such as those in the package `base`, otherwise
the R wont run anything. But, as we don't have permissions to alter those
files it can lead to problems if you require a more up-to-date version of 
one of the packages resolve a conflict with a package you're trying to install. 

In some cases, it is possible to install a local version but having two
versions of the same package that R can access can cause `segmentation faults`
which happened to me recently trying to run edgeR's `cpm()` function.


```R
 *** caught segfault ***
address 0x2aff0b8aeff0, cause 'memory not mapped'
```

[`rcurl`](https://github.com/jeroen/curl/issues/129#issuecomment-339730996) is another package that causes issues. 

I hit skids when trying to download a small package I developed called 
`tdespec`. I wanted to test it in a conda environment and with snakemake
but trying to get it installed was a nightmare. GitHub based R packages 
are not available in any conda channels and I don't want to spend time 
uploding it to conda forge until it is properly tested. 

I did manage to get the packge uploaded and it ran fine with the toy data,
but when trying to run it with a fairly large snRNAseq dataset it 
choked. Add why.

This lead me to realising that I wasn't using  

Useful links: 

- [Using R with Conda](https://www.biostars.org/p/450316/)
- [Why not R vioa conda](https://community.rstudio.com/t/why-not-r-via-conda/9438/4)

```R
# Stop home area getting full of install files
conda config --prepend pkgs_dirs /scratch/$(whoami)/.conda/pkgs
conda config --prepend envs_dirs /scratch/$(whoami)/.conda/envs

# installing Mamba for fasta downloading of packages in conda
conda install mamba -n base -c conda-forge -y
conda update conda -y
conda update --all

# Creating R environment in conda (use conda-forge not r channel)
module load mambaforge
mamba create -n R -c conda-forge r-base=4.2.0 -y

#Activating R environment
conda activate R
mamba install -c conda-forge r-essentials
mamba install -c conda-forge r-devtools
mamba install -c bioconda bioconductor-edger bioconductor-ewce bioconductor-biomart

# Opens R an libpaths is what you would expect
/scratch/c.c1477909/.conda/envs/R/bin/R

# In R
devtools::install_github("Dazcam/tdespec")
