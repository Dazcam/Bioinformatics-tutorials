---
layout: page
title: What is your path and making scripts executable
description: A description of the PATH in proramming
---

### What is the PATH?

Each computer has a PATH, which is effectively a list of folder addresses that the computer searches through 
whenever you try to run a program (or an executable file) on the command line. Once the location of the  
executable file is in your path you can call that file from anywhere in the file hierarchy. 

You can see your path at any time by typing:

~~~bash
$PATH
~~~

What is happening here is we are calling the global varaiable PATH which contains the list of directories.

A typical path looks something like this:

***

![path]({{ site.baseurl }}/assets/path.png)

***

All the directories included in the PATH are separated by colons.

When you run a command such as:

~~~bash
Bowtie2
~~~

The computer will search through all the folders listed in your path for the executable file associated with 
*Bowtie2*. If the file is there the program will run, otheriwse an error is thrown.

Frequently directories are included in your path by default such as the `~/bin` file, but not always. 


### Making files executable

When writing your own scripts, sometimes you need to make them executable. There is a permissions hierarchy for 
each file stored on any computer and there are 3 levels where you can permit access, the owner, group and other 
user levels, and at each level you can grant read, write and executable permissions.

You can visualise these permissions using `ls` with additional flags.

~~~bash
#l - long
#r - reverse chronological
#t - chronological order
#h - convert size to readable
#a - reveal hidden files

ls -lrtha
~~~

***

![nopermission]({{ site.baseurl }}/assets/no_permission.png)

***

The additional flags to the `ls` command provide a more detailed list of the files in the current directory including their 
permissions. The series of letters and dashes at the start of each line relate to permissions. I've created a file called 
**test.sh**. Notice that the 10th position in the series contains a dash, this means the `test.sh` file is is **not** 
executable. If it were executable this position would conatin an `x`.

Trying to execute `test.sh` would result in a permissions denied error message.

To alter permissions we use the `chmod` command. 

~~~bash
chmod +x test.sh
~~~

***

![permission]({{ site.baseurl }}/assets/permission.png)

***

Notice that the 10th position in the series for *test.sh* is now **x**, so the file has been made executable. The chmod 
command can be used to change read and write permissions at various levels but will not be covered here. 
For more information see [here](https://ryanstutorials.net/linuxtutorial/permissions.php).

Once you have made a file executable you can only run it from the directory it is contained in. Normally you need to
execute	files from anywhere in your directory hierarchy. So you need to add it to your PATH.

***

### Adding executable files to your path

Once you have made a file executable you can only run it from the file it is contained in using `./` before the 
name of the file. Note this will throw an error if the file is not executable.

~~~bash
./test.sh
~~~

Normally you need to execute files from anywhere in you directory hierarchy. So you need to add it to your PATH.

There are two ways to add an executable file to your path. For individual executables you can create a symbolic 
link (aka symlink) for the executable. 

~~~bash
ln –s ~/Programs/bowtie2/test.sh ~/bin/test.sh
~~~

What’s happening here is you are creating a pointer called `test.sh` in your `~/bin` directory that points to the 
place where the `test.sh` executable is stored on your computer, in this case in the '~/Programs/bowtie2' folder.
As the `~/bin` file is in your PATH already, the computer searches `~/bin`, is directed to the /Programs/bowtie2
directory and locates test.sh.  


The other option, particularly if you have several executables stored in one directory, it’s easier to add the 
directory directly to your PATH. 

***

### What is your .bash_profile?

Alternatively, and this is my preference, you can add executables to your path via your **.bash_profile**. Your *.bash_profile* is 
a hidden file that is loaded up before the terminal loads your shell environment and it contains any preferences you have 
for the global environment that is created, see [here](https://natelandau.com/my-mac-osx-bash_profile/).

~~~bash
nano ~/.bash_profile # opens your bash profile in the text editor called nano so you can add things

export PATH=~/Programs/bowtie2:$PATH  
~~~

By including the **export** command here you are making all the executables in the `~/Programs/bowtie2` folder available to all 
subprocesses that are run from the shell. Note that after adding anything to your bash profile in your current session you 
must type `source ~/.bash_profile` afterward for alterations to be activated in the current session. Alternatively, you can 
close the shell window and reopen a new window and the path will be updated. 

***

### Final word PATH - file locations 

It is important to note the difference between the two path commands above, i.e. `PATH=$PATH:newFileLocation` or 
`PATH=newFileLocation:$PATH`. In essence, these commands are doing the same thing, adding a file/folder to your path. The difference 
between them is in where the new file address you are adding is placed in relation to the list of addresses already contained within 
the path. So, in both commands we are assigning a new variable called PATH to replace the old path. Within that path, we want to place 
the list of addresses stored in the old PATH (denoted by the $PATH variable) and add the location of our new folder/file 
(denoted by the newFileLocation (i.e. ~/Prorams/bowtie2). You can see the difference of the two commands above if we use the **colon as 
the separator**. In the first example, we place the list of addresses in the old path before the new file location and in the second 
example the old path list is placed after the new file location.

It is important to understand that when the computer searches your path for an executable, it runs through the list of addresses 
sequentially first to last. So, if you want to use a different version of a package containing executables with the same name os the old 
package, i.e. python, you should add this new executable’s folder to the start of your path. This is to ensure that when your computer is 
searching your path for the corresponding executable, it finds the newer version of the executable first. Once the computer finds the 
first executable it will stop searching, so will never reach the older version that occurs later in the path. 

***

Move on to [Installing required packages]({{ base.url }}/pages/linux101/installing_required_packages.html),
or back to [Scope]({{ site.baseurl }}/pages/linux101/scope.html).
