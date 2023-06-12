---
layout: page
title: SSH keys
description: Version control with Git inroduction
---

If you are going to be pushing changes to GitHub from the command line often, it's a good idea 
to create an SSH (secure shell) key. SSH keys are a pair of public and private keys that are 
used to authenticate and establish an encrypted communication channel between a client and 
a remote machine over the internet. This useful for data security but also let you circumvent 
the necessity of typing in a password each time you want to push or pull data from a remote
repository.



    - See [here]({{ site.baseurl }}/WorkflowTutorial/SystemConfig#avoiding-passwords) to see how to generate an ssh key
    - On GitHub, go to ```Settings``` and click on ```SSH and GPG keys```
    - Type in a descriptive name for your computer and paste in your public key
    - You'll also need to set your git user name:
        - ```git config --global user.name USERNAME```
