---
layout: page
title: Linux 101
description: Some useful linux tips for bioinformatics
---

This section contains some basic linux information and tips for setting up an environment for bioinformatics,
but imo the bare essentials to get started withour prior programming knowledge. It contains elementary 
information so experienced users should skip this. 

This is by no means exaustive, and not the best resource available, it contains enough to get started and is
the 'obvious' stuff I wish I knew when I was starting out. 

### Open a terminal

To open a terminal on a mac press and hold *cmd* then press *space* to open spotlight. Type '*terminal*'.
This opens a window in your home folder. For instructions on how to do this on a windows machine see here.

The terminal is just an alternative way to access the filing system on your computer without using the graphical
user interface (GUI). So instead of seeing icons which represent folders and programs we just see lists. 
Whenever we open a terminal window for the first time we are located in the home directory of our computer. 

### Navigation

To carry out the tasks we need to via the command line we need to navigate between directories. To find out
where we are at any given time we type:

~~~
pwd
~~~

Which prints the current working directory to the screen. This is the address for our home directory. To list  
all the directories and files in our home folder we can type:

~~~bash
ls
~~~

We can see this directory contains twelve sub-directories, with a *`/`* after their name, and one file. Note
here the directories are in blue but this coulour scheme may be different, or non-existant, on different machines.

To move between folders we use the command `cd`.

~~~bash
cd Desktop
~~~

Takes us to the `Desktop` directory. Typing `ls` again lists the contents of the `Programs` directory.

To move back one file in the file hierarchy we can type:

~~~bash
cd ..
~~~

And at any time if we want to **return to the home directory** from anywhere in the files hierarchy we type:

~~~bash 
cd

# OR

cd ~/   
~~~

There are two options here we can just type `cd` or use the tilde which is command line shorthand for the 
home directory. 

***

### Relative and absolute paths

From the `Desktop` directory we can move forward using either relative or the absolute paths. The relative 
path requires you to type the adresss of your desination directory relative to where you currently are. So
to move to the `Homer` directory, withing the `Programs` directory you simply have to type:

~~~bash
cd /Programs/Homer
~~~

To do the same thing using the absolute path you would type:

~~~bash
cd /Users/Darren/Desktop/Programs
~~~  

Both methods are useful in different scenarios. 

***

### Autocompletion

A useful tool when using the command line is the autocomplete fuction. Rather than typing out long directory
addresses all the time we only need to type the first letter of any directoy in the current folder and press
the `tab` key to auto complete the name of that directory. This function is case senstive.

If you have multiple directories starting with the same letter you need to type enough letters until you can
uniquely identify the file of interest.

Note that you can autocomplete through multiple directories consecutively if you know the names of the 
directories you intend to move through in adavnce. If you need to see a list of the files in a particular 
directory that start with the same letter, press `tab` twice.

***

### Hidden files

When we type `ls` on the command line we are not presented with a list all the files in the current directory.
Most working directories contain a series of 'hidden' files that are there to run background processes for 
specific programs. They are not immediatly accessible as, generally, altering these files is not recommended.

We can view these files by adding the `-a` flag to the ls call.

~~~bash
ls -a
~~~

Note that there are several files that begin with `.`, these are the hidden files. The most important one for 
our purposes is the file called `.bash_profile`.  


### Renaming groups of files

It is important to rename all your SRA/fastq files at the start of a bioinformatics pipeline to something 
concise and memorable so that you can see at a glance what replicate/sample each file relates to. This 
naming structure will then be maintained through all the subsequent steps in the pipeline.

There are many ways to do this but a simple and easily understandable method is to create two text files, 
one called *SRA_numbers.txt* and the other called *SRA_names.txt*. We add the SRA numbers one per line to 
*SRA_numbers.txt*.

~~~bash
SRR11111111
SRR11111112
SRR11111113
SRR11111114
SRR11111115
~~~

Then add a memorable name, perhaps containing the lead author of the paper and some replicate information, 
for each SRA file to the *SRA_names.txt*, ensuring the positions in each text file for SRA numbers and 
corresponding new name match.

~~~bash
Sample1
Sample2
Sample3
Sample4
Sample5
~~~

We then use a simple `while` loop, along with the paste function, to read in both text files and pass the 
contents SRA_numbers.txt to the variable `old` and the contents SRA_names.txt to the varible new, one line 
at a time. The old and new variables are then passed to the `mv` function which rename the files accordingly.

~~~bash
while read -r old new; do
   echo mv "${old}.sra" "${new}.sra"
done < <(paste SRA_numbers.txt SRA_names.txt)
~~~

As the echo statement is included above instead of this loop actually renaming everything it just prints the 
change that would be made for each iteration of the loop. This is a great way to check if the entries in both
files correspond before actually running the loop properly.

mv SRR11111111 Sample1
mv SRR11111112 Sample2
mv SRR11111113 Sample3
mv SRR11111114 Sample4
mv SRR11111115 Sample5 

If we want to actually change the names of the files listed above we just remove the echo statement.

### Subsetting fastq files for testing

Often when debugging a script we want to subset our fastq files so that we can significantly reduce the 
processing time for every step in our script rather than run all the data through at once. 

~~~bash
# read the compressed forward fasq file, 
# piping the result in sed to extract the 
# first 4 million lines (equivalent to 1M sequences, 
# as one sequence entry always span 4 lines)
# and finally compressing it as a new file

zcat pair_1.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_1.fastq.gz

# same for read 2 (the reverse read) if your data is Paired-End
zcat pair_2.fastq.gz | sed -n 1,20000000p | gzip -c > pair-subset_2.fastq.gz
~~~

