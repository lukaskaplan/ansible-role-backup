# Backup role

Used for automatic backup deployment. It works so that the backup 
server downloads backups from other servers. The role prepares both 
the server side and the necessary one configuration of all backed up servers.

For its function, it uses the [rdiff-backup] utility, which makes differential 
backups and that backups are versioned. It uses ssh connection (like rsync), 
to transfer files.

## Function description

- The role creates a backup user + ssh key on the backup server.

- At the same time in `/home/backup` creates directories for individual backup servers.

- Copies the ssh key under /root/.ssh/authorized_keys to the backed up servers (and sets 
restricted permissions to run rdiff-backup only).

- Finally, it sets up CRON on the backup server for downloading backups and for deleting
old versions of backups.


## Use
See variables in `defaults/main.yml`

## Attention

- If you will change list of `backup_folders`, change will be done - but old (removed) backed up directories will remain on the backup server.

- Delete the backed up server from the backup must be done manually - by deleting cron
jobs on the backup server.

- Rdiff-backup does not follow symlinks to prevent loops. It is therefore necessary
list all original folders and files in `backup_folders`.

[rdiff-backup]: <https://rdiff-backup.net/>
