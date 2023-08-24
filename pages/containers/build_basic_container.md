---
layout: page
title: Building a container for tdespec
description: Building a container for tdespec
---

**NOTE**: This method may need to be updated soon as singularity has changed it's name to 
apptainer. Some of the commands below (such as the `remote login` and `pull`
functions) don't work when using the apptainer module on HAWK. So for now use the
singularity module.

***

### Setup 

1. Go to the [Sylabs site](https://cloud.sylabs.io) and create an account. I used my Gitlab
   credentials to sign in.
2. Create a time sensitive access token. Navigate to the `access token` tab, add a label for your 
access token in the command box (I called mine 'hawk') and copy the access token to 
your clipboard. This token will be valid for one month.
3. `On HAWK`: load the sigularity module `module load singularity-ce/3.10.2` (don't use default
   singularity module as it is a symlink for apptainer).
4. Type `singularity remote login` and paste access token in when prompted and hit enter. 
5. If this is successful you should see `INFO: Access Token Verified!`. Access status can be checked at any time using `singularity remote status`. When your access token has run out it will say `FATAL:   error response from server: Invalid Credentials`. So you will need to generate a new access token and repeat the steps above.

***

### Create a container

Rather than create an entire container from scratch, it's best to find a pre-existing container that contains
the OS / packages that you need that you can add to. Here, I needed a container to run R in a conda /
snakemake environment as I was often encountering dependency clashes when trying to load R packages.
As `tdespec` uses some bioconductor packages I decided to use the [`bioconductor_docker`](https://github.com/Bioconductor/bioconductor_docker/tree/devel) image which contains all the 
bioconductor packages and many useful CRAN packages like `dplyr`.

To build the container you need a `definition file`. Navigate to the `remote builder` tab
in your Sylabs repository and copy the following into the `Definitions file` section.
(The definitions file can be much more complex than this but this is all we need.) 


```
Bootstrap: docker
From: bioconductor/bioconductor_docker:devel

%labels
    Version v0.0.1

%help
    This is container to run tdespec 

    BootStrap: docker


%post
    R --no-echo -e 'remotes::install_github("Dazcam/tdespec")'
```

Then name your container in the `Repository` box (I called mine 'containers/tdespec') and hit
submit. The container takes ~50 mins to build.  

Once built, copy the pull command at the top of the page and paste it into 
the command line to download the container.

```bash
singularity pull library://dazcam/containers/tdespec
```

***

## Running commands in the container.

To run R within the container use the following:

```R
singularity exec tdespec_latest.sif R
```


## Sections of a definition file

- **%environment**: Define bash variables within the container
- **%post**: Section to build packages above the bootstrapped environment. We could install R and python here for example.
- **%runscript**: Add scripts here to test the programs you just installed in `%post`



## Additional links:

- [Snakemake: running-jobs-in-containers](https://snakemake.readthedocs.io/en/master/snakefiles/deployment.html#running-jobs-in-containers)
- [ARCCA course](https://arcca.github.io/intro_singularity/index.html)
- [HSF course](https://hsf-training.github.io/hsf-training-singularity-webpage/01-introduction/index.html)
- [Pawsey course](https://pawseysc.github.io/singularity-containers/index.html)

***

