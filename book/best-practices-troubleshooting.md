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

### 🔍 Troubleshooting Tips

* **Always check the `.err` file** first — many runtime errors are logged there.
* **Google Smartly**: Put error messages in **quotes** to search exact phrases.
* **Save working job scripts** — version control isn't just for code.

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

  * [Aire HPC Documentation](https://arcdocs.leeds.ac.uk/aire)
  * Search with clear keywords (e.g., "job submission error", "SLURM memory limit").

* **Google**:

  * Copy error messages *verbatim* into search.
  * Use quotes for exact matches.

**Example**:

> Error: `srun: error: Unable to allocate resources: Requested node configuration is not available`
> Google: `"srun: error: Unable to allocate resources: Requested node configuration is not available" HPC SLURM`

---

### **Submitting a Support Ticket**

* Only escalate if you have attempted basic troubleshooting.

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


## **Recap Quiz**

**Q1.**
What is the first step you should take when your HPC job fails?

> **Answer:** C) Read the error message and check logs.

**Q2.**
Where can you find official documentation for the Aire HPC system?

> **Answer:** B) arcdocs

**Q3.**
When is it appropriate to submit a support ticket?

> **Answer:** B) After attempting troubleshooting and collecting relevant information.

**Q4.**
What should a good support ticket *always* include?

> **Answer:** B) Full job script, error logs, and description of troubleshooting steps.

---

## **Next Steps**

* Practice troubleshooting job failures.
* Explore arcdocs for documentation.
* Practice drafting clear support tickets.
* Experiment with optimizing resource requests.

> **Pro Tip**: Systematic troubleshooting and good communication can drastically reduce time to resolve HPC issues.