---
layout: page
title: SRA to Bed file tutorial
tagline: With a few additional extras
description: Basic walkthrough for processing sequencing data
---

This tutorial is aimed at the novice bioinformatician and is  intended to demonstrate how to download publicly 
available ChIP-sea and ATAC-seq SRA files from the GEO repository and process it to produce *bam files and 
*bed files* for downstream analyses.

Whilst there are more efficent tools available to automate the processes described here, I think it is important 
to run each step individually when starting out to get a better handle what is going on and to understand how 
and why we modify the various file formats at each stage.

***

## Preparation 

In order to follow this tutorial you will need to have basic knowledge of the following;

- Linux
- Installing packages
- How to use different versions of python

Background

- [Linux 101]({{ site.baseurl }}/pages/linux101.html)
- [Scope]({{ site.baseurl }}/pages/scope.html)
- [Making scripts executable}({{ site.baseurl }}/pages/make_script_executable.html) 
- [Virtual Environments]{{  site.baseurl }}/pages/virtual_environments.html)
- [Installing required packages]({{ site.baseurl }}/pages/installing_required_packages.html)

Walkthrough

- [Getting started]({{ site.baseurl }}/pages/getting_started.html)
- [Get SRA file information]({{ site.baseurl }}/pages/get_sra_info.html)
- [Download SRA files]({{ site.baseurl }}/pages/download_SRA_files.html)
- [SRA to fastq]({{ site.baseurl }}/pages/SRA_2_fastq.html)
- [Quality control - FastQC/MultiQC]({{ site.baseurl }}/pages/fastqc.html)
- [Trim adapters]({{ site.baseurl }}/pages/trim_adapters.html)
- [Alignment]({{ site.baseurl }}/pages/alignment.html)
- [Converting sam files to bam files]({{ site.baseurl }}/pages/sam2bam.html)
- [Merging bam files]({{ site.baseurl }}/pages/bam_merging.html)
- [Calling peaks]({{ site.baseurl }}/pages/calling_peaks.html)
- [Removing blacklisted regions]({{ site.baseurl }}/pages/remove_blacklist_regions.html)
- [Merging bed files]({{ site.baseurl }}/pages/bed_merging.html)
- [Additional tools]({{ site.baseurl }}/pages/extra_tools.html)
- [Useful Links]({{ site.baseurl }}/pages/useful_links.html)

***

Thanks to:

+ [Heath O'Brien](https://github.com/hobrien) for help with content and for his [tutorials](https://hobrien.github.io/RNAseqTools/)
 introducing me to many of the concepts here.
+ Karl Broman for the excellent tutorial on how to 
[build a website using GitHub pages](https://github.com/kbroman/simple_site).

***

The source for this tutorial is [on github](https://github.com/Dazcam/SRA-to-Peak).

