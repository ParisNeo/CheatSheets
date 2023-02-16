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

# Example batch for python coded

```bash
#!/bin/bash
#SBATCH -J myjob
#SBATCH -o output_%n.log
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=$SLURM_CPUS_PER_TASK
#SBATCH --mem=$SLURM_MEM_PER_NODE
#SBATCH --gres=gpu:$SLURM_GPUS_PER_NODE
#SBATCH --time=1:00:00

echo "Starting job on node $SLURM_NODELIST..."

# Load Conda module and activate environment
module load anaconda
conda activate myenv

# Execute Python script
python myscript.py

# Deactivate Conda environment
conda deactivate

echo "Job completed on node $SLURM_NODELIST."
```

Here's what each line does:

#!/bin/bash specifies that this script should be run using the Bash shell.
The #SBATCH lines specify options for the Slurm job scheduler. --job-name sets the name of the job (myjob in this case), --output specifies the name of the output log file (output_<node_name>.log), --nodes sets the number of nodes to use (1 in this case), --ntasks-per-node sets the number of tasks per node (1 in this case), --cpus-per-task sets the number of CPU cores per task (using the $SLURM_CPUS_PER_TASK environment variable), --mem sets the amount of memory per node (using the $SLURM_MEM_PER_NODE environment variable), --gres sets the GPU resources per node (using the $SLURM_GPUS_PER_NODE environment variable), and --time sets the maximum runtime for the job (1 hour in this case).
- The echo command prints a message to the console to indicate that the job has started.
- The module and conda commands load the Anaconda module and activate the myenv environment.
- The python command runs the myscript.py Python script.
- The conda deactivate command deactivates the myenv environment.
- The final echo command prints a message to the console to indicate that the job has completed.

To submit a job using this script, you can run:
```
sbatch -c 8 --mem=16G --gres=gpu:1 \
-o output_<node_name>.log -J myjob --mail-user=<email_address> \
--mail-type=END pyjob.sh
```

This will run the pyjob.sh script with the specified resource requirements and output to the specified log file. Once the job completes, a notification will be sent to the specified email address.

# Conclusion
This is just a brief introduction to SLURM. For more information, see the official documentation.



