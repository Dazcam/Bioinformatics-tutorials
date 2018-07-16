---
layout: page
title: SRA to Bed file tutorial
tagline: With a few additional extras
description: Basic walkthrough for processing sequencing data
---

This tutorial is intended to show you how to download publicaly available ChIP-sea and ATAC-seq SRA files from the GEO repository 
and process them to produce .bam and .bed files for downstream analyses.

Whilst there are more efficent tools available to automate the processes described here, I think it is important to run each step 
individually when starting out to get a better handle what is going on and to understand how and why we modify the various file 
formats at each stage.

## Preparation 

In order to follow this tutorial you will need to have basic knowledge of the following;

- Linux
- Installing packages
- How to use different versions of python

Links other pages.

- [Linux 101](pages/linux101.html)
- [Required packages](pages/required_packages.html)
- [Getting started](pages/getting_started.html)
- [Get SRA file information](pages/get_sra_info.html)
- [Download SRA files](pages/download_SRA_files.html)
- [SRA to fastq](pages/SRA_2_fastq.html)
- [Quality control - FastQC/MultiQC](pages/fastqc.html)
- [Trim adapters](pages/trim_adapters.html)
- [Alignment](pages/alignment.html)
- [Sam2Bam](pages/sam2bam.html)
- [Merging `.bam` files](bam_merging.html)
- [Calling peaks](pages/calling_peaks.html)
- [Removing blacklisted regions](remove_blacklist_regions.html]
- [Merging `.bed` files](bed_merging.html)
- [Useful Links](pages/useful_links.html)
---

The source for this tutorial is [on github](https://github.com/Dazcam/SRA_to_Peak).

