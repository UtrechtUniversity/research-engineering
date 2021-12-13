---
layout: default
permalink: /manuals/rclone-jobscript/
---

## Using rclone in jobscripts on Snellius and Lisa

It is good practice to keep versions of your input and output files on one storage platform and sync them to the HPC platform at the start of a job to make sure the most recent versions are used. Use the scratch storage of the node for best performance. Check [Cartesius user documentation](https://userinfo.surfsara.nl/systems/cartesius/filesystems) for the difference between the scratch storage of a compute node and the user file system.

This can be done by incorporating data transfer in a job script as follows:

```
#!/bin/bash
#SBATCH --time=00:00:10
#SBATCH -N 1
#SBATCH --tasks-per-node 16

echo "/bin/date: Start "

# Loading modules


# Transfer input files to scratch
echo "/bin/date: Transferring input files to scratch"
mkdir "$TMPDIR"/input
mkdir "$TMPDIR"/output
rclone sync surfdrive:input "$TMPDIR"/input -cPv

# Compute tasks
echo "test" > "$TMPDIR"/output/filename.txt

# Transfer input files to scratch
rclone sync  "$TMPDIR"/output surfdrive:output -cPv

echo "/bin/date: End "
```
