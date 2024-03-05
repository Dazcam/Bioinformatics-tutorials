
On occasion it's necessary to build very large container. The build time limit (BTL) with sylabs is 1 hr, 
so if your container requires packages with lots of dependecies you'll often exceed the BTL. One way
around this is to build the container in smaller chunks, and layering up the packages bit by bit.

Below is a `definition file` I've been using to build a container for Seurat 5. This version of
Seurat has new functionality to more efficently analyse large datasets (1M cells) and requires
several susidiary packages to facilitate this.



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
    R --no-echo -e 'remotes::install_github("satijalab/seurat-data", "seurat5", quiet = TRUE)'
    R --no-echo -e 'remotes::install_github("satijalab/azimuth", "seurat5", quiet = TRUE)'
    R --no-echo -e 'remotes::install_github("satijalab/seurat-wrappers", "seurat5", quiet = TRUE)'
    R --no-echo -e 'remotes::install_github("stuart-lab/signac", "seurat5", quiet = TRUE)'
    R --no-echo -e 'install.packages("scCustomize")'
    R --no-echo -e 'install.packages("readxl")'

    apt clean
```
