---
layout: page
title: Virtual Environments
description: Using virtual environments in bioinformatics 
---

On this page we will download the package manager called *CONDA* and use it to create a virtual environment and to 
download the packages we need to run the tutorial. Although it was initially created for python packages it now
distributes packages across programming languages.

***

### Conda distributions

There are a few different conda distributions to be aware of.

+ Anaconda - conda + 150 most common python packages
+ Miniconda - just conda, packages need to be installed separately
+ Bioconda - bioinformatics specific packages
+ Source forge - python packages uploaded by anayone

We will download miniconda and install the other packages we need individually. Navigate 
[here](https://conda.io/miniconda.html) and download the latest version of conda relevant
to your system. 

Choose the lastest **Python 3** version of conda. We will need to use both python 2 and 3 during the walkthrough
but we can alter this easily regardless of the choice of distribution you opt for. 

Once conda is installed check which version you have by typing `conda -V`.

***

![conda]({{ site.baseurl }}/assets/conda.png)

***

### Python versions

It’s important to note that different versions of particular language exist and some programs may only work properly 
within a particular version of a language. As many bioinformatics scripts are written in python, older 
packages can be python 2 specific; python 2 is syntactically distinct from python 3. This means that if we want 
to use a program written in python 2 and our default python version is python 3 we need to chnage this explicitly by
altering the default in you environment or calling python 2 in the shebang of your script.

As we downloaded miniconda 3, our default python environment is python 3. We can check this by typing `python -V`.   

***

![pyVersion]({{ site.baseurl }}/assets/pyVersion.png)

***

We can see we are using python 3.6.4 via anaconda. Notice that typing **`which python`** shows us the location of the 
executable for python 3 in the miniconda bin directory that we just downloaded. 

***

### Setting up a virtual environment

In the scope section, we discussed assigning variables in local and global environments. When we create a virtual 
environment we are creating an isolated environment for our project of interest. This allows us to download 
packages and control the conditions of that environment independently of the global environment of the system we are 
working on, but crucially, to use the same environmental conditions on different machines/clusters. 

Another advantage of using virtual environments is that downloading new packages into your home location on your 
computer cluster usually requires special super user privilages. Using virtual environments is a means to circumnavigate
these privilage issues.

***

#### Create the sra2Peak environment

First of all we create our virtual environment that uses python 3 as the default.

~~~bash
conda create --name sra2peak
~~~

Take a note of where the environment has been created after the **environment located section** something like

~~~bash
/Users/darren/Programs/miniconda3/envs/sra2peak
~~~

Then we activate that environment.

~~~bash 
source activate sra2peak
~~~

Next we need to load the packages we want to use into the environment. Most are available through bioconda, which is 
the bioinformatics specific conda repository. Any dependencies these packages need will also be installed.

~~~bash
conda install -c bioconda sra-tools fastqc multiqc bowtie2 samtools bedtools bedops homer
~~~   

This will generate a list of all the packages and dependencies that will be installed. Type **y** to proceed. 

Now most of the packages you need for this walkthrough are installed in the **sra2peak** virtual environment. However,
one package `macs2` is written in python 2 so we need to create a sceond virtual environment for that, so let's 
deactivate the **sra2peak** environment first.

~~~bash
source deactivate sra2peak
~~~

 
***

#### Create the macs2 environment

We create our macs2 environment in similar fashion however we specify the python version as 2.7 here. Typing **y** 
to proceed whenevr required.

~~~bash
conda create --name macs2 python=2.7
~~~

And then install the **macs2** package.

~~~bash
conda install -c bioconda macs2
~~~

You are now set to run all the operations in this walkthrough. 




Move on to [Getting Started]({{ site.baseurl }}/pages/getting_started.html),
or back to [Installing required packages]({{ site.baseurl }}/pages/installing_required_packages.html)

