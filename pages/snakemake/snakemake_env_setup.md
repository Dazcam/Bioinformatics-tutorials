---
layout: page
title: Snakemake environment setup
description: Set up snakemake environment?
---

The following script sets up up a virtual environment called `snakemake_tutorial` in your scratch area and installs snakemake 
and it's dependencies. As several dependency packages are downloaded to run snakemake, Mamba is downloaded first and used
to parallelise the process. All packages and and env info is stored in you scratch directory rather than home 
to avoid hitting the strict file number limits in the home directory. 

<script src="https://gist.github.com/Dazcam/73cea735c4a4c6aade161b5153d8bdfd"></script>

***

Next we need to set up the directory tree for this project. Keep this seprate from the snakemake env
directory. The following code sets up a directory tree that follows the . Save this code to
a file called `repo_setup.sh`, then run `./repo_setup /scratch/$whoami/snakemake-tutorial`.

<script src="https://gist.github.com/Dazcam/6284597ad17f4da278f948893007b731.js"></script>

***

Move on to [run basic snakemake process]({{ site.baseurl }}/pages/snakemake/run_basic_process.md), or back 
to [snakemake introduction]({{ site.baseurl }}/snakemake_intro.html).
