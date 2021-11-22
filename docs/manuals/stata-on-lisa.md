# Using Stata on Lisa

## Introduction
This manual has been designed to introduce U.S.E. researchers on how to use the LISA-SURFSARA HPC computing system for running analyses with STATA and how to run STATA do-files in particular. New users are suggested to follow [an introductory course to using a supercomputer](https://www.surf.nl/en/agenda/research-and-ict) and check [the official webpage of LISA](https://servicedesk.surfsara.nl/wiki/display/WIKI/Lisa) for up to date manuals of the Lisa system.

Using STATA or other software on the HPC is different from using a regular PC. The HPC system is operated using the command line. To do analyses in Stata you would have to create a script (do-file) that saves the output to a file. The script can be submitted as a 'Job' on the HPC and the results can be inspected when the job has finished. To be able to do all this requires some effort in trying to understand the following concepts:
- connecting to a remote computer
- operating a computer using linux commands
- transferring data
- submitting jobs

## Connecting to a remote computer

MobaXterm is a (Windows specific) free tool that can be used to connect to a remote machine via secure shell protocol (SSH). Similar tools are e.g. Putty and WinSCP. The main advantage of MobaXterm is that it is a bit more versatile. It supports different protocols and provides the user with a file browser during an SSH session.
Apple and Linux operating systems come with a 'Terminal' application that can be used instead of MobaXterm.

Visit the [website](https://mobaxterm.mobatek.net/) for a free download and documentation.

Check this site for instructions on [how to connect to Lisa](https://servicedesk.surfsara.nl/wiki/pages/viewpage.action?pageId=30660216).

## Operating a computer using the linux command line
Once you succeeded to login, you will see a dark screen with at the very bottom a dollar sign $. This is the command prompt or command line. Via this command prompt you will 'talk' to the remote system using linux commands. 

Check these course materials and exercises to learn the [basics of a linux command line](https://swcarpentry.github.io/shell-novice/).
Make sure you understand paths and navigation files and directorie

## Transferring data
It is wise to think of some folder structure before transferring files, and create the folders using linux commands. E.g. create separate input and output folders. 
Make sure you understand paths and navigating files and directories in linux (previous section) and make sure paths in your Stata do-files are pointing to the right location before transferring your Stata script.

Check [Transfer files between the HPC system and your PC](https://servicedesk.surfsara.nl/wiki/pages/viewpage.action?pageId=30660216) for instructions on how to transfer files. For an intuitive approach check the SFTP for MobaXterm users section.

## Submitting jobs
Once you have logged in to lisa, you are located on the login node. The login node is a computer where you manage your files and create job scripts. If you want to do something with Stata, you will have to submit a 'job'. Once you submit a job, it will enter the job queue and as soon as resources are available the job will be executed on a compute node. The output will be stored on the location that you specify in the do-file or the job script.

You submit a job by creating and running a job script, see:
https://servicedesk.surfsara.nl/wiki/display/WIKI/Creating+and+running+jobs



