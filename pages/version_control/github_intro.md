---
layout: page
title: Version control with Git
description: Version control with Git inroduction
---

Version control is an important concept in computer science. It refers to the process where as you update
or make changes to the code you are working on, you create regular snapshots (or versions) of that project 
as it develops. This makes it easy to return to a previous 'working' version of the project if, for example,
you introduce code in a later version that makes the process run less efficently or if multiple people are 
working on the same project (or if one person works on it in multiple places), changes can be easily be tracked, 
merged and synced independently. 

Code is stored on online repositories such (eg, [GitHub](https://github.com/) or [GitLab](https://about.gitlab.com/)), 
GitHub is a free service, and code can be made publicly or privately available.

### [The preferred workflow for Git](http://nvie.com/posts/a-successful-git-branching-model)

![GitFlow Branching Model](http://nvie.com/img/git-model@2x.png)

### [A realistic workflow for Git](https://xkcd.com/1597)

![XKCD Branching Model](http://imgs.xkcd.com/comics/git.png)

***

### How to use Git

There are several ways to work with and upload code to GitHub:

- In your web browser on the GitHub website
- On the command line
- Using GitHub Desktop 
- Using 3rd party applications (e.g. via R)

My preference is to use the command line, though for updating README files I tend to use the browser. I'll be 
focusing on using the command line here.

***

### Create a project

The basic process is the following:

- Create [GitHub](github.com) account and initialise a new project with ```README.md``` file
- Click on ```clone or download``` and copy url
- Open terminal and clone project:
    - ```git clone [URL]```
- Open ```README.md``` in text editor, add some text and save it
- Commit changes to repo and sync:
    - ```git add README.md```
    - ```git commit -m "First commit"```
    - ```git push```
- Your changes you made to the readme file should be visible in your GitHub project after you refresh

***


### Resolving conflicts
- If sync fails because changes on remote server can't be automatically merged with local changes, open file in your text editor and edit conflicts manually
- Conflicts look like this:

    ```
    <<<<<<<<<<<< HEAD
    local version of code
    =============
    remote version
    >>>>>>>>>>>> origin/master
    ```

- Edit file to include whatever parts from the local and remote versions you please (and remove markers added by Git)
- Return to GitHub Desktop, commit changes, and redo sync
- If all else fails, save local files in another location, delete the project, clone fresh copy from server, then add back in your local changes
    
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

### A few additional considerations
- SSH keys
    - If you are going to be pushing changes to GitHub from the command line often, it's a good idea to create an SSH key and add it to GitHub so that you don't have to enter your password every time
    - See [here]({{ site.baseurl }}/WorkflowTutorial/SystemConfig#avoiding-passwords) to see how to generate an ssh key
    - On GitHub, go to ```Settings``` and click on ```SSH and GPG keys```
    - Type in a descriptive name for your computer and paste in your public key
    - You'll also need to set your git user name:
        - ```git config --global user.name USERNAME```
        
- Bookmarking snapshots (example: the version used for the analyses in a paper submission)
    - Make zipped copy of directory
    - Make a note the number of the last commit before submission (eg; ce289b3)
    - Create a separate branch for each submission
    - Tag commit
        - Go online and click on the ```0 releases``` button beneath the project description
        - Click on ```Create a new release```
        - Give release a name (unfortunately only names like "v1.0" are supported)
        - Click on ```This is a pre-release``` if it's not the final version of the paper, then on ```Publish release```
        - This will automatically create a ziped version of all the code that can be navigated to by clicking on the ```1 release``` button for the project

- Excluding private files
    - When you initialise a repo, a .gitignore file will automatically be created with a list of files that will never be synced to the server
    - Files on the list will not show up on the lists of modified files to add to a commit
    - It's a good idea to list any data files that you don't want to make public in this file, along with any config files that contain things like login credentials
    - It is a *really* good idea to keep this list up to date, because it's easy to accidentally commit all untracked files in your project folder
        - If you do this with genome-scale files, it will break *everything*
        - It's also not trivial to delete files from [github](github.com) once you've committed them, because git wants to keep a record of all changes made to all files
    - Of course, any files that are not part of the repo will need to be synced some other way

- README, LICENSE
    - When a repo is created online, you have the option of creating a README.md file that is displayed automatically on the landing page for your project
    - If you are creating a repo locally, you'd need to do this manually with a text editor
    - You are also given the option of adding a license to the repo, though the available options are tailored to software (I would suggest using the Creative Commons [CC-BY](https://creativecommons.org/licenses/by/2.0/uk/legalcode) license for most biology projects)

- Meta-notebooks
    - Notes can be organised in a logical order, without interleaving unrelated work that was done on the same day
    - Commit history preserves a chronological record
    - Additional explanation, comments, etc can be recorded in commit message without cluttering notes
    - If you're fortunate enough to have lab mentors, they can easily review your notes and leave comments
    - If using the free version of GitHub, you may need to be a bit coy about exactly what you are working on to prevent your competetors from stealing you work

- Using Git for manuscript writing
    - Can be used to avoid this:
    
    ![tweet](https://raw.githubusercontent.com/MixedModels/LearningMLwinN/master/ScreenShots/tweet.png)
    - Merging changes by different co-authors is done automatically
    - Keeps a record of all changes, who made them, and why
    - Discussions about particular changes can be had in the comment thread for that change instead of in email threads
    - Issue tracker can be used as a to-do list, and also includes comment threads for discussion of proposed tasks
    - Branching can be used to allow a co-author to make a series of commits that can all be merged at once
    - This may well be something you'd want a private repo for