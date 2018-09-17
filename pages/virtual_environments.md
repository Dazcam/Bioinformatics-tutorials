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

### Setting up a virtual environment





***

Move on to [Installing required packages]({{ site.baseurl }}/pages/installing_required_packages.html),
or back to [Getting Started]({{ site.baseurl }}/pages/getting_started.html)

