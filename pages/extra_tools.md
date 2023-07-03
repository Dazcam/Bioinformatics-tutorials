---
layout: page
title: Additional tools 
description: Recommended additional tools for bioinformatics 
---

Once you are up	to speed with the content of this tutorial there are certain tools you may want to
consider learning how to use. I use these tools pretty much every day now. 

***

### Session management - [tmux](https://github.com/tmux/tmux/wiki)

Often you will be coding for multiple projects at once. Whilst testing code you often want to run 
multiple instances of the shell terminal at once. For example, you may be testing some packages in
R in one terminal whilst writing a bash script in another. 

Packages such as `tmux` are excellent as they allow you to quickly change between environments and 
ensure exported varaibles and your bash history are distinct between sessions. 

Perhaps most importantly tmux allows you to return to sessions in your home folder even after you 
have disconnected as it can keep multiple sessions running in the background. 

+ [Tmux crash course](https://robots.thoughtbot.com/a-tmux-crash-course)
+ [Tmux shortcuts](https://gist.github.com/MohamedAlaa/2961058)

### Screen - alternative to tmux

Gotch with screen 

Be careful when using the same screen for long periods. For convienince purposes it's 
easy to just use the same screen for all snakemake purposes. However, bear in mind that
the screen is a snapshot of your global environment at the time you first created it.

So if, for example, you alter your `.myenv` or `.bashprofile` file, to change the 
default version of R you are using this will not be altered in historical global
environments.

***

### Version control - [GitHub](https://github.com) 

Without doubt GitHub is the resource I use most day to day. It is a version control repository. 

Version control is a system that allows you to keep a record of the changes you make to something
whilst keeping all the different versions of the document that you have created so you can easily 
return to a previous version of the document at any time. This is a fundamental resource for 
script writing and software development. 
[Git](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) is the main version
control system I use.

[GitHub](https://github.com) is the main online repository I use for version control. It provides
and online space for you to manage your content but it is easy to clone the online repository to 
any local machine if you'd rather work via the command line. 

I use GitHub for script writing, but also as an online lab book to stroe useful links and 
information I'd like to return to. 

I built this website using GitHub pages! No prior css knowledge required. 

Although GitHub's ethos is making information public, it is possible to keep certain repositories 
private if you are a student (or using GitLab).  

***

### Workflow management - [Snakemake](https://snakemake.readthedocs.io/en/stable/))

It is important to strive to automate your workflow as much as you can. By writing bash code you 
will quite quickly produce very long scripts which are cumbersome and can be difficult to 
read.

Snakeamake is a powerful package for efficent job scheduling on a cluster. Normally running a bash
script on a cluster works serially, the job at the top of the script is run first, then the second
and so on. If you write your script using snakemake the software quickly scans the entire script 
and separates every job. So for example, in the previous tutorial we used a for loop to run through 
a batch of files in pretty much every stage of the process. During alignment, files were passed one 
at a time to Bowtie2 and only after all files were aligned did we move on to the next process. 

Using Snakemake we don't need for loops. Each call to Bowtie2 is treated as a separate job. This means
that, in theory, all files can be run through alignment step simultaneously and as soon as one file has 
been aligned a new job is triggered for it to be changed from a sam to a bam file. There is no need to 
wait for everything to be alinged. This is parallelisation over and above that already discussed so
it is easy to see how running time can reduce rapidly.

Snakemake code is also far more readable and concise and it was designed with bioinformatics in mind
so there are online [code wrappers](https://snakemake-wrappers.readthedocs.io/en/stable/) for all of 
the processes I mentioned here. 

***

Heath O'brien, who introduced me to all of these resources has a 
[great site](https://hobrien.github.io/RNAseqTools/) which discusses the above in more detail.    

***

Move on to [Useful Links]({{ site.baseurl }}/pages/useful_links.html),
or back to [Merging bed files]({{ site.baseurl }}/pages/bed_merging.html).
