# **Session 6: HPC Best Practices & Troubleshooting**

## **Learning Outcomes**

By the end of this session, you will be able to:

* Recognize common job submission and system usage issues.
* Apply systematic troubleshooting techniques.
* Optimize resource usage and avoid common pitfalls.
* Navigate documentation effectively (arcdocs, Google).
* Submit an effective support ticket when needed.

---

## **Background / Introduction**

High Performance Computing (HPC) systems are powerful but complex environments. Errors and failures are common, especially for new users. Learning how to systematically troubleshoot saves time, reduces frustration, and improves your productivity.

You are encouraged to resolve problems independently before seeking help. This session will provide practical tools and workflows to diagnose issues and know when and how to escalate.

---

## **Common Issues with Job Submission and System Usage**

* **Job stuck in PENDING**:

  * Insufficient resources available.
  * Requested rare resources (e.g., GPUs, high-memory).
  * User priority is low (fair-share scheduling).

* **Job fails immediately**:

  * Syntax error in script.
  * Incorrect module loaded or missing environment setup.
  * File paths incorrect or inaccessible.

* **Job exceeds time/memory limits**:

  * Underestimated resource requirements.
  * Infinite loops or runaway jobs.

* **Storage problems**:

  * Disk quota exceeded.
  * Writing to non-scratch areas with insufficient space.

* **Permission errors**:

  * Incorrect file/directory permissions.
  * Attempt to write to read-only directories.

---

## **Strategies for Error Diagnosis and Resource Optimization**

A methodical approach helps:

1. **Read the Error Message**:

   * First line of defense.
   * Look for keywords like `Segmentation fault`, `Permission denied`, `Out of memory`.

2. **Check Output and Error Logs**:

   * Look at `.out` and `.err` files created by SLURM.
   * Search for unusual termination messages.

3. **Validate the Job Script**:

   * Are directives correct (`#SBATCH`)?
   * Paths to modules, data files correct?

4. **Use Monitoring Tools**:

   * `squeue` — check job status.
   * `scontrol show job <jobID>` — detailed job info.
   * `sacct` — view accounting data after job completes.
   * `seff <jobID>` — summarize efficiency (CPU and memory).

5. **Resource Requests**:

   * Adjust memory (`--mem`), CPUs (`--cpus-per-task`), or time (`--time`).
   * Test with smaller jobs first.

6. **Minimal Reproducible Example**:

   * Simplify the problem.
   * Remove unnecessary steps to isolate the cause.

7. **Restart Strategy**:

   * If a job crashes, ensure it can restart from checkpoints.

---

## **Bonus Tips**

\::::{admonition} 🔍 Troubleshooting Tips

* **Always check the `.err` file** first — many runtime errors are logged there.
* **Use `seff <jobID>`** after job completion to quickly check if memory and CPUs were used efficiently.
* **Google Smartly**: Put error messages in **quotes** to search exact phrases.
* **Save working job scripts** — version control isn't just for code.
  \::::

---

## **Sample Error Log Snippet**

Typical `.err` file:

```
Traceback (most recent call last):
  File "big_simulation.py", line 42, in <module>
    import numpy
ModuleNotFoundError: No module named 'numpy'

srun: error: node1234: task 0: Exited with exit code 1
```

**Interpretation**:

* Top error: Python cannot find the `numpy` module — indicates missing environment/module.
* Bottom error: SLURM shows that task 0 exited abnormally.

---

## **Guidance and Support**

### **Using arcdocs and Google Effectively**

