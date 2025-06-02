# Session 5: Introduction to Job Scheduling and Batch Jobs

In this session, you will learn:

* What a job scheduler is and why it is used
* How to write and submit batch job scripts
* How to monitor, manage, and cancel jobs
* How to use modules to set up software environments
* How to request high memory and GPU resources
* (Optional) How to submit task arrays

---

## Background: What is a Job Scheduler?

High Performance Computing (HPC) systems are shared by many users, each submitting their own jobs — code they want to run using the cluster's compute power.

A **job scheduler** is the system that:
- Organizes when and where jobs run
- Allocates the requested resources (CPU cores, memory, GPUs)
- Ensures fair access to shared resources for all users

Schedulers make decisions based on:
- What resources your job requests (e.g., how many cores, how much memory)
- How long your job will run (your time limit)
- Current system load
- Fair-share policies (giving all users fair access over time)

**Without a scheduler**, users would have to manually coordinate access to thousands of CPUs — impractical and chaotic.

---

## SLURM: The Scheduler on Aire

At Leeds, the **SLURM** scheduler (Simple Linux Utility for Resource Management) manages all jobs on the Aire cluster.

When you submit a job:
1. You describe what you need (e.g., CPUs, memory, time) in a **job script**.
2. You submit the job to SLURM with `sbatch`.
3. SLURM places your job in a **queue**.
4. When enough resources are available and your job’s priority is high enough, SLURM starts the job on suitable **compute nodes**.

---

### How Jobs Flow Through the System

```plaintext
Your Job Script
       │
       ▼
    SLURM Scheduler
       │
       ├── Queues Jobs
       ├── Prioritizes Jobs
       ├── Allocates Resources
       ▼
 Compute Nodes (Run the job)
       │
       ▼
    Output Files
````

---

### Common Job States

| **State**   | **Meaning**                                |
| ----------- | ------------------------------------------ |
| `PENDING`   | Job is waiting for resources               |
| `RUNNING`   | Job is actively running on compute nodes   |
| `COMPLETED` | Job finished successfully                  |
| `FAILED`    | Job failed (e.g., errors, exceeded limits) |
| `CANCELLED` | Job was manually stopped (e.g., by user)   |

You can monitor job states with the `squeue` command.

---

### Why Do Jobs Wait?

Not every job runs immediately. Reasons include:

* Not enough CPUs/memory free
* Higher priority jobs ahead of yours
* Fair-share adjustment (users who have used less recently get higher priority)
* Your job is requesting rare resources (e.g., GPU nodes)

---

### How You Interact with the Scheduler

| **Action**     | **Command**            |
| -------------- | ---------------------- |
| Submit a job   | `sbatch my_job.sh`     |
| View your jobs | `squeue -u <username>` |
| Cancel a job   | `scancel <jobID>`      |

You will learn these commands and write your first job script in the next sections.

---

## Why Use Batch Jobs?

Batch jobs allow you to:

* Set up your work once
* Submit it to the scheduler
* Log out and let it run unattended
* Automatically capture outputs and errors into files

This is essential for longer jobs that run for hours or days — you don't need to stay logged in.

---

## Summary

* A job scheduler manages who runs jobs and when on an HPC cluster.
* SLURM is the scheduler used on Aire.
* You write a job script to describe what you need.
* SLURM queues, prioritizes, and runs your job on available compute nodes.
* You interact with SLURM via simple commands like `sbatch`, `squeue`, and `scancel`.

---

# Hands-On Practical

---

## Hands-On: Write and Submit a Simple Job

**Exercise:**
Write a batch script requesting:

* 2 CPUs
* 4 GB memory
* 30 minutes runtime
* Load the Python module
* Run a simple command like `hostname`

Submit the script.

**Answer:**

```bash
#!/bin/bash
#SBATCH --job-name=simple_job
#SBATCH --time=00:30:00
#SBATCH --mem=4G
#SBATCH --cpus-per-task=2
#SBATCH --output=simple_output_%j.out
#SBATCH --error=simple_error_%j.err

module load python

hostname
```

Submit using:

```bash
sbatch simple_job.sh
```

**Expected Output:**

* You will receive a job submission message in the terminal like:

  ```
  Submitted batch job 123456
  ```

* After the job completes, check the output file `simple_output_123456.out` in your current working directory.

* **Contents of Output File:**

  ```
  nodeXYZ.arc.leeds.ac.uk
  ```

  (Your job ran `hostname`, so you get the compute node name.)

---

## Monitoring Jobs

Check job status:

```bash
squeue -u <your-username>
```

Cancel a job:

```bash
scancel <JOBID>
```

**Exercise:**
Submit a job and use `squeue` to monitor it. Cancel the job once it starts running.

**Answer:**

1. Submit job: `sbatch simple_job.sh`
2. Monitor job: `squeue -u <your-username>`
3. Cancel job: `scancel <JOBID>`

**Expected Output:**

* `squeue` shows your job in the queue:

  ```
  JOBID    PARTITION  NAME        USER    ST  TIME   NODES  NODELIST(REASON)
  123456   general    simple_job  user1   PD  0:00   1      (Priority)
  ```

  (Status `PD` means Pending; `R` means Running.)

* After `scancel`, job disappears from `squeue` list.

---

## Interactive Jobs

Interactive jobs are useful for quick testing or debugging:

```bash
srun --pty --time=01:00:00 bash
```

**Exercise:**

1. Start an interactive session.
2. Load the Python module.
3. Run `hostname`.
4. Exit the session.

**Answer:**

```bash
srun --pty --time=01:00:00 bash
module load python
hostname
exit
```

**Expected Output:**

* Terminal will change — you’ll have a shell prompt on a compute node.

* After running `hostname`, you’ll see the node name:

  ```
  nodeXYZ.arc.leeds.ac.uk
  ```

* `exit` returns you to the login node.

---

## Output and Error Files

SLURM creates:

* `slurm-<jobID>.out` — standard output
* `slurm-<jobID>.err` — standard error (if specified separately)

Control output filenames:

```bash
#SBATCH --output=/path/to/output_file.out
#SBATCH --error=/path/to/error_file.err
```

**Exercise:**
Modify your script to redirect output and error to specific files.

**Answer:**

```bash
#!/bin/bash
#SBATCH --job-name=simple_job
#SBATCH --time=00:30:00
#SBATCH --mem=4G
#SBATCH --cpus-per-task=2
#SBATCH --output=/home/<username>/my_output.out
#SBATCH --error=/home/<username>/my_error.err

