---
layout: page
title: Assigning variables and environmental SCOPE
description: How to assign varaibels in BASH and understanding environmental scope
---

### Assigning variables

Variables are used in environments to assign information. In bash we assign variables using the `=` sign. The variable 
is on the left and the information we want to assign is on the right. It works similarly to algebra. 

~~~bash
x=1
~~~

After typing this any time you type `echo $x` in your current shell session the computer will interpret `x` as `1`. Note 
that this is only true in the current session. In bash the `$` sign must proceed a call to any variable and spaces are 
not allowed at either sided of `=`. The 'echo' statement prints whatever comes next to the screen.

***

### Scope

SCOPE is an important concept in programming to be aware of, and it relates to the environments you
are working in. For example, variables set within a local script, will not be available in the global environment
unless we explicitly state that they should be.

When we	create a script an enclosed local environment is generated for it to run in. This means that when a call is made 
to serach a partiocular variable within the script the computer  only searches for variable assignments set within that 
local environment. To take this one stage further, if we created a for loopwithin our script this a further instance of 
an enclosed local environment, within the scripts environment, within the shell;s global environment..

So if we set `x=1` within the for loop,	`x` will only `=` 1 in the for loop.            

If we set `x=1` in the script `x` will `=` `1` in the script and the for loop (as loops within scripts can access variables
set in the script), however, `x` will not `=` `1` in the global environment in any of the above	scenarios.

It is possible to assign global environmental variables that can be recognised by all scripts run in that global
environment using the `export` command, but this is not recommended. 

~~~bash
export x=1
~~~

Here the export command makes the `x` variable accessible to all subprocess run from the main shell.

***
