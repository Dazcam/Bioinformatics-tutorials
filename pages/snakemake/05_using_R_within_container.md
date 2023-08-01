---
layout: page
title: Using R within a container
description: Using R within a container
---

Although there are several ways to run R scripts via snakemake, when trying to run
R scripts in a shared environment one can often encounter irresovable issues with 
package dependancies, see [add link](). There are several causes:

  1. **Base R packages are often stored in a public folder**: as you don't have permisssions
     to alter these files you need to create a local folder for any additional packges
     you want to install and if a package requires a more up-to-date version of a base
     package you can't upgrade it.
  2. **Multiple versions of same package accessible**: this is realted to 1. Often when
     an upgrade is critical, a work around is to download a 2nd, more recent version
     of the package in your local folder. This may work fine, but it can lead to
     confusing `segmentation faults`. I've messed around with changing the `.libPaths()`
     to ignore the default base packages but this always led to problems.
  3. **Several methods to install R packages**: You can install R packges manually, via
     conda (via several independent channels) or via GitHub. I've had issues downloading
     via CRAN, Bioconductor, Conda and particularly via GitHub. 
  5. **The version of R you are using may not be what it seems**: It can get confusing
     which version of R you are using. If you are using `modules` with snakemake you have
     explicity call the Rscript exe for the module. I've often encountered issues becuase
     the R version I thought I was using was in fact not the version running the script.

The best solution to avoid all this heartache is to use a standalone version of R
within a container that has all the major CRAN and bioconductor packages pre-installed.
Details on how to install a container configured for R are [here].

```R
localrules: rm_MHC_from_ref

## Remove MHC region genes from reference file
## Outputs two files: 
##  - the full ref file with MHC genes removed
##  - R object for next rule of above with fewer cols 

rule rm_MHC_from_ref:
    input:  "../resources/ref/NCBI37.3.gene.loc.txt"
    output: gene_coord = "../results/R_objects/NCBI37.3_gene_coords_noMHC.rds",
            mhc_genes = "../results/R_objects/mhc_genes.rds"
    singularity: "../resources/containers/tdespec_latest.sif"
    params: ref_noMHC = "../results/R_objects/NCBI37.3.MHCremoved.gene.loc.txt",
            R_lib = "/scratch/c.c1477909/R/library"  
    threads: 1
    log:    "../results/00LOG/prep_enrich_files/snRNAseq_rm_MHC_from_ref.log"
    script:
            "../scripts/snRNAseq_rm_MHC_from_ref.R"

rule prep_enrichment_files:
    input:  seurat_obj = "../results/R_objects/{project}.rds",
       	    gene_coord = "../results/R_objects/NCBI37.3_gene_coords_noMHC.rds",
       	    mhc_genes =	"../results/R_objects/mhc_genes.rds"
    output: "../results/gene_lists/prep_enrichment_file_{project}.done"
    singularity: "../resources/containers/tdespec_latest.sif"
    resources: tasks = 1, mem_mb = 240000
    params: ctd_outdir = "../results/R_objects/",
            outdir = "../results/gene_lists/",
            study_id = "{project}",
    threads: 20
    log:    "../results/00LOG/prep_enrich_files/snRNAseq_prep_enrichment_files_{project}.log"
    script:
            "../scripts/snRNAseq_prep_enrich_files_A.R"

```

