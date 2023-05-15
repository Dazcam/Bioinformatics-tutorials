---
layout: page
title: Run basic snakemake process
description: Run basic snakemake process
---

# 4 basic files

To run Snakemake you need 4 basic file types:

- Snakefile
- Shell script
- Config file
- Cluster config file

***

# Create dummy fastq files

First navigate to the `/scratch/$whoami/snakemake-tutorial/workflow/` directory and create
some dummy fastq files.

```bash
mkdir -p ../resources/fq
for i in {1..5}; do echo "cortex_ExN_$i" > ../resources/fq/cortex_ExN_$i.fastq ; done
```

Which should create the following:

```bash
../resources/fq/
├── cortex_ExN_1.fastq
├── cortex_ExN_2.fastq
├── cortex_ExN_3.fastq
├── cortex_ExN_4.fastq
└── cortex_ExN_5.fastq
```




***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.md), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
