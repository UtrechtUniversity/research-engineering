---
layout: default
permalink: /manuals/rsc-icommands/
---

## Installing and using iCommands on Surf Research Cloud

The instructions below describe the process of data transfer between Surf Research Cloud and Yoda using iCommands. This manual is developed for workspaces based on a Linux operating system (including the Jupyter Notebook and RStudio workspaces). Transfer is also possible via the Webdav protocol using [Rclone](https://rclone.org). For large files iCommands is faster due to parallel transfer options.

### Prerequisites
You will need some basic linux skills to be able to use the iCommands successfully. 
If you don't have these skills, take some time to practice using sections 1, 2, 3 and 7 of this short [online course](https://swcarpentry.github.io/shell-novice) before proceeding. You can practice in the terminal (see step 1).

### Step 1: Make sure you are in a terminal
- In Jupyterhub (Jupyter Workspace in Research cloud): 
  To open a new terminal, click the + button in the file browser and select the terminal in the new Launcher tab
  ([find a short video here](https://jupyterlab.readthedocs.io/en/stable/user/terminal.html)).
- In Rstudio (Rstudio workspace in Research cloud):
  In the bottom left panel, click the 'terminal' tab.

### Step 2: Install iRODS iCommands
Some workspaces have iRODS iCommands preconfigured. This should be visible in step 1 when creating a workspace.

Copy and paste (or type) the following commands in the terminal, or view the most up to date installation instructions for iRODS iCommands on the [yoda website](https://www.uu.nl/en/research/yoda/guide-to-yoda/i-am-using-yoda/using-icommands-for-large-datasets) in the section "Installing iCommands on Ubuntu". 

```
sudo apt-get update
sudo apt-get install -y --only-upgrade ca-certificates
wget -qO - https://packages.irods.org/irods-signing-key.asc | sudo apt-key add -
echo "deb [arch=amd64] https://packages.irods.org/apt/ bionic main" | sudo tee /etc/apt/sources.list.d/renci-irods.list
sudo apt-get update
sudo apt -y install irods-runtime=4.2.11-1~bionic irods-icommands=4.2.11-1~bionic

# Prevent automatic upgrades of these two packages
sudo apt-mark hold irods-runtime irods-icommands

# Network tuning, ad hoc and permanent
echo "120" | sudo tee /proc/sys/net/ipv4/tcp_keepalive_time
echo "30" | sudo tee /proc/sys/net/ipv4/tcp_keepalive_intvl
echo "net.ipv4.tcp_keepalive_time=120" | sudo tee -a /etc/sysctl.d/99-sysctl.conf
echo "net.ipv4.tcp_keepalive_intvl=30" | sudo tee -a /etc/sysctl.d/99-sysctl.conf
```

### Step 3: Create a config file

You may need help from your datamanager or contact Yoda support to obtain the information needed for the config file.

Go to the home directory and create a hidden directory called '.irods'

```
cd ~
mkdir .irods
cd .irods
```

In this directory, create a file named `irods_environment.json`.

```
touch irods_environment.json
```
Edit the file using a text editor such as nano or vim
```
nano irods_environment.json
```

Go to the [yoda website](https://www.uu.nl/en/research/yoda/guide-to-yoda/i-am-using-yoda/using-icommands-for-large-datasets), scroll to **Step 2. Configuring iCommands** and copy and paste the text belonging to your institution in the file (similar to the file below) and change the email address next to `irods_user_name`to your yoda user name (typically your uu email address).

```
{   
"irods_host": "science.data.uu.nl",   
"irods_port": 1247,    "irods_home": "/nluu6p/home",   
"irods_user_name": "exampleuser@uu.nl",   
"irods_zone_name": "nluu6p",   
"irods_authentication_scheme": "pam",   
"irods_encryption_algorithm": "AES-256-CBC",   
"irods_encryption_key_size": 32,   
"irods_encryption_num_hash_rounds": 16,   
"irods_encryption_salt_size": 8,   
"irods_client_server_negotiation": "request_server_negotiation"
}
```

### Step 4: Initialize connection to iRODS

Initialize the connection to the Yoda system as follows:

```
iinit
```

Type your Yoda password when requested.

Type `ils` to see whether the connection has been established. The output of the `ils` command will list the current Yoda folder and the folders that are located within the current folder.

`iinit` and `ils` are commands which can only be used when icommands is active. Some of these commands are very similar to standard unix commands. When icommands is active you can still operate the HPC system using normal unix commands. A complete list of all 'icommands' can be found [here](https://docs.irods.org/4.2.9/icommands/user/).

### Step 5: Transferring data

Standard transfer of a single file can be done using `iget` and `iput`

The `irsync` command can be used to synchronize entire directories between Yoda and Cartesius. The command is used as follows.

```
irsync <source> <destination>
```

In contrary to the `iput` and `iget` commands, in `irsync` it is necessary to put 'i:' before the Yoda path. The Yoda path (directly after 'i:') to the folder that should be synchronized should be relative to the **current** Yoda folder (check with `ils`).

E.g.

```
irsync -rKV i:myfolder /my_researchcloud_folder
```

`-rKV` means the following flags (options) are used:

* `-r` recursive - store the whole folder including subdirectories
* `-K` Calculate and verify the checksum on the data
* `-V` Very verbose

To synchronize in the opposite direction

```
irsync -rKV /my_researchcloud_folder/ i:myfolder
```

> note the '/' at the end of /my\_researchcloud\_folder/ that is not used in the command above.

### Parallel transfer

With icommands it is possible to transfer individual files in parallel using multiple threads, which results in higher transfer speeds. The flag `-N` can be used to control the number of parallel threads (only recommended in very specific situations). When not specified the server decides a default number of threads. For large files this number can be e.g. 16 threads.

```
irsync -rKV -N 0 i:myfolder /my_researchcloud_folder
```
