---
layout: page
title: Snakemake introduction
description: What is snakemake and workflow management?
---

### What is snakemake?

Snakemake is a workflow management and job scheduling tool which allows you to produce reproducible and 
scaleable bioinformatics workflows / pipelines. Workflow management tools allow you to automate multi-step
bioinformatics analyses (e.g. data collection, QC, processing and visualisation) and organise complex pipelines 
in a human readable manner. 

### Why should I use snakemake?

- Automation: Allows you to automate mundane steps of bioinformatics process to focus more on data analysis
- Reproducibility: Promotes scientifc reproducibility 
- Error tracking: Errors are easier to track reducing effort required to find and correct them
- Portability: Rule based system for subprocess organisation makes it easy to reuse / adapt workflow across projects / systems
- Readability: Snakemake uses python
- File creation is tracked: 
    - By completion: If a process crashes unexpectedly snakemake will automatically delete corrupted / incomplete files
    - By date: so if you change a file at stage 2 of the process snakemake will infom you to rerun all subsequent processes
- Makes parellisation easy
- Allows you to easily assign specific resources to each subprocess 

***

### Links

- This is all covered in detail here:
    - Wilson, et al. (2017). Good enough practices in scientific computing. [PLoS Computational Biology, 13(6), e1005510](http://doi.org/10.1371/journal.pcbi.1005510)

