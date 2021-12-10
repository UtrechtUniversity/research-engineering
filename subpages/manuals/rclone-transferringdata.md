## Using Rclone

For all Rclone commands see: [https://rclone.org/commands/](https://rclone.org/commands/)
Below are the most useful commands:
- lsd (listing directories)
- ls (listing files)
- copy (copying individual files)
- sync (copying entire folders)

### rclone lsd

The `lsd` commands is used to list all directories in the current directory.
e.g. to list all directories in your surfdrive type:

```
rclone lsd surfdrive:
```
e.g. to list all directories in a specific subfolder of a folder on surfdrive type:

```
rclone lsd surfdrive:myfolder/mysubfolder
```

### rclone ls

The `ls` commands is used to list all files in the current directory.
e.g. to list all files in a specific folder in your surfdrive type:

```
rclone ls surfdrive:myfolder
```

### rclone copy

To copy a file from SURFdrive to a certain folder:

```
rclone copy surfdrive:file.txt ./my_destination_folder
```

### rclone sync

To synchronize an entire folder from surfdrive use `rclone sync`:

> Warning! Synchronization with Rclone makes the destination folder equal to the source folder and deletes files and folders in the destination folder that are not present in the source folder. Therefore it is wise to use the --dry-run flag to see what will be copied and deleted before actually running the command. `rclone sync <source> <destination> --dry-run`

```
rclone sync surfdrive:my_source_folder ./my_destination_folder --dry-run
rclone sync surfdrive:my_source_folder ./my_destination_folder -cPv
```

`-cPv` means the following flags (options) are used:\
&#x20;`-c` skip files that are already present (compared using checksums)\
&#x20;`-P` report progress of transfer\
&#x20;`-v` verbose; increase the amount of information in the logs

Further reading:
- For all Rclone commands see: [https://rclone.org/commands/](https://rclone.org/commands/)  
- When using rclone on Snellius or Lisa, read: [how to use rclone in jobscripts](rclone-jobscript.md)
