# Install Rclone

The steps below are used to install Rclone on Research Cloud.

## Step 1: Make sure you are in a terminal
- In Jupyterhub (Jupyter Workspace in Research cloud): 
  To open a new terminal, click the + button in the file browser and select the terminal in the new Launcher tab
  ([find a short video here](https://jupyterlab.readthedocs.io/en/stable/user/terminal.html)).
- In Rstudio (Rstudio workspace in Research cloud):
  In the bottom left panel, click the 'terminal' tab.

## Step 2: Download and install rclone

Type the following command in the terminal:

`curl https://rclone.org/install.sh | sudo bash`

## Step 3: Check if installation was successful

Type the following command. A version number will be displayed when installation was successful.

```
rclone version
```


## Next: [Configure Rclone](config-rclone.md)
