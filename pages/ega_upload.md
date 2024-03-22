---
layout: page
title: Uploading files to the EGA repository
description: Uploading files to the EGA repository
---



The documentation for uploading fastq files to EGA is a bit convoluted. Below are details
on how to do it quickly.

***

#### Prepare fastq files and upload document

All fastqs for upload to EGA repo need to be encrytpted. EGA suggests a couple of 
methods for encryption and file upload. I'd recommend using the JAVA
application [`EGACryptor`](https://ega-archive.org/submission/data/file-preparation/egacryptor/#DownloadClient)
for file encryption and to upload the files via [ftp](https://ega-archive.org/submission/data/uploading-files/ftp/). 
The shell script below will download the application and encript the fastq files. 
It also creates a `.csv` file that informs the EGA server of what files to expect. 
The required fields for each set of paired end fastq files in the `.csv` are:

- sample ID
- encrytped fastq files (.md5)
- encrytped checksum files (.gpg.md5)
- unencrypted checksum files (.gpg)

These files are produced after running `EGACryptor`. The shell script below takes the following inputs:

- A directory where the fastqs are stored
- A file containing a list sample IDs for the fastqs, one ID per line

A set of encrypted files will be produced for each paired set of fastq files.
An upload document called `ega_upload.csv` will also be produced. Note that the script
is designed to work with fastq files with the following naming structure:

- fq: `14993_WGE_ATAC_S5_L002_R2_001.fastq.gz`
- sample id: `14993_WGE_ATAC`

For brevity, the script alters the sample IDs given to input 2 to `993_GE` for 
population of column 1 of the `.csv` file. The script will have to be altered for
fastq files that do not follow this naming structure.

<details>

<summary>Shell script to create upload csv file </summary>

```bash
# Preps fq files and creates csv for EGA repository upload
# Loads java as module on Hawk
# Downloads file encryption application, runs, and then rms it

# Set variables
fq_dir=$1 
mapfile -t arr < $2
mkdir encrypted_files

# Load java
module load java

# Download EGA cryptor 2.0.0 software
wget https://ega-archive.org/assets/files/EgaCryptor.zip
unzip EgaCryptor.zip  
mv EGA-Cryptor-2.0.0/* .
rm -rf EGA-Cryptor-2.0.0 EgaCryptor.zip 

# Get a lst of fq filenames
for sample in "${arr[@]}"; do

    ls -1 ${fq_dir}${sample}*R{1,2}* | cut -d/ -f 7 >> tmp.csv

done

# Get a lst of sample IDs
cat tmp.csv | cut -d_ -f 1-5 | sed 's/W//' | uniq >> tmp2.csv

# Add header to upload file
echo 'Sample alias,First Fastq File,First Checksum,First Unencrypted checksum,Second Fastq File,Second Checksum,Second Unencrypted checksum' > ega_upload.csv

# Populate csv
cat tmp2.csv | while read line; do

  paste -d, \
  <(printf '%s' "$line" | cut -d_ -f 1-2 | cut -d4 -f2) \
  <(printf '%s' "${line}_R1_001.fastq.gz.md5") \
  <(printf '%s' "${line}_R1_001.fastq.gz.gpg.md5") \
  <(printf '%s' "${line}_R1_001.fastq.gz.gpg") \
  <(printf '%s' "${line}_R2_001.fastq.gz.md5") \
  <(printf '%s' "${line}_R2_001.fastq.gz.gpg.md5") \
  <(printf '%s' "${line}_R2_001.fastq.gz.gpg") >> ega_upload.csv
  
done

# Run encryption - option to parallelise - but fairly quick and not
# resouce intesive for < 50 files when ran in bkgrnd
for sample in "${arr[@]}"; do

    for rep in `ls -1 ${fq_dir}${sample}*R{1,2}*`; do

    java -jar ega-cryptor-2.0.0.jar -i ${rep} -o encrypted_files

    done

done

# Clean
rm tmp.csv tmp2.csv ega-cryptor-2.0.0.jar
```

***

</details>
