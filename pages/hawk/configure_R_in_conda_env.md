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

In some cases, it is possible to install a version of the packge to a local folder
but having two versions of the same R package that R can access can cause 
`segmentation faults` which happened to me recently trying to run edgeR's 
`cpm()` function.

```R
 *** caught segfault ***
address 0x2aff0b8aeff0, cause 'memory not mapped'
```

This can also cause errors such as the following:

```R
Activating conda environment: /scratch/$(whoami)/.snakemake/conda/a59cf92d36da73a499009a8120ab8eef
Error: package or namespace load failed for ‘tdespec’ in dyn.load(file, DLLpath = DLLpath, ...):
 unable to load shared object '/scratch/$(whoami)/R/library/Matrix/libs/Matrix.so':
  libRlapack.so: cannot open shared object file: No such file or directory
```

[`rcurl`](https://github.com/jeroen/curl/issues/129#issuecomment-339730996) is another package that causes issues. 

I hit skids recently when trying to download a small R package that I developed 
called `tdespec`. I wanted to test it in a conda environment and with snakemake
but trying to get it installed was a nightmare. GitHub based R packages 
are not available in any conda channels and I don't want to spend time 
uploding it to conda forge until it is properly tested. 

I did manage to get the packge uploaded and it ran fine with the toy data,
but when trying to run it with a fairly large snRNAseq dataset it 
choked. I realised that although I was loading an R module with the command 
`envmodules: "libgit2/1.1.0", "R/4.2.0"` in my snakemake rule because I was
using the `script` directive to call `../scripts/snRNAseq_rm_MHC_from_ref.R`
snakemake was not explicitly opening the R version loaded by the module, it
was running the script with another version of R, the baseline version
provided on the cluster. This is explained in more detail [here](). It 
was the same if using a conda environment using an `R.yml` file. One 
thing to try would be to run it using the `shell` directive and explcitly 
calling the specific `Rscript` executable. 

So I have attempted to get around this by creating a specific conda
environment for a specific versions of R using some tips oulined in these 
links:

- []([caught segfault - 'memory not mapped' error in R](https://stackoverflow.com/questions/49190251/))
- [Using R with Conda](https://www.biostars.org/p/450316/)
- [Why not R via conda](https://community.rstudio.com/t/why-not-r-via-conda/9438/4)



```R
# Stop home area getting full of install files
conda config --prepend pkgs_dirs /scratch/$(whoami)/.conda/pkgs
conda config --prepend envs_dirs /scratch/$(whoami)/.conda/envs

# installing Mamba for fasta downloading of packages in conda (if module not available on system)
# conda install mamba -n base -c conda-forge -y
# conda update conda -y
# conda update --all

# Creating R environment in conda (use conda-forge not r channel)
module load mambaforge
mamba create -n R-4.2.0 -c conda-forge r-base=4.2.0 -y

# Activating R environment (only load packages from conda forge, 
# don't mix channels i.e load bioconductor packages via conda
# otherwise the env does not resolve)
# And don't install R via the R channel use conda-forge
conda activate R-4.2.0
mamba install -c conda-forge r-essentials
mamba install -c conda-forge r-devtools
conda install -c conda-forge r-biocmanager
#mamba install -c bioconda bioconductor-edger bioconductor-ewce bioconductor-biomart

# Opens R an libpaths is what you would expect
/scratch/c.c1477909/.conda/envs/R-4.2.0/bin/R

# In R
BiocManager::install("edgeR")
BiocManager::install("biomaRt")
BiocManager::install("EWCE")
install.packages('SeuratObject')
install.packages('ggdendro')
devtools::install_github("Dazcam/tdespec")


# Smooth installations up until r-biocmanager via the conda-forge channel.
# It failed the [frozen and flexible](https://stackoverflow.com/questions/67528012) 
# solve saying that python 3.1 rather than python 3.9.2 (the default when the mamaba
# module is loaded) is required. 

# Option 1 - just try and install bioconductor packages from within R
# Option 2 - reload env with python 3.1 

# Option 1 worked but got this warning when loading tdespec:

# `Skipping 3 packages not available: edgeR, EWCE, biomaRt`

# It was suggested to use [this](https://stackoverflow.com/questions/74224357) 
# for installing github packages that require bioconductor packages. This may allow
# us to omit the BiocManager calls abbove:

remotes::install_github(
    "Dazcam/tdespec",
    repos = BiocManager::repositories()
)

# The pipeline works with with toy data on the head node
```

Still getting segmentation faults. Is this an MPI / SMP problem? Rule
this out.