* **arcdocs**:

  * [Aire HPC Documentation](https://arcdocs.leeds.ac.uk/)
  * Search with clear keywords (e.g., "job submission error", "SLURM memory limit").

* **Google**:

  * Copy error messages *verbatim* into search.
  * Use quotes for exact matches.

**Example**:

> Error: `srun: error: Unable to allocate resources: Requested node configuration is not available`
> Google: `"srun: error: Unable to allocate resources: Requested node configuration is not available" HPC SLURM`

---

### **Submitting a Support Ticket**

Only escalate if:

* You have attempted basic troubleshooting.
* The issue persists and blocks your work.

**How to Write a Good Ticket**:

1. **Clear Description**.
2. **Job Details** — job script, output/error logs.
3. **Environment Info** — loaded modules, software versions.
4. **What You’ve Tried**.

**Example**:

> **Subject**: Job Failing with Out of Memory — Aire HPC
>
> **Description**:
> I'm submitting a job with 32 cores and 128GB memory. It fails with "oom-killer" message after \~1 hour.
>
> **Script**:
>
> ```bash
> #SBATCH --cpus-per-task=32
> #SBATCH --mem=128G
> #SBATCH --time=2:00:00
> ```
>
> **Modules Loaded**:
>
> * python/3.8
> * mpi/openmpi-4.1
>
> **Error Logs**:
> `Out of memory: Kill process 12345 (python) score 1234 or sacrifice child`
>
> **Steps Tried**:
>
> * Reduced number of cores.
> * Increased memory request to 160GB (still fails).

---

## **Exercises**

### Exercise 1: Diagnose a Failed Job

You submitted:

```bash
#!/bin/bash
#SBATCH --job-name=test_fail
#SBATCH --time=00:30:00
#SBATCH --mem=2G
#SBATCH --cpus-per-task=4
#SBATCH --output=test_output.out
#SBATCH --error=test_error.err

module load python
python big_simulation.py
```

Error file contains:

```
ModuleNotFoundError: No module named 'numpy'
```

**Task**: Identify the issue and suggest a fix.

---

### Exercise 2: Find the Right Documentation

Error:

```
srun: error: Unable to create job step
```

**Task**: Use arcdocs or Google to find possible causes and solutions.

---

### Exercise 3: Draft a Support Ticket

Error:
`slurmstepd: error: Exceeded job memory limit`
Job requested 8GB memory. Simulation needs \~20GB.

**Task**: Draft a support ticket reporting the issue.

---

## **Answers / Expected Outputs**

### Exercise 1 Answer

**Issue**:

* The Python environment lacks the `numpy` module.

**Fix**:

* Load a Python module with `numpy` or install it.

Example:

```bash
module load python/3.10
pip install --user numpy
```

---

### Exercise 2 Answer

**Search Terms**:

> `SLURM srun Unable to create job step`

**Solution**:

* Insufficient resources or a mismatch between `srun` and job allocation.

---

### Exercise 3 Answer

**Support Ticket Draft**:

> **Subject**: Exceeded Job Memory Limit — Job ID 987654
>
> **Description**:
> Simulation failed with memory limit error.
>
> **Job Script**:
>
> ```bash
> #SBATCH --mem=8G
> #SBATCH --time=2:00:00
> ```
>
> **Error Log**:
> `slurmstepd: error: Exceeded job memory limit`
>
> **Modules Loaded**:
>
> * python/3.8
>
> **Steps Tried**:
>
> * Reviewed simulation memory needs (\~20GB).

---

## **Recap Quiz**

**Q1.**
What is the first step you should take when your HPC job fails?

> **Answer:** C) Read the error message and check logs.

**Q2.**
Which of the following is *NOT* a good practice for troubleshooting HPC jobs?

> **Answer:** B) Ignore error logs and focus on the job script.

**Q3.**
Where can you find official documentation for the Aire HPC system?

> **Answer:** B) arcdocs

**Q4.**
When is it appropriate to submit a support ticket?

> **Answer:** B) After attempting troubleshooting and collecting relevant information.

**Q5.**
What should a good support ticket *always* include?

> **Answer:** B) Full job script, error logs, and description of troubleshooting steps.

---

## **Next Steps**

* Practice troubleshooting job failures.
* Explore arcdocs for documentation.
* Practice drafting clear support tickets.
* Experiment with optimizing resource requests.

> **Pro Tip**: Systematic troubleshooting and good communication can drastically reduce time to resolve HPC issues.