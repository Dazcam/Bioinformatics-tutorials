---
layout: page
title: Version control with Git
description: Version control with Git inroduction
---

### [Working with branches](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- I usually don't bother creating additional branches beyond the local and remote master versions
- If multiple people are working on the same project, it's nice if they can work on the same files independently without having to merge everything together each time they make a commit
    - This can acomplished by creating seperate branches for each person, then merging everything together once everyone has completed their part of the project
- Can also be useful to "snapshot" your project, eg; when submitting for publication
- Create a new branch by clicking on the ```Create new branch``` is in the upper left, next to the ```master``` dropdown menu
- Give your branch a name and click ```OK```
- Any changes will now be confined to your new branch
- The ```Sync``` button will now be replaced by a ```Publish``` button because there is no copy of your new branch on the server to sync to
- After clicking on ```Publish```, the ```Sync``` button will come back and any changes made to your files will not conflict with changes on the master version on the server
- You can now use the drop down menu in the upper left to switch between branches, which will automatically change the code in your folder
- By some black magic, this changes the files that are available in the finder
- When you are ready to merge branches back together, return to master, click on ```compare``` then select the branch you want to merge from, and click on ```Update from ...```
- It's usually a good idea to merge from master to your new branch and resolve any conflicts, then merge back into master

### Forking
- You can clone projects created by other people using ```git clone```, or by downloading a zip file, but if you want to add it to GitHub Desktop, you need to ```fork``` it
- This creates a copy that is owned by you which you can modify however you want
- If you think your changes might be useful to users of the original code, you can submit a ```pull request```
- If the owner of the original project agrees that your changes are an improvement, they can accept, which merges your code into theirs
- You can also merge modifications of the original code (or other forked versions) into your fork

### Using git from the command line 
- Copy project url and clone locally:
    - ```git clone https://github.com/hobrien/RNAseqTools.git```
- List modified files:
    - ```git status -s```
- Add changes to project:
    - ```git add FILENAME```
    - ```git commit -m "initial commit"```
    - can also commit without -m flag, which opens the commit message in your text editor of choice
- Update local version with remote changes:
    - ```git pull```
- Update remote repo with local committed changes:
    - ```git push```
    - always sync in this order
- Undo changes to a file:
    - ```git checkout -- FILENAME```
- Create new branch:
    - ```git branch -b Version2```
- Switch branches:
    - ```git checkout master```
- Merge branches:
    - ```git merge Version2```
    - ```git branch -d Version2```
- See [here](http://ohshitgit.com) for some examples of how to fix mistakes
