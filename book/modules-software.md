# Session 4: Modules and Software

In this session we will learn about software on Aire, and how to access software via the module system. We will also discuss some alternatives to install software yourself on the system.

`````{tab-set}
````{tab-item} Introduction to Modules
- **What are Modules?**
  - Modules are a way to manage different software environments on HPC systems.
  - They allow users to load and unload software packages dynamically.
  - This helps in managing different versions of software and their dependencies.

- **Why Use Modules?**
  - Simplifies the user environment.
  - Avoids conflicts between software versions.
  - Provides a consistent and reproducible environment.

:::: exercise
**Exercise 1:** List all available modules on Aire.
::::

:::: solution
```bash
module avail
```
::::
````

````{tab-item} Using Modules
- **Loading a Module:**
  - To use a software package, load its module.
  - Example:
    ```bash
    module load python/3.13.0
    ```

- **Unloading a Module:**
  - To remove a software package from your environment.
    ```bash
    module unload python/3.13.0
    ```

- **Listing Loaded Modules:**
  - To see which modules are currently loaded.
    ```bash
    module list
    ```

:::: exercise
**Exercise 2:** Load the `gcc` module and verify it's loaded.
::::

:::: solution
```bash
module load gcc
module list
```
::::
````

````{tab-item} Requesting New Modules
- **Centralized Management:**
  - Popular software is centrally installed by the Research Computing team.
  - Ensures optimized performance and avoids conflicts.

- **Requesting New Software:**
  - If a required software is not available, users can request its installation.
  - Submit a Research Computing Query with details about the software.

:::: exercise
**Exercise 3:** Identify a software you need that's not available and draft a request.
::::

:::: solution
Provide the software name, version, and a brief justification for its use in your research.
::::
````

````{tab-item} Alternative Software Management
- [**Spack:**](https://spack.io/)
  - A flexible package manager for HPC systems.
  - Allows users to install software without admin privileges.
  - Supports complex dependency management.

- [**EasyBuild:**](https://easybuild.io/)
  - Framework for building and installing software on HPC systems.
  - Automates the build process using configuration files.

- **Downloading and Self-Building:**
  - Users can manually download and compile software.
  - Requires knowledge of build systems and dependencies.

- **Containers:**
  - Encapsulate software environments using tools like [Apptainer](https://apptainer.org/).
  - Ensures portability and consistency across systems.

:::: exercise
**Exercise 4:** Choose a simple software package and outline steps to install it using Spack.
::::

:::: solution
1. Load Spack module: `module load spack`
2. Install software: `spack install htop`
3. Load the installed package: `spack load htop`
::::
````

````{tab-item} Best Practices
- **Environment Management:**
  - Use modules to manage software environments effectively.
  - Unload modules when they are no longer needed to avoid conflicts.

- **Reproducibility:**
  - Document the modules and versions used in your research.
  - Use containers for complex environments to ensure consistency.

- **Collaboration:**
  - Share module load commands with collaborators to replicate environments.
  - Use version-controlled scripts to manage workflows.

:::: exercise
**Exercise 5:** Create a script that loads necessary modules for your project.
::::

:::: solution
```bash
#!/bin/bash
module load gcc
module load python/3.8
# Add other module load commands as needed
```
::::
````
`````