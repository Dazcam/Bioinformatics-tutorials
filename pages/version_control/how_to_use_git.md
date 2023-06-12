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
    
***

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
