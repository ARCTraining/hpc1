# Session 3: Storage on Aire

::: note  
**Objectives:** Understand the main file areas on Aire (home, scratch, etc.), learn the purpose and differences of each storage type, practice navigating between storage areas and checking disk quotas, and learn commands for copying and transferring data to/from Aire.  
:::

`````{tab-set}
````{tab-item} 1. Structure
- The Aire HPC file system includes several special directories:  
  - **Home directory** (`/users/<username>`, env var `$HOME`) for personal files.  
  - **Scratch directory** (`/mnt/scratch/<username>`, env var `$SCRATCH`) for large, temporary data.  
  - **Flash (NVMe) scratch** (`$TMP_SHARED`, usually `/mnt/flash/tmp/job.<JOB-ID>`) for very fast I/O during jobs.  
  - **Node-local scratch** (`$TMP_LOCAL` or `$TMPDIR`, typically `/tmp/job.<JOB-ID>`) for fast local storage on each compute node.  
- Symlinks exist: `/scratch` → `/mnt/scratch`, and `/flash` → `/mnt/flash`.  
- Home is backed up and not automatically purged; scratch and flash are not backed up and flash is deleted after each job.  
- **Always** copy important results from temporary storage (flash or node-local) back to your home directory or another permanent area **before** the job ends.


::: important  
**Exercise:** On the Aire login node, display the values of `$HOME` and `$SCRATCH` using `echo`.  
:::

::: tip  
**Solution:** For example, run:  
```
$ echo $HOME
/users/yourusername
$ echo $SCRATCH
/mnt/scratch/yourusername
```  
:::
````

````{tab-item} 2. Storage Types
- **Home (`$HOME`)**: Quota ≈30 GB and 1,000,000 files. Backup enabled and not deleted automatically. Best for small, persistent files (scripts, documents, configs).  
- **Scratch (`$SCRATCH`)**: Quota ≈1 TB and 500,000 files. **No backups**. Not deleted automatically, but you must clean up manually. Use for large datasets and intermediate job data.  
- **Flash (`$TMP_SHARED`)**: Quota ≈1 TB per job, no backup, **auto-deleted** when the job completes. Very fast NVMe storage ideal for I/O-intensive tasks during the job.  
- **Local scratch (`$TMP_LOCAL`, `$TMPDIR`)**: Node-specific quota. No backup, **auto-deleted** after job completion. Best for very fast, node-local storage during single-node jobs.  
- **Important:** Data in scratch/flash areas is temporary. These are *not backed up* and may be purged. Always move important data to your home or external storage when done.  

````

````{tab-item} 3. Quotas

::: important  
**Exercise:** Check your current disk usage and quotas. *(Hint: use the `quota` command.)*  
:::

::: tip  
**Solution:** Run `quota -s`. For example:
```
$ quota -s
Disk quotas for user yourusername (uid 12345):
     Filesystem   blocks   quota   limit   grace   files   quota   limit
      /users     15000*   30000    33000            200000 1000000 1100000
   /mnt/scratch 100000   1000000 1100000           500000 1500000 1650000
```
:::
````

````{tab-item} 4. Navigating
- Use standard Linux shell commands: `cd`, `ls`, `pwd`, etc.  
- To go to your home directory: `cd $HOME` or simply `cd` or `cd ~`. To go to scratch: `cd $SCRATCH`.  
- List contents with `ls`: e.g. `ls $HOME` or `ls $SCRATCH`. Use `pwd` to show the current directory path.  
- Use environment variables (`$HOME`, `$SCRATCH`, etc.) in commands. For example, `ls $HOME/projects` lists your project folder in home.  
- Copy files between areas on Aire with `cp`. For example: `cp data.txt $SCRATCH/` to copy **to** scratch, or `cp $SCRATCH/output.dat $HOME/results/` to copy **back** to home.  
- Remove unneeded files with `rm` to free space. After cleanup, re-check your usage with `quota -s` or `du` as needed.  
````

````{tab-item} 5. Transferring Files

- **scp** (secure copy) to/from the login node. For example, from **your local machine** to Aire:  
  ```bash
  scp myfile.txt <username>@target-system:$SCRATCH/
  ```  
  And to copy a file **from** Aire to local:  
  ```bash
  scp <username>@target-system:/users/yourname/results.txt .
  ```  
- **rsync** for efficient transfers (especially many files). Example:  
  ```bash
  rsync -avh data/ <username>@target-system:$HOME/data/
  ```  
- **wget/curl** on the login node to download from a URL. For instance:  
  ```bash
  wget https://example.com/data.zip
  ```  
- **GUI/SFTP clients:** Use FileZilla, WinSCP or Cyberduck to connect to Aire via SFTP.  
- **Best practice:** Transfer all needed input data to `$SCRATCH` *before* running jobs, and copy results out of `$SCRATCH` after jobs finish.

::: important  
**Exercise:** On your **local machine**, write the `scp` command to download `results.txt` from your home directory on Aire into your current local directory.  
:::

::: tip  
**Solution:** Use the syntax:  
```bash
scp username@aire-login.leeds.ac.uk:/users/yourname/results.txt .
```  
:::
````

````{tab-item} 6. Summary
- **Multiple storage areas:** Aire provides different file systems: home (`$HOME`), scratch (`$SCRATCH`), NVMe flash, and node-local. Use each appropriately.  
- **Navigation:** Use `cd`, `ls`, etc., along with environment variables.  
- **Check usage:** Regularly run `quota -s` to see your space and file usage. Clean up old files from `$SCRATCH`.  
- **Data transfer:** Use `scp` or `rsync` via the login nodes to move data.  
- **Avoid data loss:** Remember that scratch and flash are *temporary*. Always **backup important data** to home or external storage when done.  

::: note  
**Tip:** Keep your home directory organized. Place large data in scratch only while needed. Archive or delete old files in scratch after use.  
:::
````
`````
