# Session 4: Modules and Software

In this session we will learn about software on Aire, and how to access software via the module system. We will also discuss some alternatives to install software yourself on the system.

`````{tab-set}
````{tab-item} 1. Intro

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

````{tab-item} 2. Loading modules
- **Loading a Module:**
  - To use a software package, load its module.
  - Example:
    ```bash
    module load python/3.13.0
    ```
    - What happens if you do not specify a version?

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

````

````{tab-item} 2.1 Ex.

:::: exercise
**Exercise 2:** Load the `gcc` module and verify it's loaded.
::::

- Note: there are more than one version of gcc available. What is the behaviour if you do not specify a version?
  - How should you load a module when running jobs to ensure reproducibility?

:::: solution
```bash
#!/bin/bash

module load gcc
module list

```
::::
````

````{tab-item} 3. Using modules

In the previous exercise, we saw how to write a bash script that loads a particular module.

Say we have a Python file called `hello_world.py`:

```python
print("hello world!")
```

How would we write a bash script that loads the Python module and runs the Python script?

````

````{tab-item} 3.1 Soln.

We want to write a bash script that runs the following Python script (`hello_world.py`):

```python
print("hello world!")
```

A simple script could look like this:

```bash
#!/bin/bash

module load python/3.13.0
python hello_world.py

```

After creating this file, called `python_test.sh` you will need to add executable permissions to it: `chmod +x python_test.sh`. Use `ls -F` to check that it is executable.

To run the script:

```bash
./python_test.sh
```

Note: when running Python jobs, we would recommend that instead of using the basic Python install, to instead use the Miniforge module to create a conda environment. [Read our documentation on how to set this up](https://arcdocs.leeds.ac.uk/aire/usage/dependency_management.html)

````

````{tab-item} 4. Requesting New Modules
- **Centralized Management:**
  - Popular software is centrally installed by the Research Computing team.
  - Ensures optimized performance and avoids conflicts.

- **Requesting New Software:**
  - If a required software is not available, users can request its installation.
  - Submit a Research Computing Query with details about the software.

:::: exercise
**After the course:** Identify a software you need that's not available and draft a request. Can you find the Research Computing Query form via the IT website? Try to locate it through the IT website (*Home > Research IT > Research IT Query*), but if you're stuck, [here's a link to the request form](https://bit.ly/arc-help)
::::

:::: solution
Provide the software name, version, and a brief justification for its use in your research.
::::
````

````{tab-item} 5. Alternative Software Management

You also can manage your own software on Aire via a number of different routes. Many users will not need to do this; but it may be neccessary if you want fine-grained control and need to use older versions of software than are available via the module system.

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
**After this course:** Choose a simple software package and outline steps to install it using Spack.
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
  - When using software like R or Python, use the Miniforge module to create a conda environment; [read our documentation on how to set this up](https://arcdocs.leeds.ac.uk/aire/usage/dependency_management.html)

- **Reproducibility:**
  - Specify the version of the module you want to load; the default modules may change as new versions of software are released.
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
module load gcc/14.2.0
module load python/3.13.0
# Add other module load commands as needed
```
::::
````
`````