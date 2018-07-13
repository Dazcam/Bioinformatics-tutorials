
There are several scenarios within a ChIP-seq or an ATAC-seq pipeline where you may have to merge files
to pool information between repliplcates. 

### Merging `.bam` files using Samtools - ChIP-seq

When anaylsing a ChIP-seq experiment we will have individual `.bam` files for each of our technical replicates for
each ChIP modification we're looking at (i.e. H3K4Me1, H3K27ac, SPI1 etc.) as well as our INPUT control replicates.

In order to call peaks appropriately on our data we need to pool the information in our `.bam` files for all our 
replicates. So if we have the following:

+ H3K4Me1 - 3 replicates
+ H3K27ac - 3 replicates
+ SPI1    - 3 replicates
+ INPUT   - 5 replicates

We could use samtools merge to create a pooled bam file for each condition.

~~~bash
samtools merge  H3K4Me1_RepA.bam H3K4Me1_RepB.bam H3K4Me1_RepC.bam > H3K4Me1_Mrgd.bam
~~

However, this method creates 4 additional large files. If storage is not an issue then this metod is fine, and you 
take your merged files on for peak calling. Otherwise there is a workaround in Macs2 that allows you to specify 
multiple input files to the `treatment` and `control` parameters. 

***

### MACS2 workaround for merging `.bam` files - ChIP-seq

To save storage space and circumvent the need to merge `.bam` files manually, you can just pass all the technical 
replicates for a particular condition directly yo MACS2 and it will merge them for you as part of the peak calling 
process.

~~~bash

~~~
 
