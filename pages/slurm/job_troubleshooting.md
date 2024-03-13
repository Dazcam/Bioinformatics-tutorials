

#### Assessing resource usage

There are two common commands to use to check resource usage on Slurm:

+ `seff` - to obtain detailed job inormation on a single job
+ `sacct` - to obtain minimal data on a list of jobs

When using `seff <slurmjobid>` the ouput will look something like this:

```bash

```

<details>

<summary> Common seff commands  </summary> 

Running `sacct` shows the jobs you have run on the current date (i.e. since last midnight). 
To specify a time interval (must be within 3 months):

```bash
sacct -S YYYY-MM-DD
```

To print out all available data on a completed job:

```bash
sacct -l -j <slurmjobid> 
```

Use the `-o` option to specify specific output For example. The following shows
job name, job ID, used memory, job state and elapsed wall-clock time:

```bash
sacct -o jobname,jobid,maxrss,state,elapsed -j <slurmjobid> 
```

To print a list of all available data `sacct` data fields:

```bash
sacct -e
```

</details>

<details>

<summary> Common sacct commands  </summary> 
To print out all available data on a completed job:

```bash
sacct -l -j <slurmjobid> 
```

Use the `-o` option to specify specific output For example. The following shows
job name, job ID, used memory, job state and elapsed wall-clock time:

```bash
sacct -o jobname,jobid,maxrss,state,elapsed -j <slurmjobid> 
```

To print a list of all available data `sacct` data fields:

```bash
sacct -e
```

</details>
