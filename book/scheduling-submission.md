# Session 5: Introduction to Job Scheduling and Batch Jobs

In this session, you'll explore:

* What job schedulers are and their importance
* How to write and submit batch job scripts
* Key SLURM commands for managing your tasks
* Working with output and error files
* How to use modules to manage environments

---

## What Is a Job Scheduler? 🤖

A **job scheduler** is a software that manages and prioritizes compute jobs.

### Key Points to Explore:

* What do you think a job scheduler does? Write down your thoughts.
* How does a scheduler help in maximizing cluster resources?

**Task:** Review the following diagram on job scheduling:

* **User → Scheduler → Compute Nodes**

🔗 [More on Aire's job scheduler](https://arcdocs.leeds.ac.uk/aire/usage/job_scheduler.html)

---

## What Are Batch Jobs? 📝

**Batch jobs** are non-interactive tasks that are scheduled and run on the cluster.

### Key Concepts to Understand:

* How do batch jobs differ from interactive sessions?
* What does the **fair-share policy** mean?

**Self-Reflection:** Think about how batch jobs are queued and prioritized on the system. Why is the fair-share policy important?

---

## Writing Job Scripts ✍️

To create a job script, you’ll need to:

* Edit with `nano`, `vim`, or `emacs` (Beginner? Try `nano`).
* Always include `#SBATCH` directives at the top.

### Key Steps to Try:

1. Write a simple script with these directives: `--time`, `--mem`, `--cpus-per-task`.
2. What happens if you forget a necessary directive, like `--time`?

**Tip:** If you wrote your script on Windows, run `dos2unix` to avoid submission errors.

🔗 [Writing scripts on Aire](https://arcdocs.leeds.ac.uk/aire/usage/job_type.html#writing-job-scripts)

---

## Simple Serial Job 🚀

### Example Job Script:

```bash
#!/bin/bash
#SBATCH --job-name=basic_job
#SBATCH --time=01:00:00
#SBATCH --mem=1G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
```

**Activity:** Create and submit a simple job using the script above. What do you expect the output to look like?

---

## Viewing & Cancelling Jobs 🔍✂️

**Learn how to monitor jobs:**

* Use `squeue` to list running jobs.
* Example output:

```bash
JOBID  PARTITION  NAME        USER  ST  TIME  NODES  NODELIST(REASON)
12345  general    basic_job   user  R   00:02  1      node01
```

### Challenge:

* Run `squeue` and identify the status of your current job.
* Try canceling a job using `scancel <JOBID>`. What happens?

🔗 [Monitoring jobs](https://arcdocs.leeds.ac.uk/aire/usage/job_type.html#using-the-queue)

---

## Task Arrays Simplified 🔁

**Task arrays** allow you to submit multiple jobs with similar scripts, each identified by a unique task ID.

### Try It:

1. Write a script with the following:

```bash
#SBATCH --array=1-3
#SBATCH --output=array_%A_%a.out
```

2. Run `sbatch` with your task array script.
3. Check the output files for each task.

---

## Interactive Sessions 💻

**Interactive sessions** are useful for real-time testing and debugging.

### Self-Test:

1. Start an interactive session: `srun --pty -t01:00 bash`
2. Inside the session, load a module, e.g., `module load python`.
3. Exit the session and reflect on the limitations.

**Reflection:** Why are interactive sessions not recommended for long-term tasks?

---

## Working with Output and Error Files 🖥️

Every job generates two files:

* **Standard Output** (`slurm-<jobID>.out`)
* **Standard Error** (`slurm-<jobID>.err`)

### Activity:

1. Submit a job script.
2. Check the output and error files.
3. Try redirecting them to specific locations using `#SBATCH --output=/path/to/output_file`.

---

## High Memory & GPU Nodes 🧠💻

If your job requires **high memory** or **GPU nodes**, request them using `--mem` or `--gres=gpu`.

### Key Challenge:

1. Request high memory for your job by adding `#SBATCH --mem=256G`.
2. Try requesting a GPU with `#SBATCH --gres=gpu:1`.

**Reflection:** When would you need high memory or GPU resources?

🔗 [High Memory and GPU Nodes](https://arcdocs.leeds.ac.uk/aire/usage/high_memory_gpu.html)

---

## SLURM Options at a Glance 📊

| Option            | Purpose                  |
| ----------------- | ------------------------ |
| `--time`          | Runtime (required)       |
| `--cpus-per-task` | Cores per task           |
| `--mem`           | Memory per node          |
| `--partition`     | Select queue (e.g., gpu) |

### Challenge:

* Review the table and write down which options you think are most important for your work.

---

## Recap & Quiz 🎯

**Test Your Knowledge:**

1. What does a job scheduler do?
2. Which flag sets the number of CPU cores per task?
3. How do you cancel a job with ID 42?
4. What’s the default memory allocation per job?

---

## Exercise 🔬

**Create a job script** that:

1. Requests 2 cores and 4 GB RAM
2. Runs for 30 minutes
3. Is part of a 5-task array
4. Loads the Python module

**Check Your Solution:** Compare your script with the example provided.

```bash
#!/bin/bash
#SBATCH --job-name=capstone
#SBATCH --time=00:30:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G
#SBATCH --array=1-5
#SBATCH --output=capstone_%A_%a.out
#SBATCH --error=capstone_%A_%a.err

module load python

echo "Job ID: $SLURM_JOB_ID"
echo "Task ID: $SLURM_ARRAY_TASK_ID"
hostname
module list
```

---

## Closing Thoughts 🔍

You’ve now learned how to:

* Write and submit batch jobs
* Monitor job status and cancel jobs
* Utilize high memory and GPU nodes

For further exploration, revisit the documentation linked throughout this session, and practice writing and submitting scripts.

📚 For full documentation, visit: [arcdocs.leeds.ac.uk/aire](https://arcdocs.leeds.ac.uk/aire)

---

### **Next Steps:**

1. Keep practicing by submitting your own job scripts and experimenting with SLURM options.
2. Try using advanced SLURM features as you become more familiar with the system.