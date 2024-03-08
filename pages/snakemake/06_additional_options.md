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

Move on to [snakemake environment setup]({{ site.baseurl }}/pages/snakemake/02_snakemake_env_setup.html), or back to [Run basic process]({{ site.baseurl }}/index.html).
