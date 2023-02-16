# SLURM Cheat Sheet
SLURM is an open-source workload manager and job scheduler for Linux and Unix-like systems.

# Basic Commands
Here are some basic SLURM commands:

| Command |	Description |
|sbatch script.sh	|Submit a batch job using the script script.sh.|
|squeue	| Show information about running and pending jobs.|
|scancel jobid	| Cancel a running or pending job with ID jobid.|
|scontrol show job jobid	| Show detailed information about job with ID jobid.|
|sinfo	| Show information about the compute nodes in the cluster.|

# Job Scripts
To submit a batch job, you need to create a job script that contains the commands to run the job. Here's an example job script:

```bash
#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G

echo "Hello, world!"
```
In this example, the #SBATCH lines are SLURM directives that specify job parameters, such as the job name, number of tasks, CPUs per task, and memory allocation. The last line is a command to be executed by the job, which in this case is simply printing a message.

To submit the job script, you would run the sbatch command, like this:

```bash
sbatch myscript.sh
```

# Job Arrays
You can also submit job arrays, which are multiple jobs that run with slightly different parameters. Here's an example job array script:

```bash
#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --array=1-10

echo "Hello, world! This is job $SLURM_ARRAY_TASK_ID."
```

In this example, the #SBATCH --array directive specifies that the job array should consist of tasks 1 through 10. The last line of the script uses the SLURM_ARRAY_TASK_ID environment variable to print a message with the task ID.

To submit the job array script, you would run the sbatch command, like this:

```bash
sbatch myscript.sh
```

# Conclusion
This is just a brief introduction to SLURM. For more information, see the official documentation.



