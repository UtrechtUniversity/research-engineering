# Cartesius - iRODS iCommands

### Instructions for synchronization of data on Yoda and HPC platforms

The instructions below describe the process of data transfer between Cartesius and Yoda using iCommands (recommended method). Transfer is also possible via the Webdav protocol using [Rclone](https://rclone.org). For large files iCommands is faster due to parallel transfer options.

### ** Steps**

1. Create a config file
2. Load Icommands
3. Initialize connection to iRODS
4. Submit Synchronization commands

Step 1 only needs to be performed once. The other steps need to be performed each session.

#### Step 1:  Create a config file (first time only)

You may need help from your datamanager or contact Yoda support to obtain the information needed for the config file.

From the home directory of your HPC system, go to the hidden directory called '.irods'

```
mkdir .irods
cd .irods
```

In this directory, create a file named `irods_environment.json`.

```
touch irods_environment.json
```

Edit the file using a text editor such as nano or vim (when working via e.g. MobaXterm it is possible to edit using e.g. notepad++, this may lead to end-of-line errors (EOL), see the troubleshooting section at the bottom of this page).

Go to the [yoda website](https://www.uu.nl/en/research/yoda/guide-to-yoda/i-am-using-yoda/using-icommands-for-large-datasets), scroll to **Step 2. Configuring iCommands** and copy and paste the text belonging to your institution in the file (similar to the file below) and change the email address next to `irods_user_name`to your yoda user name (typically your uu email address).

```
{   
"irods_host": "science.data.uu.nl",   
"irods_port": 1247,    "irods_home": "/nluu6p/home",   
"irods_user_name": "<exampleuser>@uu.nl",   
"irods_zone_name": "nluu6p",   
"irods_authentication_scheme": "pam",   
"irods_encryption_algorithm": "AES-256-CBC",   
"irods_encryption_key_size": 32,   
"irods_encryption_num_hash_rounds": 16,   
"irods_encryption_salt_size": 8,   
"irods_client_server_negotiation": "request_server_negotiation"
}
```

Save and close the file.

#### Step 2: Load icommands

Load the icommands each time you login to the system.&#x20;

```
module load 2020
module load "iRODS-iCommands/4.3.0"
```

#### Step 3: Initialize connection to iRODS

Initialize the connection to the Yoda system as follows:

```
iinit
```

Type your Yoda password when requested.

Type `ils` to see whether the connection has been established. The output of the `ils` command will list the current Yoda folder and the folders that are located within the current folder.

`iinit` and `ils` are commands which can only be used when icommands is active. Some of these commands are very similar to standard unix commands. When icommands is active you can still operate the HPC system using normal unix commands. A complete list of all 'icommands' can be found [here](https://docs.irods.org/4.2.9/icommands/user/).

#### Step 4: Transferring data

Standard transfer of a single file can be done using `iget` and `iput`

The `irsync` command can be used to synchronize entire directories between Yoda and Cartesius. The command is used as follows.

```
irsync <source> <destination>
```

In contrary to the `iput` and `iget` commands, in `irsync` it is necessary to put 'i:' before the Yoda path. The Yoda path (directly after 'i:') to the folder that should be synchronized should be relative to the **current** Yoda folder (check with `ils`).

E.g.

```
irsync -rKV i:myfolder /my_hpc_folder
```

`-rKV` means the following flags (options) are used:

* `-r` recursive - store the whole folder including subdirectories
* `-K` Calculate and verify the checksum on the data
* `-V` Very verbose

To synchronize in the opposite direction

```
irsync -rKV /my_hpc_folder/ i:myfolder
```

> note the '/' at the end of /my\_hpc\_folder/ that is not used in the command above.

### Parallel transfer

With icommands it is possible to transfer individual files in parallel using multiple threads, which results in higher transfer speeds. The flag `-N` can be used to control the number of parallel threads (only recommended in very specific situations). When not specified the server decides a default number of threads. For large files this number can be e.g. 16 threads.

```
irsync -rKV -N 0 i:myfolder /my_hpc_folder
```

### Job scripts

The above commands can be used in job scripts to transfer input data and scripts from Yoda directly to the scratch space of the worker that will be doing your tasks or to your home file system on Cartesius.  You can also use icommands to transfer (intermediate) output to Yoda during or at the end of a job.

Example jobscript:

```
#!/bin/bash
#SBATCH --time=00:00:10
#SBATCH -N 1
#SBATCH --tasks-per-node 16

echo "/bin/date: Start "

# Loading modules
module load 2020 
module load icommands

# Transfer input files to scratch
echo "/bin/date: Transferring input files to scratch"
mkdir "$TMPDIR"/input
mkdir "$TMPDIR"/output
irsync -rv i:my_yoda_input_directory "$TMPDIR"/input

# Compute tasks
echo "test" > "$TMPDIR"/output/filename.txt

# Transfer input files to scratch
irsync -rKv "$TMPDIR"/output/ i:my_yoda_output_directory

echo "/bin/date: End "
```

``

