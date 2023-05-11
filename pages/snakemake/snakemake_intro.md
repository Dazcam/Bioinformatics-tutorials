---
layout: page
title: Snakemake introduction
description: What is snakemake and workflow management?
---

### What is snakemake?

![snakemake overview](https://snakemake.readthedocs.io/en/stable/_images/idea.png)

Snakemake is a workflow management and job scheduling tool which allows you to produce reproducible and 
scaleable bioinformatics workflows / pipelines. Workflow management tools allow you to automate multi-step
bioinformatics analyses (e.g. data collection, QC, processing and visualisation) and organise complex pipelines 
in a human readable manner. 

### Why should I use snakemake?

- **Automation**: Allows you to automate mundane steps of bioinformatics process to focus more on data analysis
- **Reproducibility**: Promotes scientifc reproducibility 
- **Error tracking**: Errors are easier to track reducing effort required to find and correct them
- **Portability**: Rule based system for subprocess organisation makes it easy to reuse / adapt workflow across projects / systems
- **Readability**: Snakemake uses python
- **File creation is tracked**: 
    - By completion: If a process crashes unexpectedly snakemake will automatically delete corrupted / incomplete files
    - By date: so if you change a file at stage 2 of the process snakemake will infom you to rerun all subsequent processes

***

### How does snakemake work?

- Used to compile [source code](https://github.com/alexdobin/STAR/tree/master/source) into binary
- Developed at a time when compilation was extremely resource-intensive
- Allows "nightly builds" where only modified code is re-compiled
- Builds a dependency tree from implicit wildcard rules
- Can be used to develop [bioinformatics pipelines](https://swcarpentry.github.io/make-novice)
- However: 
    - limited functionality and flexibility
    - perl-level syntax opacity
    - doesn't support parallelization
 
***

### Benefits of using snakemake on a computer cluster

- **Makes parellisation simple**: Manages scheduling of job submission to [cluster](http://snakemake.readthedocs.io/en/stable/executable.html#cluster-execution) (or to the [cloud](http://snakemake.readthedocs.io/en/stable/executable.html#cloud-support))
- **Allows you to easily assign specific resources to each subprocess**: pe smp and h_vmem can be specified as params in the Snakefile, in a [cluster config file](http://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html#cluster-configuration) or ...
    - cluster config files allow specification of default parameters
- Can be used to execute shell commands or python code blocks (in theory also [R code blocks](http://snakemake.readthedocs.io/en/stable/snakefiles/utils.html#scripting-with-r))   
- Inputs, outputs, and [parameters](http://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-4-rule-parameters) can be specified for each rule
- Need to include a "master rule" (usually called ```all```) which requires all of your desired outputs as input
- Intermediate files can be [automatically removed](http://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-6-temporary-and-protected-files) once they are no longer needed
- Supports [benchmarking](http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#benchmarking) to report CPU and memory usage and [Logging](http://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-5-logging) of messages/errors
- Supports [config files](http://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html) to abstract details of pipeline from inputs and outputs
    - [Input functions](https://snakemake.readthedocs.io/en/stable/tutorial/advanced.html#step-3-input-functions) allow config file entries to be accessed by wildcard values
- Workflows can also be further abstracted by:
    - using ```include``` statements to import [python code](http://snakemake.readthedocs.io/en/stable/project_info/faq.html#i-want-to-import-some-helper-functions-from-another-python-file-is-that-possible)
    - using the ```script``` command to [execute a python script](http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#using-custom-scripts), giving it access to variables defined in the Snakefile
    - using ```include``` statements to import rules from [other Snakefiles](http://snakemake.readthedocs.io/en/stable/snakefiles/modularization.html#includes)
    - creating [sub-workflows](http://snakemake.readthedocs.io/en/stable/snakefiles/modularization.html#sub-workflows)
- [Conda environments](http://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html#integrated-package-management) can automatically be set up for each step of the analysis
- Many popular tools have [prewritten wrappers](https://snakemake-wrappers.readthedocs.io/en/stable) that automatically create the necessary environment and run the tools using the specified inputs, outputs, and paramaters
- There is also a [repository](https://bitbucket.org/johanneskoester/snakemake-workflows) of example rules and workflows for NGS analyses

### Getting started with Snakemake
- Snakemake [documentation](https://snakemake.readthedocs.io/en/stable) and [tutorial](https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html)
- Examples of:
    - a [Snakefile](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/Snakefile)
    - including [additional Snakefiles](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/bamQC)
    - a [config file](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/config.yaml)
    - [cluster configuration](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/cluster_config.yaml)
    - a [bash script](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/snakemake.sh) for invoking snakemake on the cluster, including email notification upon completion

### Snakemake usage
- Do a dry run of workflow, printing commands to screen:
    - ```snakemake -np```
- Produce a diagram of dependency tree:
    - ```snakemake -n --dag | dot -Tsvg > dag.svg```

![dag](https://github.com/hobrien/RNAseqTools/blob/master/Benchmarking/dag.png?raw=true)

- Rerun rule (and all rules with it as a dependency):
    - ```snakemake -R RULENAME```
- Rerun on new input files:
    - ```snakemake -n -R `snakemake --list-input-changes` ```
- Rerun edited rules:
    - ```$ snakemake -n -R `snakemake --list-params-changes` ```
- Submit jobs to cluster:
    - ```snakemake --use-conda --cluster-config cluster_config.yaml --cluster "qsub -pe smp {cluster.num_cores} -l h_vmem={cluster.maxvmem}" -j 20```

- See [here](http://snakemake.readthedocs.io/en/stable/api_reference/snakemake.html) for additional command-line options

### Alternatives to Snakemake

### Links

- This is all covered in detail here:
    - Wilson, et al. (2017). Good enough practices in scientific computing. [PLoS Computational Biology, 13(6), e1005510](http://doi.org/10.1371/journal.pcbi.1005510)

