---
layout: page
title: Additional options
description: Additional options
---

#### Accessing wildcards in `params`

In order to access a wildcard in the params directive you have to write a small lambda function:

```python
params: region=lambda wc: wc.get("region")
```

Using {wildcards.region} does not work here (why?). It may seem unecessary to add a wildcard 
as a parameter, if you can acesss the wildcard using either {region} or {wildcards.region} in
snakemake directive and shell scripts respectively.

However, this comes in handy when trying to access wildcards within R scripts which are run
using the `script:` directive. The above could be accessed via:

```r
snakemake@params['region']
```

***

Move on to [snakemake environment setup]({{ site.baseurl }}/pages/snakemake/02_snakemake_env_setup.html), 
or back to [Run basic process]({{ site.baseurl }}/index.html).

***

#### Logging within R scripts

When using the `script:` directive to run an R script, it is not permitted to add {log} after
calling the script. This means you have to find an alternative method redirect `stdout` etc.

A neat method is to write a small function in R to capture information you wish to log:

```r
source: https://github.com/kelly-sovacool/snakemake-Rscript-log-mwe/blob/master/script.R
log_smk <- function() {
  if (exists("snakemake") & length(snakemake@log) != 0) {
    log <- file(snakemake@log[1][[1]], open = "wt")
    sink(log, append = TRUE)
    sink(log, append = TRUE, type = "message")
  }
}
```

Note that when an R script is run by snakmake the variable `snakemake` is available in the 
local R environment. We can use this to create a log file only if the script is being run 
by snakemake. The snakemake variable can be useful in scripts designed to be run 
on local and remote machines. Note that the square brackets after `snakemake@log` are 
must be added here.

***

#### Methods for reading in data

When running a snakemake pipeline, you will inevitably need to manage large number of files 
that often have long complex file names. There are several methods that can be used to read
files in.

**Using a dictionary**

[source](https://www.biostars.org/p/9588877/#9589491)

```r
transform = {"new_name1": "sample1", "new_name2": "sample2"}

rule example:
    input:
        i = lambda wc: "path/%s.vcf" % transform[wc.newname],
    output:
        o = "path/{newname}.vcf",
```
