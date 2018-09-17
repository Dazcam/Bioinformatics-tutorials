---
layout: page
title: Environmental scope
description: How to assign varaibels in BASH and understanding environmental scope
---

### Scope

Scope is an important concept in programming to be aware of and it relates to the accessibility of variables within the 
environment you are working in. 

When we open a terminal window we are located in the **global environment** for that session. Whenever a script is run within the global 
environment an additional enclosed local environment is generated for it to run in. This is important when setting variables
as setting variables in one environment does not necessarily make it available in all environments. 

For example, variables set within the local script environment will not be available in the global environment unless we explicitly 
state that they should be.

When a variable call is made within the script the computer only searches for variable assignments set within that 
local environment. To take this one stage further, if we created a **for** loop within our script this creates a further instance of 
an enclosed local environment, within the scripts environment, within the shell's global environment.

So if we set `x=1` within the for loop,	`x` will only `= 1` **in the for loop**.            

If we set `x=1` in the script `x` will `=` `1` **in the script and the for loop** (as loops within scripts can access variables
set in the script), however, `x` will not `=` `1` in the global environment in any of the above	scenarios.

It is possible to assign global environmental variables that can be recognised by all scripts (and loops) run within that global
environment using the `export` command, but this is not recommended. It is much tidier to assign variables in scripts and loops.

~~~bash
export x=1
~~~

Here the export command makes the `x` variable accessible to all subprocess run from the main shell.

***

Move on to [Making scripts executable](/SRA-to-Peak/pages/make_script_executable.html), or back 
to [Linux 101]({{ site.baseurl }}/pages/linux101.html)
