---
layout: page
title: Linux 101
description: Some useful linux tips for bioinformatics
---

### Renaming groups of files

~~~bash
while read -r old new; do
   echo mv "$old" "$new"
done < <(paste listA listB)
~~~

```bash
### Subsetting fastq files for testing

# read the compressed forward fasq file, 
# piping the result in sed to extract the 
# first 4 million lines (equivalent to 1M sequences, 
# as one sequence entry always span 4 lines)
# and finally compressing it as a new file
zcat pair_1.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_1.fastq.gz
# same for read 2 (the reverse read) if your data is Paired-End
zcat pair_2.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_2.fastq.gz
```
~~~ bash
while read -r old new; do
   echo mv "$old" "$new"
done < <(paste listA listB)
~~~
