

### Samtools

Samtools is an excellent package for manipulating `.sam` and `.bam' files. SAM stands for sequence
alignment/map format and a `.sam` file contains read information from a fastq file along with the 
alignment cordinates for each read that was added during the read alignment. A detailed description 
of the `.sam` file format can be found [here](http://samtools.github.io/hts-specs/SAMv1.pdf).

However, `.sam` files are very large and are in tab-delimited format, not an ideal format for 
manipulating programmatically. We therefore convert `.sam` files into binary format (i.e. from text
to 1s and 0s) which makes it easier for the computer to process. For this we use `samtools view`.

    for file in ls `~/sam_files`
    do
        prefix=$(basename ${sample} ".sam")
        echo ${prefix}
  
        samtools view -b ${file} > ${prefix}.bam
    done

The `-b` parameter specifies taht we want the output to be in `.bam` format. As the default output 
is printed to standard output (i.e. just printed to the screen) we write this to a file using `>`
followed by the name of the output file.  

### Sorting and indexing `.bam` files

Many of the packages we use downstream require the `.bam` files to be sorted and indexed to speed up
processing. We also use samtools for this. We can write a script containing use two simple `for` loops 
for this.

    echo "===Sorting and indexing bam_files with samtools==="
    echo "Started sorting bam files: " 
    /bin/date
    echo " "

    for bam in *.bam
    do
        name=$(echo $bam | awk -F"/" '{print $NF}' | awk -F".bam" '{print $1}')
        echo "Sorting: "$bam
        samtools sort $bam -o ${name}.srtd.bam
    done

    echo "Bam files sorted: " 
    /bin/date
    echo " "

    echo "Started indexing sorted bam files: " 
    /bin/date

    for bam in *.srtd.bam
    do
        name=$(echo $bam | awk -F"/" '{print $NF}' | awk -F".bam" '{print $1}')
        echo "Indexing: "$bam
        samtools index $bam ${name}.bam.bai
    done 
    
    echo "Sorted bam files indexed: " 
    /bin/date
    echo " "

    echo "Sorting and Indexing DONE: " 
    /bin/date
    echo "=================================================="

Essentially all we need here are the two `for` loops. The additional `echo` statements are there to
introduce basic scripting and to demonstrate how you can control the output of your script to tell
you where you are in the process. The `bin/data` statements print the time and date to the screen and
gives you an idea of how long each process takes. This is useful for memory assignment on the cluster,
as you can assign more memory to memroy intensive processeses.

Also notice that as we move through each process we alter the suffix of each file slightly. After 
sorting we add `.srtd.` to the new files and after indexing the sorted `.bam` files we add `.bai`. 
It is important to control you file names in this manner, so you know at a glance how you files have 
been altered. 

Eventually you will want to remove superfluous files to save space but this method retains all files.    