# Session 2: Logging-on and Linux recap

In this session, we will get connected to Aire. This section is modified from the [documentation pages for Aire](https://arcdocs.leeds.ac.uk/aire/getting_started/logging_on.html). Please note that a University of Leeds account is required to view the links below. 

## Logging on

The training facilitator will share their screen and take your through the steps required to log in, swapping from these notes to the KB articles linked below for the relevant session.

:::{note}
Note that the delivery of this session will depend whether it is hosted in-person or remotely.

To connect to the Aire platform, the steps vary depending on whether you are on the wired campus network or offsite/using Eduroam. If you are offsite, you must connect through the University’s SSH gateway or use the University VPN. However, if you are on the campus network, no additional steps are required. The instructions below are organized based on whether you are connecting from on-campus or off-campus.
:::

### General instructions

To access Aire, you simply need access to an [SSH (Secure SHell) client](https://en.wikipedia.org/wiki/Secure_Shell). This could be your terminal or Bash if you are using Linux or a Mac, or PowerShell or MobaXTerm if you are using Windows.

From a wired connection on campus, use the following command in your SSH client:

```bash
ssh <username>@target-system
```

From off-campus (or using a mchine on Eduroam as opposed to the wired network), just add in the provided SSH gateway/jumphost:

```bash
ssh <username>@target-system -J <username>@jump-host
```

If you need more information, please see the KB articles linked below.

### KB articles for connecting from campus

**For in-person sessions or for people attending remotely from an on-campus machine.**

You can connect to Aire and log in using SSH (Secure Shell) from within the University network.
Please see the following Knowledge Base articles for more information:

+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018276" target="_blank">KB0018276 - How do I connect to Aire from a Windows machine on campus?</a>
+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018275" target="_blank">KB0018275 - How do I log in to Aire on Linux or Mac OS from on Campus?</a>

Note that the above articles require you to log in with your University account to view.

### KB articles for connecting when off-site

**For online/remote sessions and connecting from a laptop not connected to the University ethernet.**

To access Aire from outside the University, you will need to connect through the University's general SSH gateway or use the University VPN, whereas access from the campus network does not require this additional step.
Please see the following Knowledge Base articles for more information:

+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018286" target="_blank">KB0018286 - How do I connect to Aire from a Windows machine off campus?</a>
+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018284" target="_blank">KB0018284 - How do I log in to Aire on Linux or Mac OS from off Campus?</a>

For more general guidance on using SSH and the VPN for remote access, please see the following Knowledge Base articles:

+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0014674" target="_blank">KB0014674 - SSH remote access at the University of Leeds</a>
+ <a href="https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0014410" target="_blank">KB0014410 - Connecting to the University VPN</a>

Note that the above articles require you to log in with your University account to view.

</br>

---

## Linux recap

::::{tab-set}

:::{tab-item} Command line interface (CLI)

Many large research machines (such as the HPC machines ARC4 and Aire here at Leeds) do not have a GUI (Graphical User Interface) and so you need to interact with them through a CLI.

- CLIs allow us to interact with a computer through text-based commands typed into the command-line.
- While GUIs can be simple and intuitive to use, they can make it difficult to reproduce workflows:
    - Sometimes you have to record by hand (or with a screen recording) what sub-options from different menus you used;
    - Updates to GUIs can make it difficult to find the same menu options;
    - A workflow with multiple steps can be tedious to repeat for multiple datasets (having to click through multiple layers of menu options for each dataset).

:::

:::{tab-item} Command line interface (CLI)

Bash is the default shell on Unix systems like Linux or Mac

https://arctraining.github.io/rc-slides/hpc1.html#/exercise-1.1

See our handy [Linux cheat sheet here](https://arctraining.github.io/rc-slides/linux101#/cheat-sheet).

:::

:::{tab-item} File systems on Linux

- Navigate to your home directory with `cd`
- List the files and directories present with `ls`
- Change to a different directory with `cd path/to/directory`

:::

:::{tab-item} Exercise

What do the following Linux commands do? How might they be used on the HPC service?

- `ls`
- `pwd`
- `mkdir`
- `cp`
- `wget`
- `rm`

:::

::::

:::{admonition} Become more familiar with Linux CLI

Please see our slides on [Introduction to Linux for HPC](https://arctraining.github.io/rc-slides/linux101#/section) for a more in-depth introduction to Linux commands.

:::