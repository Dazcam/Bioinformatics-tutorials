---
layout: page
title: Required packages
description: Required packages for high throughput sequencing walkthrough
---

### Installing software

There are three main ways to install software for bioinformatics.

+ Dowloading it from source and compiling it manually
+ Dowloading pre-compiled software
+ Using a package manager to install sotware



When loading packages from source you are downloading the code (usually in an advanced language such as C or C++) 
required to build the package in question, along with any associated files and dependencies. Commands such as make, 
build, install are used to do this. There is some flexibility in using this install method as you can specify 
where you want files to be stored, but often the program doesn’t run properly for reasons that the trainee 
programmer could waste days trying to remedy, so this option for loading packages is best avoided.

When a package is installed the original C code is translated into binary format (1s and 0s), which is the language 
the computer understands and interprets. All scripts are interpreted in binary format. A more efficient way to 
download packages is to download pre-compiled binary files. Here, the make requirement is skipped as it has 
effectively already been done and the binaries are downloaded directly. Although repositories such as CRAN 
provide both options, using pre-compiled binaries is often the best option as there are usually fewer problems.

Package managers are a 3rd option to install software. Package managers such as Macports, Homebrew and Conda 
are all versions of package managers. The benefit of package managers is that they organise where files are 
stored on your system for you and makes sure that all packages/dependencies/scripts etc. are stored in the same 
place. Loading packages individually can often mean files are strewn all over your system, so this is neat way 
to avoid that happening. However, a big drawback of package managers is that some packages are not compatible 
with all package managers. So, if you have decided to use homebrew as your package manager but a package that you 
want to download is only available via Macports, downloading Macports can lead to clashes between the package 
mangers. For example, often competing package managers will store files in different locations, or try to reset 
default folders for previously downloaded files which can corrupt operations that previously ran smoothly. If 
you decide to use the package manager method the best thing to do is to stick to one, but bear in mind that 
it is unlikely that an individual package manager will have all the packages you want to use available in its 
package list, so you may still need to manually install some packages. 

Heath recommended downloading pre-compiled binaries whenever possible and storing them in a location of your choosing. 




* SRA files
  * [SRA toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std))
    * prefetch 
    * fastq_dump 
  * [parallel-fastq-dump](https://github.com/rvalieris/parallel-fastq-dump)

* Quality control
  * fastqc
  * multiqc

* Alignment
  * [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) 

* `.sam` file to `.bam` file, and sorting and indexing `.bam` files
  * [Samtools](http://samtools.sourceforge.net)

* Peak calling
  * [Macs2](https://github.com/taoliu/MACS)

* Remove blacklisted regions, merge peaks, extend peaks
  * [Bedtools](http://bedtools.readthedocs.io/en/latest/)

* Remove mitochodrial reads
  * [Bedops](http://bedops.readthedocs.io/en/latest/)

* Motif analysis
  * [Homer](http://homer.ucsd.edu/homer/)

Move onto [Get SRA files](pages/get_sra_info.html).
