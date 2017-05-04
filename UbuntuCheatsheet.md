# Ubuntu Cheat Sheet

#### Purpose: Repository of common Ubuntu or Bash tasks

- - -

## Transferring Files

### Upload a File from Local to Server

From local, Run:

```bash
rsync -chavzP -e 'ssh -p 22' --stats file.tar.gz [username]@[server]:~/remote/destination
```

### Download a File from a Public Host

```bash
wget http://www.example.com/path/to/file.txt
```

### Get a File from Server to Local

From local, Run:

```bash
scp -P 22 [username]@[server]:~/remote/location /Users/jsample/Desktop/destination
```

### Send File from Server to Server

From server hosting the file, Run:

```bash
scp -P 22 file.tar.gz [username]@[server]:~/remote/destination
```


### Tar Gzip Directory on Server

Tar: create (c) an archive from the files in directory (tar is recursive by default), compress it using the gzip (z) algorithm, store the output as a file (f) named archive.tar.gz, and verbosely (v) list all the files it adds to the archive. The '.' at the end saves hidden files as well in the tar.

From /home/clients, Run:

```bash
tar -zcvf archive.tar.gz example/.
```


### Un-Tar Gzip File on Server

Use x (extract), v (verbose), z (decompress archive using gzip), f (file path - must be last flag)

```bash
tar -xvzf archive.tar.gz
```


- - -


## Databases


### Dump/Export a MySQL Database on Server

Exports a dump ‘export.sql’ of ‘database_name' using ‘db_user’. Run this where the dump should be saved.

```bash
mysqldump -u db_user -p database_name > export.sql
```


### Import a MySQL Database on Server

Imports a sql dump ‘export.sql’ to a ‘database_name’ user ‘db_user'. Run this from the directory containing the sql dump.

```bash
mysql -u db_user -p -h localhost database_name < export.sql
```


### Add User to Database w/ Full Privileges

```bash
mysql -h localhost -u db_user -p
create database if not exists db_name;
use db_name;
create user 'new_db_user'@'%' identified by 'new_db_user_password';
grant all privileges on db_name.* to 'new_db_user'@'%';
```


- - -


## Searching


### Search for a File on the Server

Find all filenames that begin with "google"

```bash
find /var/www/example/ -name "google*"
```

Find a specific file on server

```bash
find / -name "my_file.txt" -type f
```


### Search for a Directory on the Server

Search in / for a directory named “my_directory_name"

```bash
find / -name my_directory_name -type d
```


### Search for a String in a File(s) on the Server

```bash
grep 'example' filename.txt
```


### Search in multiple files

```bash
grep 'word' file1 file2 file3
```


### Search for a String in a Directory on the Server

You can search recursively i.e. read all files under each directory for a string "192.168.1.1"

```bash
grep -r "192.168.1.1" /etc/
```


- - -


## File/Folder Manipulation


### Move Files to Parent Directory

From within the directory you want to move all the files up a level, run:

```bash
mv * .[^.]* ..
```


### Change Ownership

Change owner and group of files -r (recursively) to the path

```bash
chown user:group -r /some/path
```


### Update Permissions for ALL Directories and Files in a Path

```bash
find * -type d -print0 | xargs -0 chmod 0755  # for directories
find . -type f -print0 | xargs -0 chmod 0644  # for files
```


### Copy a Directory

Copy a directory and all files and preserve permissions, ownership, etc (-a)

```bash
cp -a /path/old_directory /path/new_directory
```


### Delete a Directory

Delete a directory ‘example’ recursively (-r) and force/do not prompt (-f). BE EXTREMELY CAREFUL...

```bash
rm -rf example
```


### Delete Files that Start with “xyz” in a Directory

```bash
find /var/www/example/ -name “xyz*" -type f -delete
```


### Delete LOTS of Files in a Directory

Navigate to desired directory… commands says to list files by updated date, and run xargs to execute ‘rm' command on each line

```bash
ls -U | xargs rm
```


- - -


## Lookups


### Lookup Size of Directory

Disk-Usage: returns the summary (-s) and size (-h) in an easy to ready format

```bash
du -sh /var/www/example
```


### Lookup Current Server’s IP

Returns the public IP of the server you are on

```bash
curl ifconfig.me
```


### Lookup DNS Information on a Domain

Returns the Nameservers and DNS records for the domain

```bash
dig mydomain.com ANY
```


### Lookup Users Currently Logged into a Server

```bash
last
```


### Lookup All User Accounts on a Server

```bash
cut -d: -f1 /etc/passwd
```


- - -


## Apache2


### Check Error Log

View the last 40 lines of ‘error.log’ in Apache

```bash
tail -n 40 /var/log/apache2/error.log
```


### Apache2 Enable VirtualHost

You must reload Apache2 (see below) after enabling or editing a Virtual Host file.

```bash
a2ensite example.com.conf
```


### Apache2 Disable VirtualHost

```bash
a2dissite example.com.conf
```


### Reload Apache2

```bash
service apache2 reload
```


- - -


## Miscellaneous


### Increase SFTP Connection Limits

In `sftp_config`, locate `LimitConnectionByUser` and change the limit to 5.

```bash
nano /etc/ssh/sftp_config
```


### Test for Shellshock Vulnerability on the Server

```bash
env x='() { :;}; echo vulnerable' bash -c 'echo hello'
```


### Scan for Common Hacks

```bash
grep -Rln "<?php eval(" .
```
