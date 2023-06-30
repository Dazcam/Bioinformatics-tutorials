---
layout: page
title: Be aware of HAWK environment settings 
description: Be aware of HAWK environment settings 
---

Something to be aware of in Hawk is that Conda, python and R versions
change depending on the environment you are in. This can be important
when installing packages (particularly R packages within R) as the 
version of R you think you are using may not be the one you are 
**actually** using. For example here are the settings for the 
3 most common environments that I use:

**Base**

```bash
Conda version: NULL
Python version: Python 2.7.5
R version: /apps/languages/R/4.0.3/el7/AVX512/gnu-8.1/bin/R
```

**Conda**

```bash
Set in ~/.myenv: eval "$(/apps/languages/anaconda/2020.02/bin/conda shell.bash hook)"

Conda version: conda 4.8.3
Python version:  Python 3.7.6
Python loc: /apps/languages/anaconda/2020.02/bin/python
R version: /apps/languages/R/4.0.3/el7/AVX512/gnu-8.1/bin/R
```

**Snakemake**

```bash
Conda version: conda 4.8.3
Python version:  Python 3.8.3
Python loc: ~/.conda/envs/snakemake/bin/python
R version: /apps/languages/R/4.0.3/el7/AVX512/gnu-8.1/bin/R
```

Notice that the conda and python versions change but the R versions stays constant. 

After running `module load R/4.2.0` the R version and location changes:

```bash
R version: /apps/languages/R/4.2.0/el7/AVX512/gnu-7.3/bin/R
```

***

Now let say you have created a conda environment with a new version of R
that is different from the version on Hawk. You have to be careful to make 
sure that the version of R you are using within the virtual environment is 
not the Hawk version. By just typing `R` in the virtual environment you will
be opening the Hawk version of R. To open the virtual environment specific
version of R you will need to give the explicit path for it i.e.:

```bash
/scratch/$(whoami)/.conda/envs/R/bin/R
```

Then within R, you will see that the associated R library is stored in
a similar location, and crucially, that there is no Hawk specific R 
library path in the `.libPath`:

```R
.libPaths()
#> [1] "/scratch/c.c1477909/.conda/envs/R/lib/R/library"
```


#### Questions 

1. Compilers



