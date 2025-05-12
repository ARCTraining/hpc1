# Session 3: Storage on Aire

In this session, we will work to understand the main file areas on Aire (home, scratch, etc.), learn the purpose and differences of each storage type, practice navigating between storage areas and checking disk quotas, and learn commands for copying and transferring data to/from Aire. 


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
````

````{tab-item} 1.1 Ex.

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

It's important for you to be able to check how much of your allocated quota you are using in a particular storage area, as exceeding this quota will cause your jobs to fail.

- `quota`
  - This command tells you your disk usage and limits. By default, the unit is kbytes or "blocks". You can add the `-s` flag to make the output "human readable", showing units after the values (`quota -s`).
  - If the output is difficult to read, try resizing your terminal windows to accomodate the table.
- `du`
  - This command reports the amount of disk space used by different files and directories, by default reporting in 512-byte blocks.
  - You can provide the path to a specific directory to focus the results.
  - Add the flag `-h` to make the output human readable.
  - Add the flag `-s` to summarise the results (and not recursively provide results for subdirectories).
  - `du -hs *` will provide you with the space taken up by the data in each directory. This can be a little slow!

````

````{tab-item} 3.3 Ex.

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

````{tab-item} 4.4 Ex.

::: important  
**Exercise:** Navigate around the file system using `cd directory_path`, and `cd` to return home.

- Try to use the environment variables (like `$SCRATCH`)
- Use `pwd` to check where you are in the directory structure, and `ls` to see what other files are present

:::

````

````{tab-item} 5. Transferring Files

- **scp** (secure copy) to/from the login node. For example, from **your local machine** to Aire:  
  ```bash
  scp myfile.txt <username>@target-system:$SCRATCH/
  ```  
  And to copy a file **from** Aire to local (current directory):  
  ```bash
  scp <username>@target-system:/path/results.txt .
  ```  
- **rsync** for efficient transfers (especially many files). Example:  
  ```bash
  rsync -avh data/ <username>@target-system:path/data/
  ``` 

- **wget/curl** on the login node to download from a URL. For instance:  
  ```bash
  wget https://example.com/data.zip
  ```  
- **GUI/SFTP clients:** Use FileZilla, WinSCP or Cyberduck to connect to Aire via SFTP.  
- **Best practice:** Transfer all needed input data to `$SCRATCH` *before* running jobs, and copy results out of `$SCRATCH` after jobs finish.

````

````{tab-item} 5.1 Transferring Files

- From off campus, you will have to use the usual tunnelling/jumphost commands.
  - rsync:
    ```bash
    rsync -r --info=progress2 -e 'ssh -J e<username>@jump-host' file_to_copy.txt <username>@target-system:path/
    ```
    - `--info=progress2` gives you a progress bar for the upload/download
    - `-e ssh` allows you to set up a remote ssh shell (to use the jumphost)
  - scp:
    ```bash
    scp -rq -J <username>@jump-host <username>@target-system:path/file-to-copy.txt local-folder-path/
    ```
    ```bash
    scp -rq -J <username>@jump-host local-folder-path/file-to-copy.txt <username>@target-system:path/
    ```
````

````{tab-item} 5.2 Ex.

::: important  
**Exercise:** On your **local machine**, write the `scp` command to download `results.txt` from your home directory on Aire into your current local directory.  
:::

::: tip  
**Solution:** Use the syntax:  
```bash
scp <username>@target-system:/users/yourname/results.txt .
```  
:::
````

````{tab-item} 6. Summary
- **Multiple storage areas:** Aire provides different file systems: home (`$HOME`), scratch (`$SCRATCH`), NVMe flash, and node-local. Use each appropriately.  
- **Navigation:** Use `cd`, `ls`, etc., along with environment variables.  
- **Check usage:** Regularly run `quota -s` to see your space and file usage. Clean up old files from `$SCRATCH`.  
- **Data transfer:** Use `scp` or `rsync` via the login nodes to move data.  
- **Avoid data loss:** Remember that scratch and flash are *temporary*. Always **backup important data** to home or external storage when done. 

Read up on [best practises here](https://arcdocs.leeds.ac.uk/aire/usage/file_data_management/best_practices.html).

::: note  
**Tip:** Keep your home directory organized. Place large data in scratch only while needed. Archive or delete old files in scratch after use.  
:::
````
`````


You can read more about the [different storage options on Aire in our documentation](https://arcdocs.leeds.ac.uk/aire/system/storage_filesystem.html#summary-of-storage-types), and find [detailed guidance on organising your files and research data in our data management section](https://arcdocs.leeds.ac.uk/aire/usage/file_data_management/start.html).