---
description: >-
  Rclone is a command line program to manage files on cloud storage. It can be
  used to copy or synchronize files between a cloud storage platform (e.g.
  SURFdrive) and an HPC cluster (e.g. Cartesius).
---

# Cartesius - Rclone

This manual aims to help setup Rclone for data transfer between SURFdrive and Cartesius. The assumptions is that the reader has some experience with HPC computing and knows how to [submit jobscripts](https://userinfo.surfsara.nl/systems/cartesius/getting-started#submitting).

Steps:

1. Create a config file
2. Transfer files
3. Use Rclone in jobscripts

## Step 1. Create an Rclone config file

Rclone has an interactive menu to generate a config file for you. You need to complete this menu once for each storage platform that you want to connect to. You can use Rclone for many different types of storage platforms: check the [homepage](https://rclone.org) for a list. SURFdrive and Research drive are based on ownCloud software. Platforms built on standard protocols such as WebDAV (including Yoda, Data Archive or the U: network drive of UU student and employees) are also supported.

To check whether Rclone is available on your system:&#x20;

```
rclone version
```

You should see a version number displayed in the terminal.

To start the interactive menu:

```
rclone config
```

Now walk through the questions. For SURFdrive you need the following information (Rclone version 1.55.1)

1. Type `n` for New remote
2. For 'name' choose e.g. `surfdrive` (or SD)
3. Choose option `37`for WebDAV&#x20;
4. Fill in the URL. You can look this up via the web portal of SURFdrive. In the top right corner click your account name. Go to settings. In the left panel, click security. At the very bottom of the page there will be a section WebDAV passwords. There is also an URL, something like: [https://surfdrive.surf.nl/files/remote.php/nonshib-webdav](https://surfdrive.surf.nl/files/remote.php/nonshib-webdav)\
   &#x20;Use this URL.
5. Before you continue with the Rclone menu, first perform the following step via the [web portal](https://surfdrive.surf.nl) of SURFdrive. Fill in an app name (e.g. lisa) on the security settings page (same page as previous step) and click “create new app password”. The page will show a username and a password. You will need these in the following steps of the Rclone menu.
6. Select the type of WebDAV storage system. Type: `2` for ownCloud.
7. Type in your user name from SURFdrive (this is the username found in the web portal (step 5))
8. Select: y) Yes type in my own password
9. Type in your password from the web portal and type it in again for confirmation.
10. Skip the Bearer token option by pressing enter.
11. Choose `n` to skip the advanced config
12. Confirm your settings by typing `y`.
13. Choose `q` to quit the menu.

Test whether everything is setup correctly by typing:

```
rclone lsd surfdrive:
```

Change `surfdrive:` to any other name chosen in step 2.

If everything is setup correctly, you should see a list of files and folders that are present on your SURFdrive.

## Step 2: Transfer files

For all Rclone commands see: [https://rclone.org/commands/](https://rclone.org/commands/)

To copy a file from SURFdrive to a certain folder on Cartesius:

```
rclone copy surfdrive:file.txt ./my_destination_folder
```

To synchronize an entire folder use `rclone sync`:

> Warning! Synchronization with Rclone makes the destination folder equal to the source folder and deletes files and folders in the destination folder that are not present in the source folder. Therefore it is wise to use the --dry-run flag to see what will be copied and deleted before actually running the command. `rclone sync <source> <destination> --dry-run`

```
rclone sync surfdrive:my_source_folder ./my_destination_folder -cPv
```

`-cPv` means the following flags (options) are used:\
&#x20;`-c` skip files that are already present (compared using checksums)\
&#x20;`-P` report progress of transfer\
&#x20;`-v` verbose; increase the amount of information in the logs

## Step 3: Use Rclone in jobscripts

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

