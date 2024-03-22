

The documentation for uploading fastq files to EGA is a bit convoluted. Below are details
on how to do it quickly.

***

#### Prepare the CSV file

You need to upload a `.csv` to let the EGA server know what files to expect. It's 
possible to metadata columns to the `.csv` file, but in terms of
required fields you need is the sample ID, the names of all of th following:

+ the encrytped fastq files (.md5)
+ the encrytped checksum files (.gpg.md5)
+ the unencrypted checksum files (.gpg)

There is shell script below to generate this file that takes as input:

A: A directory name where the fastqs are stored
B: A set of sample IDs for the fastqs

It will generate and upload file called `ega_upload.csv`. Note that the
code will need to be tweaked for your own fastq files.


<details>

<summary>Shell script to create upload csv file </summary>

```bash
# 
fq_dir=/scratch/c.c1477909/fetal_brain_snATACseq_V3_010323/resources/fq/
declare -a arr=("14510_WGE_ATAC" "14611_WGE_ATAC" "14993_WGE_ATAC")

for sample in "${arr[@]}"; do

    ls -1 ${fq_dir}${sample}*R{1,2}* | cut -d/ -f 7 >> tmp.csv

done

cat tmp.csv | cut -d_ -f 1-5 | sed 's/W//' | uniq >> tmp2.csv

echo 'Sample alias,First Fastq File,First Checksum,First Unencrypted checksum,Second Fastq File,Second Checksum,Second Unencrypted checksum' > ega_upload.csv


while read line; do

  paste -d, \
  <(printf '%s' "$line" | cut -d_ -f 1-2 | cut -d4 -f2) \
  <(printf '%s' "${line}_R1_001.fastq.gz.md5") \
  <(printf '%s' "${line}_R1_001.fastq.gz.gpg.md5") \
  <(printf '%s' "${line}_R1_001.fastq.gz.gpg") \
  <(printf '%s' "${line}_R2_001.fastq.gz.md5") \
  <(printf '%s' "${line}_R2_001.fastq.gz.gpg.md5") \
  <(printf '%s' "${line}_R2_001.fastq.gz.gpg") >> ega_upload.csv
  
done <tmp2.csv

rm tmp.csv tmp2.csv
```

Note that 

</details>
