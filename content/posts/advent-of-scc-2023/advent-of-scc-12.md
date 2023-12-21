---
title: "Advent of Supercomputing: Day 12"
date: 2023-12-12
publishDate: 2023-12-12T08:00:00Z
author: ["khai"]
draft: false
---

# `rclone`

In the basic sense rclone allows you to mount cloud drives and storage as if they were local storage. This page will guide you through setting this up for google drive since that is one of the cloud storage providers provided to UCSD students. The steps for Microsoft Onedrive are the same.

## setup
First we setup the name of the rclone remote storage.
```
$ rclone config
2023/12/08 22:54:04 NOTICE: Config file "/home/khaiv/.config/rclone/rclone.conf" not found - using defaults
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n

Enter name for new remote.
name> google-drive
```
Then we select the remote storage provider we're indending on using. For this example we're using google drive with an id of 18.
```
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
1 / 1Fichier
  \ (fichier)
2 / Akamai NetStorage
  \ (netstorage)
.
.
.
18 / Google Drive
   \ (drive)
.
.
.
34 / Microsoft OneDrive
   \ (onedrive)
.
.
.
55 / premiumize.me
   \ (premiumizeme)
56 / seafile
   \ (seafile)
Storage> 18
```
some client id stuff we can skip over.
```
Option client_id.
Google Application Client Id
Setting your own is recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
If you leave this blank, it will use an internal key which is low performance.
Enter a value. Press Enter to leave empty.
client_id>

Option client_secret.
OAuth Client Secret.
Leave blank normally.
Enter a value. Press Enter to leave empty.
client_secret>
```

selecting what permissions we have when accessing this drive
```
Option scope.
Comma separated list of scopes that rclone should use when requesting access from drive.
Choose a number from below, or type in your own value.
Press Enter to leave empty.
 1 / Full access all files, excluding Application Data Folder.
   \ (drive)
 2 / Read-only access to file metadata and file contents.
   \ (drive.readonly)
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ (drive.file)
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ (drive.appfolder)
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ (drive.metadata.readonly)
scope> 1
```
Some account stuff we can skip over
```
Option service_account_file.
Service Account Credentials JSON file path.
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Leading `~` will be expanded in the file name as will environment variables such as `${RCLONE_CONFIG_DIR}`.
Enter a value. Press Enter to leave empty.
service_account_file>

Edit advanced config?
y) Yes
n) No (default)
y/n> n
```
Authenticating in the browser.
```

Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.

y) Yes (default)
n) No
y/n> y
2023/12/08 22:56:58 NOTICE: If your browser doesn't open automatically go to the following link: http://127.0.0.1:
53682/auth?state=Q01Esj3uuxzKXu9eP6IM-w
2023/12/08 22:56:58 NOTICE: Log in and authorize rclone for access
2023/12/08 22:56:58 NOTICE: Waiting for code...
2023/12/08 22:57:08 NOTICE: Got code
Configure this as a Shared Drive (Team Drive)?

y) Yes
n) No (default)
y/n> n
```
Sucessful config and confirmation output.
```
Configuration complete.
Options:
- type: drive
- scope: drive
- token: {"access_token":"[REDACTED]","token_type":"Bearer","refresh_token":"[REDACTED]","expiry":"[REDACTED DATE]"}
- root_folder_id:
```
Confirmation
```
Keep this "google-drive" remote?
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
google-drive         drive
e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

## mounting
To mount rather than using the "`mount`" command we use "`rclone --vfs-cache-mode writes mount`" instead. Using the config we created earlier the command will be 
```
rclone --vfs-cache-mode writes mount google-drive: ~/GDrive
```
note that "`--vfs-cache-mode writes`" simply caches any files that are written to, you can use "`full`" instead of "`writes`" to also cache files that you read.
from here you can cd into "`~/GDrive`" to access files in your google drive. To unmount the remote drive the process is the same as unmounting a normal mount point being 
```
umount ~/GDrive
```
(since the mount is done in user space you wont need root unlike if you had used `mount`).

## transfering
If you noticed when we were configuring the google drive remote storage there was a file mentioned
```
2023/12/08 22:54:04 NOTICE: Config file "/home/khaiv/.config/rclone/rclone.conf"
```
With this config file you can simplly copy it to another system and not have to setup the rclone again on said system

## finalizing
Once this is all setup I generally use a script similar to 
```
if [ $(df | grep google-drive | wc -l) -eq 0 ]
then
  rclone --vfs-cache-mode writes mount google-drive: ~/GDrive & disown
else
  umount ~/GDrive
fi
```
to handle mounting and unmounting the remote drive. For more information check out the rclone [documentation](https://rclone.org/docs/)
