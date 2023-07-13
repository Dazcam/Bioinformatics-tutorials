---
layout: page
title: Building a container for tdespec
description: Building a container for tdespec
---

**NOTE**: This method may need updating as it appears that singularity is changing to 
apptainer. On Hawk some of the commands below (such as the remote login and pull
functions) don't work when using the apptainer module on Hawk. 

***

## Setup 

1. Go to https://cloud.sylabs.io and create an account, I used my Gitlab credentials.
2. Create an access token. Navigate to the `access token` tab, add a label for your 
access token in the command box (I called mine 'hawk') and copy the access token to 
your clipboard.
3. ON HAWK: load the sigularity module `module load singularity-ce/3.10.2` (note that the default is apptainer)
4. Yype `singularity remote login` and paste access token in when prompted and hit enter.
5. If this is successful you should see `INFO: Access Token Verified!`. This can be checked at any time 
using `singularity remote status`.

***

## Create a container

The ideal scenario is to find a prexisting container that you can add to rather than 
building one from stratch. As `tdespec` uses some bioconductor packages I decided to 
use the [`bioconductor_docker`](https://github.com/Bioconductor/bioconductor_docker/tree/devel) 
image which contains all the bioconductor packages. 

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

