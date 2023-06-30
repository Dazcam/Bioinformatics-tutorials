## Snakemake common errors

Common errors:

Usually a bracket or quotes are not open / closed properly.

```python
SyntaxError in line 12 of /scratch/c.c1477909/Herring_snRNAseq_2023/workflow/Snakefile:
EOL while scanning string literal
```

You need to make sure that your wildcards are identical in the output, log and benchmark
directives of a single rule.

```python
SyntaxError:
Not all output, log and benchmark files of rule rm_MHC_from_ref contain the same wildcards.
```
