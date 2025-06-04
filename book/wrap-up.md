
# Session 7: Wrap Up

In this final session, we’ll consolidate what you’ve learned throughout the course and point you towards further resources to continue developing your skills with Aire and HPC more generally.

---

## Recap of Key Concepts

Let’s quickly revisit the main topics covered in this course:

* **What is HPC?**

  * High Performance Computing allows multiple processors (cores) to work together to solve large problems faster.
  * HPC clusters like **Aire** are built from many nodes, each with powerful CPUs (and sometimes GPUs).
  * Parallelism is key to exploiting HPC systems effectively.

* **Logging on and Linux Basics**

  * Access Aire through SSH, either directly from the campus network or via VPN/jumphost if off-campus.
  * Linux command-line skills are critical for interacting with the system.
  * Key commands: `ls`, `cd`, `pwd`, `cp`, `wget`, `rm`.

* **Storage on Aire**

  * Understand storage types: **home**, **scratch**, and **shared storage**.
  * Efficiently move data and check quotas.

* **Modules and Software**

  * Software is accessed through **modules**.
  * Learned to load, list, swap modules.
  * Other strategies: Spack, EasyBuild, self-build, containers.

* **Job Scheduling and Batch Jobs**

  * **SLURM** scheduler manages all jobs on Aire.
  * Wrote batch scripts, submitted jobs, monitored queues.
  * Job states: `PENDING`, `RUNNING`, `COMPLETED`.
  * Hands-on practice with interactive sessions and task arrays.

* **Best Practices and Troubleshooting**

  * Diagnosing job errors, optimizing resource requests.
  * Start small (test jobs), scale up once validated.
  * Using arcdocs, Google, and support tickets for help.

---

## Further Guidance and Next Steps

### Explore Advanced Topics

* **Parallel Programming:** MPI, OpenMP, GPU computing.
* **Workflow Management:** Snakemake, Nextflow, job dependencies.
* **Performance Optimization:** Profiling, benchmarking.

### Recommended Resources

* [ARC Documentation Portal](https://arcdocs.leeds.ac.uk/aire/welcome.html)
* [HPC Carpentry Lessons](https://carpentries-incubator.github.io/hpc-intro/)
* [High Performance Python Course](https://arc.leeds.ac.uk/courses/swd6-high-performance-python/)

### Stay Connected

* [Research Computing Training Calendar](https://arc.leeds.ac.uk/courses/)
* [Submit a Support Ticket](https://it.leeds.ac.uk) if you need assistance.

---

## Final Q\&A / Discussion

Use this time to:

* Ask any outstanding questions.
* Share challenges you encountered and solutions you found.
* Request demos or deeper dives into specific topics.
* Discuss how to apply HPC in your research area.

### Q\&A Prompts

* *What was the most challenging part ?*
* *What would you like to use Aire for in your workflows?*
* *Is there a particular software package you'd like to or using?*
* *Any lingering questions?*

---

## Bonus Tips for Continuing Success

* **Start small**: Always begin with a minimal test case to confirm your setup before scaling up.
* **Version control your scripts**: Use Git or another version control system to track your job scripts and code.
* **Resource efficiency**: Request only what you need — excessive resource requests can delay your job.
* **Learn by doing**: Schedule time to practice; the more batch scripts you write, the more confident you'll become.
* **Stay informed**: Subscribe to mailing lists or notifications from the HPC service for updates and downtime notices.

\:::

---

## **Recap Quiz**

**Q1.**
What is the main advantage of using a HPC system like Aire?

* A) Larger storage capacity
* B) Faster problem solving through parallel processing
* C) Access to free software licenses
* D) Automatic data backups

> **Answer:**
> **B) Faster problem solving through parallel processing**

---

**Q2.**
Which command would you use to submit a batch job on Aire?

* A) `srun`
* B) `ssh`
* C) `sbatch`
* D) `scp`

> **Answer:**
> **C) sbatch**

---

**Q3.**
Where should large, temporary files for active computations be stored?

* A) Home directory
* B) Scratch storage
* C) Admin node
* D) Login node

> **Answer:**
> **B) Scratch storage**

---

**Q4.**
If your job is stuck in `PENDING` state, what might be the cause?

* A) Syntax error in your script
* B) Not enough available resources
* C) The login node is overloaded
* D) You forgot to save the output

> **Answer:**
> **B) Not enough available resources**

---

**Q5.**
What’s a good first step if your job fails with an unknown error?

* A) Immediately resubmit it
* B) Open a support ticket
* C) Check the error log and output files
* D) Assume the cluster is broken

> **Answer:**
> **C) Check the error log and output files**

---

## Where to Go Next: Mini-Roadmap

Here’s how you can continue your HPC journey:

| **Step**                  | **What to Do**                                                        |
| ------------------------- | --------------------------------------------------------------------- |
| **1. Practice**           | Regularly submit small jobs, explore module loading, refine scripts.  |
| **2. Advanced Courses**   | Take courses on parallel programming (MPI/OpenMP) or HPC Python.      |
| **3. Optimize**           | Learn profiling and benchmarking tools to make your code faster.      |
| **4. Real Projects**      | Apply your skills to real research problems, scale up carefully.      |
| **5. Community**          | Join research computing forums, HPC mailing lists, and attend events. |
| **6. Mentorship/Support** | Reach out to HPC support teams early if stuck — avoid wasting cycles. |

\:::{admonition} Final Tip
The best way to learn HPC is by *doing*. Start small, break things, fix them, and gradually scale up your work. Every issue you encounter is a learning opportunity.
\:::
