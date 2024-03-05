---
layout: page
title: Building a container on sylabs in layers
description: Building a container on sylabs in layers
---

On occasion it's necessary to build very large container. The build time limit (BTL) with [Sylabs](https://sylabs.io) 
is 1 hr, so if your container requires packages with lots of dependecies you'll often exceed the BTL. 
One way around this is to build the container in smaller chunks, and layering up the packages bit by bit.

Below is a `definition file` I've been using to build a container for Seurat 5. This version of
Seurat has new functionality to more efficently analyse large datasets (1M cells) and requires
several susidiary packages to facilitate this.

<details>

<summary>Definition file</summary>

```
Bootstrap: docker
From: bioconductor/bioconductor_docker:devel

%labels
    Version v0.0.1

%help
    This is a container to run Seurat 5 on Hawk

%post
    # Update the image
    apt update
    apt upgrade -y

    # for igraph
    apt install -y glpk-utils libglpk-dev

    # for sctransform
    apt install -y libicu-dev

    # for BPCells
    apt install -y libhdf5-dev

    # Install R packages
    R --no-echo -e 'remotes::install_github("bnprks/BPCells")'
    R --no-echo -e 'BiocManager::install("glmGamPoi")'
    R --no-echo -e 'install.packages("RPresto")'
    R --no-echo -e 'install.packages("Seurat")'
    R --no-echo -e 'setRepositories(ind=1:3)' # Needed to automatically install Bioconductor dependencies for Signac
    R --no-echo -e 'install.packages(c("R.utils", "Signac"))'
    R --no-echo -e 'remotes::install_github("satijalab/seurat-wrappers")'
    R --no-echo -e 'remotes::install_github("satijalab/azimuth")'
    R --no-echo -e 'BiocManager::install(c("scuttle", "scater"))'
    R --no-echo -e 'install.packages("scCustomize")'
    R --no-echo -e 'install.packages("readxl")'

    apt clean
```
</details>

Instead of running the entire thing at once, which exceeds the BTL, I built the container 
up until the `R --no-echo -e 'install.packages("Seurat")'` line first:

<details>

<summary>Layer 1</summary>

```
Bootstrap: docker
From: bioconductor/bioconductor_docker:devel

%labels
    Version v0.0.1

%help
    This is a container to run Seurat 5 on Hawk

%post
    # Update the image
    apt update
    apt upgrade -y

    # for igraph
    apt install -y glpk-utils libglpk-dev

    # for sctransform
    apt install -y libicu-dev

    # for BPCells
    apt install -y libhdf5-dev

    # Install R packages
    R --no-echo -e 'remotes::install_github("bnprks/BPCells")'
    R --no-echo -e 'BiocManager::install("glmGamPoi")'
    R --no-echo -e 'install.packages("RPresto")'
    R --no-echo -e 'install.packages("Seurat")'

    apt clean
```
</details>

This took about 45 minutes to build. I then used that container as the root conatiner and 
added the remianing packages to it.

<details>

<summary>Layer 2</summary>

```
Bootstrap: library
From: library://dazcam/containers/seurat5a:latest

%labels
    Version v0.0.1

%help
    This is a container to run Seurat 5 on Hawk

%post
    # Install R packages
    R --no-echo -e 'setRepositories(ind=1:3)' # Needed to automatically install Bioconductor dependencies for Signac
    R --no-echo -e 'install.packages(c("R.utils", "Signac"))'
    R --no-echo -e 'remotes::install_github("satijalab/seurat-wrappers")'
    R --no-echo -e 'remotes::install_github("satijalab/azimuth")'
    R --no-echo -e 'BiocManager::install(c("scuttle", "scater"))'
    R --no-echo -e 'install.packages("scCustomize")'
    R --no-echo -e 'install.packages("readxl")'
```
</details>

A few other points to mention here. 

First, notice the `apt` commands in layer A. `Apt` is a package
manager for linux distributions. From time to time, R packages require additional software to be added 
to the underlying linux kernal. These can be added as needed using `apt update / upgrade / install`.

Second, notice the difference in the headers for layer A and layer B. Layer A uses the bioconductor
base container as the input container from Docker Hub, whereas layer B uses the Layer A container 
stored in my personal singularity library.

***
