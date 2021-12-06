# Configuring Rclone for surfdrive

Rclone has an interactive menu to generate a config file for you. You need to complete this menu once for each storage platform that you want to connect to. You can use Rclone for many different types of storage platforms: check the [homepage](https://rclone.org) for a list. SURFdrive and Research drive are based on ownCloud software. Platforms built on standard protocols such as WebDAV (including Yoda, Data Archive or the U: network drive of UU student and employees) are also supported but require slightly different configuration steps.

To check whether Rclone is available on your system:

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
3. Choose option `40`for WebDAV&#x20;
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

## Next: [Using rclone for data transfer](rclone-transferringdata.md)