module load python

hostname
```

**Expected Output:**

* After job completion, you will find:

  * `/home/<username>/my_output.out` — contains node name from `hostname`.
  * `/home/<username>/my_error.err` — should be empty if no errors.

---

## High Memory and GPU Requests

For high-memory jobs:

```bash
#SBATCH --mem=256G
```

For GPU jobs:

```bash
#SBATCH --gres=gpu:1
```

**Exercise:**
Update your batch script to request:

* 256 GB memory
* 1 GPU

**Answer:**

```bash
#!/bin/bash
#SBATCH --job-name=highmem_gpu_job
#SBATCH --time=00:30:00
#SBATCH --mem=256G
#SBATCH --cpus-per-task=2
#SBATCH --gres=gpu:1
#SBATCH --output=highmem_output_%j.out
#SBATCH --error=highmem_error_%j.err

module load python

hostname
```

Submit using:

```bash
sbatch highmem_gpu_job.sh
```

**Expected Output:**

* Terminal submission message:

  ```
  Submitted batch job 123457
  ```
* Output file `highmem_output_123457.out` will contain:

  ```
  nodeXYZ.arc.leeds.ac.uk
  ```

  (Node assigned may be a GPU node.)

---

## (Optional) Task Arrays

Arrays allow submitting multiple similar jobs efficiently.

Example script:

```bash
#!/bin/bash
#SBATCH --job-name=array_job
#SBATCH --array=1-3
#SBATCH --output=array_output_%A_%a.out

module load python

python my_script.py $SLURM_ARRAY_TASK_ID
```

Submit:

```bash
sbatch array_script.sh
```

**Exercise (Optional):**
Write a batch script to submit a task array with 3 tasks, each printing its task ID.

**Answer:**

```bash
#!/bin/bash
#SBATCH --job-name=array_example
#SBATCH --array=1-3
#SBATCH --output=array_%A_%a.out

module load python

echo "Task ID: $SLURM_ARRAY_TASK_ID"
hostname
```

Submit using:

```bash
sbatch array_example.sh
```

**Expected Output:**

* Three output files:

  ```
  array_<jobID>_1.out
  array_<jobID>_2.out
  array_<jobID>_3.out
  ```
* Each file contains:

  ```
  Task ID: 1
  nodeXYZ.arc.leeds.ac.uk
  ```

  (or `2`, `3` — depending on the task.)

---

## Further Reading

* [Aire Job Scheduling Guide](https://arcdocs.leeds.ac.uk/aire/usage/job_scheduler.html)
* [Writing Job Scripts](https://arcdocs.leeds.ac.uk/aire/usage/job_type.html)
* [Interactive Jobs](https://arcdocs.leeds.ac.uk/aire/usage/job_type.html#interactive)

---

**Next Steps:**

* Practice writing and submitting simple job scripts.
* Experiment with resource requests.
* Explore more advanced SLURM features as needed.

---

## **Recap Quiz**

**Q1.**
What is the purpose of a job scheduler on an HPC system?

* A) To speed up internet connections
* B) To manually assign jobs to users
* C) To allocate compute resources and manage job queues
* D) To monitor user emails

> **Answer:**
> **C) To allocate compute resources and manage job queues**

---

**Q2.**
Which scheduler is used on the Aire HPC system?

* A) PBS
* B) SLURM
* C) LSF
* D) Grid Engine

> **Answer:**
> **B) SLURM**

---

**Q3.**
What does the job state `PENDING` mean?

* A) The job is actively running on a node
* B) The job is waiting for available resources
* C) The job has completed successfully
* D) The job was cancelled by the user

> **Answer:**
> **B) The job is waiting for available resources**

---

**Q4.**
Which command would you use to submit a job script to SLURM?

* A) `srun`
* B) `squeue`
* C) `sbatch`
* D) `scancel`

> **Answer:**
> **C) sbatch**

---

**Q5.**
Why is it important to set a time limit (`--time`) in your job script?

* A) It makes the job run faster
* B) It helps SLURM schedule jobs more efficiently
* C) It avoids the job being cancelled for overrun
* D) Both B and C

> **Answer:**
> **D) Both B and C**

---
