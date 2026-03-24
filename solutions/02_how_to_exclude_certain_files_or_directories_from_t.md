# How to exclude certain files or directories from the rsync backup

_Use the --exclude option with rsync to exclude files or directories from the backup_

## Steps

1. Create a new file for the exclude list, for example `sudo nano /etc/backup/exclude.txt`
2. Add the files or directories to exclude, one per line, to the exclude.txt file
3. Modify the rsync command in the backup.service file to include the --exclude option, for example `ExecStart=/usr/bin/rsync -avz --exclude-from=/etc/backup/exclude.txt /source/directory/ /destination/directory/`
4. Reload the systemd daemon and restart the timer by running the commands `sudo systemctl daemon-reload` and `sudo systemctl restart backup.timer`
5. Verify that the excluded files or directories are not included in the backup by running the command `rsync -avz --exclude-from=/etc/backup/exclude.txt /source/directory/ /destination/directory/`
6. Test the backup by running the command `sudo systemctl start backup.service` and verifying the output

## Pitfalls

- Not using the correct syntax for the --exclude option