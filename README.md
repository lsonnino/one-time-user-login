# One-Time users

Create and manage a temporary user account on a Linux server and delete it at the end of the day.

<u>note</u>: requires *root* permissions



## Installation

Make the `ot-users` file executable then move it in `/usr/share/bin`.

```bash
chmod +x ot-users
sudo mv ot_user /usr/bin/
sudo chown root:root /usr/bin/ot-users
```

Add the `clear` task in the crontab to be executed daily:

```bash
sudo crontab -e
```

then add the following line at the end of the file: 

```bash
@daily ot-user clear
```



## Config

After the command is launched once (even an invalid one, see *Usage* here below), two files and a folder will be created:

1. The config file in `/etc/ot-user.config` containing numerous options
2. A file `/root/temporary-users.txt` where each line contains the username of a temporary user that will be deleted when the `clear` command is launched
3. A folder `/home/temporary` where all the home folders of all the temporary users will be located

A new user group will also be created containing the temporary users: `temporaryuser`.

All of those can be changed in the config file (except for the config file itself).



## Usage

The `ot-user` command follows the following pattern:

```bash
ot-user [command]
```

Currently, there are four supported commands. Any other command (or no command at all) will display the help message.

* `clear`: delete all the temporary users
* `create`: create a new temporary user and display it's username and password
* `help`: displays the help message
* `list`: list all the created temporary users



## Uninstall

Currently, no uninstall procedures exists except the manual one:

```bash
# Remove all the temporary users
sudo ot-user clear

# Remove the executable
sudo /usr/bin/ot-user
# Remove the config file and other automatically created files
sudo rm /etc/ot-user.config
sudo rm /root/temporary-users.txt

# Remove the folder containing the user's home folder
sudo rm -R /home/temporary

# Remove the user group containing the temporary users
sudo groupdel temporaryuser
```

<u>note</u>: the code here above is to be adapted with the actual files/folders path. The code above considers the default paths.



## Important note

### Changing the temporary users file

Every day, the crontab starts the `ot-user clear` command to remove the temporary users. The username of those users have to be stored in the file described by the config file, one username per line. If the path to that file changes, do not forget to paste the content of the old temporary users file into the new one.

### Temporary users permissions

Temporary users use `rbash` which prevent from cd-ing out of their home directory but read permissions are still allowed.

